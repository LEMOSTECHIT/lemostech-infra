---
date: 2026-11-27
tags: [projeto, integracao, prioridade]
---
# Guia de uso — lemostech-infra

## Regra 1: tag, nunca `main`

```yaml
uses: LEMOSTECHIT/lemostech-infra/.github/workflows/ci-python.yml@v1   # certo
uses: LEMOSTECHIT/lemostech-infra/.github/workflows/ci-python.yml@main # errado
```

Com `@main`, um commit neste repositório muda o pipeline de **todas** as
aplicações de uma vez, sem revisão em nenhuma delas. Com tag, a mudança é
publicada quando alguém decide mover a tag — e um repositório de cada vez.

## Regra 2: o que fica no repositório da aplicação

| Fica na aplicação | Fica no infra |
|---|---|
| versão do Python | sequência de passos |
| arquivo de dependências | como instalar e cachear |
| comando de teste | como rodar e reportar |
| piso de cobertura | como falhar quando abaixo |
| segredos (deploy, tokens) | uso dos segredos |

O teste é simples: **se o valor muda de aplicação para aplicação, é input**;
se é o mesmo para todas, é código do infra.

## Regra 3: um piloto por vez

1. Move a tag (ou cria `v2`) no infra.
2. Migra **um** repositório — hoje, o ITSM.
3. Roda o CI em pull request, não em `main`, e confere verde.
4. Só depois migra o segundo.

## Como migrar o ITSM (piloto)

```bash
# no repositório itsm
cp .github/workflows/ci.yml ~/backup-ci.yml
cp /caminho/lemostech-infra/exemplos/itsm/ci.yml .github/workflows/ci.yml
git checkout -b ci/reusable-workflow
git commit -am "ci: usa workflow reutilizável do lemostech-infra"
gh pr create --fill
```

O `deploy.yml` só troca depois que o CI estiver verde no PR — o CD depende do CI.

## Nomes de input que existem hoje

`ci-python.yml`: `python-version`, `requirements-file`, `working-directory`,
`run-lint`, `run-tests`, `test-command`, `coverage-min`, `coverage-source`,
`timeout-minutes`.

`cd-deploy-receptor.yml`: `sha`, `origem`; segredos `DEPLOY_URL` e `DEPLOY_SECRET`.

## Checklist antes de mover a tag

- [ ] `actionlint` passou neste repositório
- [ ] Um PR de teste em um repositório migrado ficou verde
- [ ] O CD disparou e o receptor respondeu 202
- [ ] CHANGELOG atualizado com o que mudou
