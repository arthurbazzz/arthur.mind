# CQRS e Projeções

Na hora de puxar essas informações dentro dentro do banco vamos realizar um **replay** (reproduzir) entretanto, se for pegar todos os eventos da conta seria muito lento.

Nesse sentido podemos aplicar o **CQRS** e **Projeções** que são formas de otimizar esse processo.

---

### O que é CQRS?

**CQRS** significa *Command Query Responsibility Segregation* (Segregação de Responsabilidade de Comando e Consulta).

Em arquiteturas tradicionais, usamos o mesmo modelo mental e frequentemente as mesmas classes e tabelas do banco de dados tanto para salvar dados (escrita) quanto para ler dados (leitura).

O que o CQRS propõe: **dividir o sistema em dois lados completamente separados**:

1. **O Lado de Comando (Write / Escrita):**
   * Responsável por alterar o estado do sistema.
   * Processa regras de negócio pesadas e validações.
   * Em uma arquitetura orientada a eventos, esse lado apenas valida as regras e **salva Eventos Imutáveis** no banco de dados (Event Store).
   * *Exemplo: `RealizarDepositoCommand`.*

2. **O Lado de Consulta (Query / Leitura):**
   * Responsável apenas por mostrar os dados para o usuário da forma mais rápida possível.
   * Não processa regras de negócio, apenas devolve dados.
   * *Exemplo: `ObterSaldoContaQuery`.*

A grande sacada do CQRS é que **o banco de dados de leitura não precisa ser o mesmo banco de dados de escrita**. O lado de comando pode salvar em um banco otimizado para logs de eventos, e o lado de consulta pode ler de um banco relacional clássico (como MySQL) ou de documentos (como MongoDB) ou até mesmo de cache (como Redis).

---

## O que são Projeções (Read Models)?

As **Projeções** são a ponte que liga o lado de Comando (os eventos gerados) ao lado de Consulta (os dados prontos para leitura).

Uma Projeção é um pedaço de código (um *listener* ou *worker*) que fica escutando os eventos que são salvos no banco. Toda vez que um novo evento acontece, a projeção atualiza o banco de dados de leitura (Read Model).

### Como funciona o fluxo?

1. O Usuário pede para depositar R$ 100.
2. O sistema valida as regras e salva um evento no Event Store: `DepositoRealizado(100)`.
3. A **Projeção** percebe que um novo evento `DepositoRealizado` aconteceu.
4. A Projeção vai até o banco de dados de leitura (ex: tabela `contas_bancarias` no MySQL) e atualiza o saldo: `UPDATE contas_bancarias SET saldo = saldo + 100 WHERE id = 1;`.
5. Quando o usuário entrar no aplicativo e consultar o saldo, o sistema simplesmente faz um `SELECT * FROM contas_bancarias` super rápido.

Você projeta (daí o nome *Projeção*) os eventos do passado em formatos altamente otimizados para leitura no presente.

---

## ⏳ Consistência Eventual (Eventual Consistency)

Ao separar a escrita da leitura com CQRS e Projeções, você abraça a **Consistência Eventual**.

Isso significa que, quando você salva um evento, a projeção pode levar alguns milissegundos (ou até segundos em sistemas sob alta carga) para processar aquele evento e atualizar a tabela de leitura. 
Durante esse curto período de tempo, se o usuário consultar o saldo, ele ainda verá o saldo antigo. A promessa do sistema não é uma consistência *imediata*, mas sim que *eventualmente* o banco de leitura ficará atualizado.

---

## 💻 Exemplo Conceitual (Em PHP/Laravel)

```php
// ==========================================
// 1. O EVENTO
// ==========================================
class DepositoRealizado {
    public function __construct(
        public string $contaId, 
        public float $valor
    ) {}
}

// ==========================================
// 2. A PROJEÇÃO (Projector)
// Escuta eventos e atualiza o banco de leitura (MySQL)
// ==========================================
class SaldoContaProjector
{
    // Método que é chamado automaticamente quando o evento acontece
    public function onDepositoRealizado(DepositoRealizado $evento)
    {
        // Atualiza a tabela clássica para leitura rápida
        DB::table('contas_leitura')
            ->where('conta_id', $evento->contaId)
            ->increment('saldo', $evento->valor);
    }
}

// ==========================================
// 3. A CONSULTA (Query)
// Extremamente rápida e simples, não sabe o que é evento
// ==========================================
class ContaController
{
    public function obterSaldo($contaId)
    {
        // O lado de leitura apenas busca da tabela projetada!
        $conta = DB::table('contas_leitura')->find($contaId);
        
        return response()->json(['saldo' => $conta->saldo]);
    }
}
```

## Por que usar isso?

1. **Escalabilidade Extrema:** Em muitos sistemas, você lê dados 100 vezes mais do que escreve. O CQRS permite que você escale seus servidores de banco de dados de leitura de forma independente da escrita.
2. **Consultas Complexas Simplificadas:** Você não precisa fazer `JOINs` complexos entre 15 tabelas. Você pode criar uma Projeção que já salva os dados em formato de relatório ou JSON, prontos para a tela do front-end.
3. **Múltiplas Visões (Projeções) dos Mesmos Dados:** A partir de um mesmo evento (ex: `PedidoRealizado`), você pode ter uma projeção que atualiza o *Estoque*, outra que atualiza o *Relatório Financeiro* e outra que alimenta um sistema de *Busca (Elasticsearch)*. Cada um consome o evento e atualiza seu próprio banco.
