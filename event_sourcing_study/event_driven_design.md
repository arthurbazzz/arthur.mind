# Event Driven Design

Devemos pensar de duas formas: no pretérito (evento) e no imperativo (comando).

Quando descrevemos algo no pretérito, estamos narrando um fato que já aconteceu — declarando um evento. Evento não se edita, não se deleta e não se discute.

Já um comando (imperativo) expressa uma **intenção** — como `AprovarCadastro`. Ele pode ser rejeitado, cancelado ou nem chegar a acontecer. Diferente do evento, o comando não é um fato.

****

## Modo Fato (Evento) vs Modo Comando

Antes de qualquer código, o hábito é: toda vez que algo muda no sistema, pergunte-se se é **intenção** ou **fato**.

### Modo Fato — pretérito, imutável

```
Fato: o depósito de R$ 100 foi confirmado
Evento: DepositoConfirmado(valor: 100)
```

```
Fato: o usuário cancelou a assinatura
Evento: AssinaturaCancelada(plano: 'pro')
```

```
Fato: o pedido #882 foi despachado
Evento: PedidoDespachado(transportadora: 'Loggi', rastreio: 'LX123')
```

### Modo Comando — imperativo, intenção

```
Intenção: quero depositar R$ 100 (pode falhar se conta bloqueada)
Comando: Depositar(valor: 100)
```

```
Intenção: quero cancelar minha assinatura (pode falhar se já cancelada)
Comando: CancelarAssinatura()
```

```
Intenção: quero despachar o pedido #882 (pode falhar se pagamento pendente)
Comando: DespacharPedido(pedidoId: 882)
```

****

## Os verbos do domínio: ações no pretérito

Cada ação do sistema vira um evento com nome no passado. Exemplos genéricos:

| Ação | Modo Comando (intenção) | Modo Fato (evento) |
|---|---|---|
| Ação 1 — cadastrar | `CadastrarCliente` | `ClienteCadastrado` |
| Ação 2 — verificar | `VerificarDocumentos` | `DocumentosVerificados` |
| Ação 3 — aprovar | `AprovarLimite` | `LimiteAprovado` |
| Ação 4 — bloquear | `BloquearConta` | `ContaBloqueada` |
| Ação 5 — reativar | `ReativarConta` | `ContaReativada` |

****

## Exemplos no domínio Bancario: trilha de auditoria em eventos

No contexto de compliance, os eventos viram prova documental:

```
1. account.opened → a conta corrente foi aberta
2. transaction.authorized → pagamento de R$ 1.250,00 foi autorizado
3. credit.proposal_evaluated → o analista aprovou o empréstimo com score 780
4. chargeback.initiated → contestação de compra iniciada por fraude alegada
5. chargeback.resolved_merchant → contestação resolvida a favor do estabelecimento com comprovante
6. account.closed → conta encerrada com saldo zerado e motivo registrado
```

Pretérito, imutável, legível: **isto já é uma trilha de auditoria.**

****

## Nomenclatura de eventos

Para a nomenclatura de eventos, usamos a linguagem ubíqua do domínio — nomes que fazem sentido tanto para devs quanto para o time de compliance.

Exemplos de bons nomes de evento:

```php
class AlertTriaged extends ShouldBeStored {}
class SanctionMatchConfirmed extends ShouldBeStored {}
class HitDismissedAsHomonym extends ShouldBeStored {}
```

E seus respectivos comandos:

```php
// Comando (intenção — pode ser recusado)
class TriarAlerta extends Command {}
class ConfirmarMatchDeSancao extends Command {}

// Evento (fato — imutável, já aconteceu)
class AlertTriaged extends ShouldBeStored {}
class SanctionMatchConfirmed extends ShouldBeStored {}
class HitDismissedAsHomonym extends ShouldBeStored {}
```

---

## Material complementar

- [Capítulo 1 - Event driven design](https://claude.ai/public/artifacts/03a03bca-34c0-41c8-995a-6368a915a56d)

