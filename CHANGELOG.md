# Changelog

## v1 — 27/11/2026 (não publicado)

- `ci-python.yml`: workflow reutilizável para projetos Python. Inputs: versão do
  Python, arquivo de dependências, diretório de trabalho, liga/desliga de lint e
  teste, comando de teste, piso de cobertura e pacote medido.
- `cd-deploy-receptor.yml`: workflow reutilizável que chama o receptor de deploy
  do servidor, com validação do SHA e exigência de HTTP 202.
- `lint-workflows.yml`: actionlint em todo push e pull request deste repositório.
- `exemplos/itsm`: piloto de migração do ITSM (CI e CD).
