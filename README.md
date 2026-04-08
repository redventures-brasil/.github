# redventures-brasil — Shared Workflows

Repositório central de workflows GitHub Actions reutilizáveis da organização **Red Ventures Brasil**.

## O que o Claude consegue fazer

Mencione `@claude` em qualquer comentário de PR ou issue para acionar o agente:

| Comando | Onde usar | O que acontece |
|---|---|---|
| `@claude revisa esse PR` | Comentário de PR | Analisa bugs, segurança, performance, testes e legibilidade |
| `@claude implementa X` | Issue ou PR | Explora o repositório, implementa a mudança e commita |
| `@claude corrige o bug descrito aqui` | Issue ou PR | Localiza o problema no código e aplica a correção |
| `@claude o que faz essa função?` | Comentário de PR | Responde perguntas sobre o código com base nos arquivos reais |
| `@claude triagem` | Issue | Classifica por tipo, severidade, componente e sugere próximos passos |

## Como usar em um repositório

Crie `.github/workflows/claude.yml` no repositório consumidor:

```yaml
name: Claude

on:
  pull_request_review_comment:
    types: [created]
  issue_comment:
    types: [created]
  issues:
    types: [opened]

jobs:
  claude:
    uses: redventures-brasil/.github/.github/workflows/claude-review.yml@main
    secrets:
      ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
```

## Configuração avançada

Use `with:` para customizar o comportamento por repositório:

| Input | Padrão | Descrição |
|---|---|---|
| `model` | `claude-sonnet-4-20250514` | Modelo Claude a usar |
| `max_turns` | `200` | Máximo de turnos de conversação por execução |
| `review_language` | `pt-BR` | Idioma das respostas: `pt-BR`, `en`, `es` |
| `extra_instructions` | `""` | Instruções adicionais específicas do repositório |

**Exemplo com contexto de negócio:**

```yaml
jobs:
  claude:
    uses: redventures-brasil/.github/.github/workflows/claude-review.yml@main
    with:
      extra_instructions: |
        Este é um serviço de pagamentos. Priorize revisões de segurança,
        especialmente validação de inputs financeiros e proteção de dados
        sensíveis (PII/PCI). Verifique conformidade com LGPD quando relevante.
    secrets:
      ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
```

## Secret necessário

O secret `ANTHROPIC_API_KEY` deve estar configurado no repositório consumidor ou herdado das secrets da organização.

Documentação: [Using secrets in GitHub Actions](https://docs.github.com/en/actions/security-guides/using-secrets-in-github-actions)
