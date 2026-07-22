# Entendendo o Event Sourcing

O **Event Sourcing** é um padrão arquitetural de software onde as mudanças de estado de uma aplicação são armazenadas como uma sequência de eventos imutáveis, em vez de armazenar apenas o estado atual (como acontece no modelo tradicional de banco de dados CRUD).

Nesta arquitetura, a **fonte da verdade** não é uma tabela mostrando como o dado está agora, mas sim um registro (log) de tudo o que aconteceu para que o dado chegasse ao estado atual.

---

## Como Funciona? (Event Sourcing vs. CRUD Tradicional)

### A Abordagem Tradicional (CRUD)
Em um sistema tradicional, se você tem uma conta bancária com saldo de $100 e deposita $50, o sistema vai no banco de dados e **sobrescreve** o valor do saldo para $150. A informação de que havia $100 antes da operação de depósito acaba sendo perdida ou depende de tabelas de log secundárias para ser recuperada.

### A Abordagem Event Sourcing
No Event Sourcing, nós não guardamos o "saldo atual" como fonte principal da verdade. Nós armazenamos os **eventos** que ocorreram. 
Para a mesma conta bancária, o banco de dados armazenaria:
1. `ContaCriada` (Saldo inicial: 0)
2. `DepositoRealizado` (Valor: 100)
3. `DepositoRealizado` (Valor: 50)

Para saber o saldo atual, a aplicação **reproduz (replays)** os eventos desde o início (0 + 100 + 50 = 150).

---

## Conceitos Principais

1. **Eventos Imutáveis:** Um evento representa algo que *já aconteceu* no passado (por isso o nome geralmente é no passado, ex: `UserRegistered`, `OrderShipped`). Como já aconteceu, **nunca pode ser apagado ou alterado**.
2. **Append-Only Store:** O banco de dados de eventos (Event Store) é apenas de inserção (*append-only*). Não existem operações de `UPDATE` ou `DELETE`. Para reverter uma ação, você adiciona um evento de compensação (ex: `TransferenciaEstornada`).
3. **Projeções e Read Models (CQRS):** Como calcular o estado atual a partir de milhares de eventos pode ser lento, os sistemas de Event Sourcing frequentemente utilizam **Projeções**. Uma projeção ouve os eventos e cria uma "visão" atualizada (estado atual) em um banco de dados rápido para leitura. Isso caminha lado a lado com o padrão **CQRS** (Command Query Responsibility Segregation).

---

## Exemplo Prático de Código (Conceitual)

Imagine como seriam os eventos em uma aplicação PHP/Laravel:

```php
// 1. O que é armazenado no banco de dados (Event Store)
[
    ['id' => 1, 'type' => 'ContaCriada', 'payload' => ['conta_id' => 'A123', 'titular' => 'Arthur']],
    ['id' => 2, 'type' => 'DepositoRealizado', 'payload' => ['conta_id' => 'A123', 'valor' => 500]],
    ['id' => 3, 'type' => 'SaqueRealizado', 'payload' => ['conta_id' => 'A123', 'valor' => 200]],
]

// 2. A lógica para reconstruir o estado (Projeção / AggregateRoot)
class ContaBancaria
{
    public float $saldo = 0;

    public function aplicar(array $eventos)
    {
        foreach ($eventos as $evento) {
            match ($evento['type']) {
                'ContaCriada' => $this->saldo = 0,
                'DepositoRealizado' => $this->saldo += $evento['payload']['valor'],
                'SaqueRealizado' => $this->saldo -= $evento['payload']['valor'],
            };
        }
    }
}

// 3. Uso
$conta = new ContaBancaria();
$conta->aplicar($eventosDoBanco);
echo $conta->saldo; // Resultado: 300
```

---

## Vantagens
* **Trilha de auditoria infalível:** Essencial para softwares de missão crítica.
* **Escalabilidade (com CQRS):** Separação total de quem escreve os dados (eventos) e de quem lê os dados (projeções).
* **Análise de Dados Futura:** Você pode criar novas projeções hoje e rodar sobre os eventos do passado para descobrir novas informações que você nem sabia que precisava antes.

## Material complementar
- [Artefato do Claude](https://claude.ai/public/artifacts/03a03bca-34c0-41c8-995a-6368a915a56d)

- [WHAT IS EVENT SOURCING?](https://eda-visuals.boyney.io/visuals/what-is-eventsourcing)