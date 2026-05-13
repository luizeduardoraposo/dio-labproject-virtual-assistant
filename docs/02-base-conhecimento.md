# Base de Conhecimento

## Dados Utilizados

O MoneyGuide utiliza os arquivos da pasta `data` como base inicial para personalizar recomendações, contextualizar o histórico do cliente e justificar sugestões com dados estruturados.

| Arquivo | Formato | Utilização no Agente |
|---------|---------|---------------------|
| `historico_atendimento.csv` | CSV | Recuperar atendimentos anteriores, temas recorrentes e contexto de suporte já resolvido |
| `perfil_investidor.json` | JSON | Identificar perfil de risco, renda, patrimônio, objetivos e metas financeiras do cliente |
| `produtos_financeiros.json` | JSON | Filtrar produtos compatíveis com perfil, risco aceito e objetivo principal |
| `transacoes.csv` | CSV | Analisar entradas e saídas, identificar padrões de consumo e apoiar alertas de orçamento e liquidez |

> [!TIP]
> **Quer um dataset mais robusto?** Você pode utilizar datasets públicos do [Hugging Face](https://huggingface.co/datasets) relacionados a finanças, desde que sejam adequados ao contexto do desafio.

---

## Adaptações nos Dados

> Você modificou ou expandiu os dados mockados? Descreva aqui.

Os dados mockados foram mantidos em formato simples e legível para facilitar a prototipação do agente. A principal adaptação foi estruturar a base para refletir um cenário plausível de atendimento financeiro pessoal:

- O arquivo `perfil_investidor.json` concentra informações centrais do cliente, como renda mensal, perfil moderado, metas e reserva de emergência atual.
- O arquivo `transacoes.csv` organiza movimentações por data, descrição, categoria, valor e tipo, permitindo cálculos de fluxo de caixa e classificação de despesas.
- O arquivo `produtos_financeiros.json` reúne opções de investimento com risco, aporte mínimo e indicação de uso, o que facilita recomendações coerentes com o perfil do cliente.
- O arquivo `historico_atendimento.csv` registra interações anteriores para dar continuidade ao relacionamento e evitar respostas desconectadas do contexto.

Em uma evolução futura, essa base pode ser expandida com mais meses de transações, múltiplos perfis de clientes e regras de elegibilidade para produtos.

---

## Estratégia de Integração

### Como os dados são carregados?
> Descreva como seu agente acessa a base de conhecimento.

Os arquivos JSON e CSV são carregados no início da sessão ou no momento da consulta, dependendo do fluxo da aplicação. O agente usa:

- `perfil_investidor.json` para montar o contexto principal do cliente.
- `transacoes.csv` para calcular receitas, despesas e padrões recentes.
- `produtos_financeiros.json` para cruzar perfil e objetivo com produtos compatíveis.
- `historico_atendimento.csv` para recuperar dúvidas anteriores e manter continuidade no atendimento.

Essa abordagem permite combinar contexto estático do cliente com dados transacionais e histórico conversacional.

### Como os dados são usados no prompt?
> Os dados vão no system prompt? São consultados dinamicamente?

Os dados são consultados dinamicamente e inseridos no contexto da conversa conforme a intenção do usuário. O system prompt define o papel do MoneyGuide, suas limitações e regras de segurança, enquanto os dados da base de conhecimento entram como contexto estruturado para fundamentar a resposta.

Exemplos de uso no prompt:

- Perfil do investidor para limitar recomendações ao risco aceito.
- Transações recentes para detectar excesso de gastos ou risco de saldo baixo.
- Histórico de atendimento para recuperar dúvidas já tratadas.
- Produtos financeiros para sugerir apenas alternativas compatíveis com o objetivo do cliente.

Com isso, o agente evita respostas genéricas e mantém explicabilidade ao justificar recomendações com base em dados objetivos.

---

## Exemplo de Contexto Montado

> Mostre um exemplo de como os dados são formatados para o agente.

```
Dados do Cliente:
- Nome: João Silva
- Perfil: Moderado
- Idade: 32
- Profissão: Analista de Sistemas
- Renda mensal: R$ 5.000,00
- Patrimônio total: R$ 15.000,00
- Reserva de emergência atual: R$ 10.000,00
- Objetivo principal: Construir reserva de emergência
- Aceita risco: Não

Metas:
- Completar reserva de emergência: R$ 15.000,00 até 2026-06
- Entrada do apartamento: R$ 50.000,00 até 2027-12

Últimas transações:
- 01/10: Salário - R$ 5.000,00
- 02/10: Aluguel - R$ 1.200,00
- 03/10: Supermercado - R$ 450,00
- 05/10: Netflix - R$ 55,90
- 07/10: Farmácia - R$ 89,00

Histórico de atendimento:
- 15/09: Dúvida sobre CDB
- 01/10: Explicação sobre Tesouro Selic
- 12/10: Acompanhamento da meta de reserva de emergência

Produtos compatíveis com o perfil:
- Tesouro Selic
- CDB Liquidez Diária
- Fundo Multimercado
```
