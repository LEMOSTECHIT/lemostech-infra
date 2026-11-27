# lemostech-infra

Workflows reutilizáveis do GitHub Actions para os repositórios da LemosTech.
Um lugar para mudar o pipeline, todos os repositórios seguem — desde que apontem
para uma **tag**, nunca para `main`.

## Por que existe

Cada aplicação tinha (ou teria) a mesma sequência copiada: instalar Python,
instalar dependências, rodar lint, rodar teste, às vezes com cobertura. Cópia de
pipeline significa que corrigir um problema exige editar N repositórios — e que
cada um fica diferente do outro com o tempo.

Aqui a lógica vive uma vez. O repositório da aplicação só declara **o que é dele**:
onde está o código, qual arquivo de dependências, qual comando de teste.

## O que tem aqui

| Arquivo | Papel |
|---|---|
| `.github/workflows/ci-python.yml` | CI reutilizável: instala dependências, roda Ruff (check e formato) e pytest, com cobertura opcional |
| `.github/workflows/cd-deploy-receptor.yml` | CD reutilizável: chama o receptor de deploy do servidor (`deploy.lemostech.dev.br`) e exige HTTP 202 |
| `.github/workflows/lint-workflows.yml` | Confere os próprios workflows com `actionlint` a cada push |
| `exemplos/itsm/` | Como o ITSM fica depois da migração (piloto) |
| `docs/guia-uso.md` | Regras de uso, versionamento e o que não deve ser centralizado |

## Uso rápido

No repositório da aplicação:

```yaml
# .github/workflows/ci.yml
name: CI
on:
  push:
    branches: [main]
  pull_request:

jobs:
  ci:
    uses: LEMOSTECHIT/lemostech-infra/.github/workflows/ci-python.yml@v1
    with:
      python-version: "3.12"
      requirements-file: requirements.txt
      coverage-min: 70
      coverage-source: apps
```

## Regra de ouro

**Nunca aponte para `@main`.** Um commit errado aqui quebraria o pipeline de
todos os repositórios de uma vez. Use tag (`@v1`), teste a mudança num
repositório piloto e só então mova a tag.

## Estado

Criado em 27/11/2026. Piloto: ITSM. Nada foi publicado no GitHub ainda.
