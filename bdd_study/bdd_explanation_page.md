# Behavior Driven Development (BDD)

O **Behavior Driven Development** (BDD), ou Desenvolvimento Orientado a Comportamento, é uma técnica de desenvolvimento de software ágil que incentiva a colaboração entre desenvolvedores, equipes de qualidade (QA) e pessoas não técnicas ou de negócios em um projeto.

Ele surgiu como uma evolução do **TDD** (Test Driven Development), buscando resolver o problema de comunicação entre as áreas técnicas e de negócios através do uso de uma linguagem comum (Linguagem Ubíqua).

## Principais Conceitos

1. **Foco no Comportamento:** O objetivo não é focar em como o código é implementado (a lógica técnica), mas sim em garantir que o sistema se comporte da maneira esperada pelo usuário final.
2. **Linguagem Comum:** BDD utiliza uma linguagem natural que todos os envolvidos no projeto (desenvolvedores, POs, stakeholders) conseguem entender, evitando mal-entendidos gerados por jargões técnicos.
3. **Documentação Viva:** Os cenários descritos funcionam simultaneamente como especificações de requisitos, testes automatizados e uma documentação do sistema que se mantém sempre atualizada.

## A Estrutura Gherkin (Dado / Quando / Então)

A forma mais comum de escrever os cenários e histórias de usuário em BDD é utilizando a sintaxe **Gherkin**. Ela ajuda a estruturar o comportamento esperado de forma clara, utilizando palavras-chave específicas:

* **Dado (Given):** O contexto inicial. Descreve o estado do sistema antes da ação (pré-condição).
* **Quando (When):** A ação. Descreve o evento que ocorre ou a ação do usuário.
* **Então (Then):** O resultado esperado. A validação do que deve acontecer após a ação.
* **E (And) / Mas (But):** Usados para adicionar mais etapas, ações ou validações de forma legível.

### Exemplo Prático:

```gherkin
Funcionalidade: Autenticação de Usuário

  Cenário: Login com sucesso
    Dado que o usuário está na página de login
    Quando ele insere "usuario@email.com" e a senha "123456"
    E clica no botão "Entrar"
    Então ele deve ser redirecionado para o painel principal
    E a mensagem "Bem-vindo!" deve ser exibida
```

## Benefícios do BDD

* **Alinhamento e Comunicação:** Reduz significativamente o atrito de entendimento entre o que o negócio precisa e o que o time técnico desenvolve.
* **Desenvolvimento Guiado por Valor:** O time foca no que realmente entrega valor para o usuário e para o negócio, evitando código desnecessário.
* **Testes mais Claros:** Quando um teste falha no BDD, é muito fácil identificar qual regra de negócio foi violada.

## Exemplo em Código: BDD com Pest (PHP)

Veja como o cenário de autenticação ilustrado acima ficaria em código Pest:

```php
<?php

use App\Models\User;

describe('Autenticação de Usuário', function () {
    
    it('deve realizar login com sucesso', function () {
        // Dado (Given) que o usuário existe no banco de dados
        $user = User::factory()->create([
            'email' => 'usuario@email.com',
            'password' => bcrypt('123456'),
        ]);

        // Quando (When) ele insere as credenciais e clica no botão Entrar
        $response = $this->post('/login', [
            'email' => 'usuario@email.com',
            'password' => '123456',
        ]);

        // Então (Then) ele deve ser redirecionado para o painel principal
        $response->assertRedirect('/painel');
        
        // E estar devidamente autenticado
        $this->assertAuthenticatedAs($user);
    });

});
```

Dessa forma, o próprio código de teste serve como uma documentação viva e descritiva de como o sistema deve se comportar.


## Material de Apoio
https://medium.com/@racheloliveira07/um-guia-b%C3%A1sico-sobre-bdd-behavior-driven-development-942e0b586e04