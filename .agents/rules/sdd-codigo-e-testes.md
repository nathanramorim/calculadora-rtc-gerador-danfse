---
description: Como manter qualidade de código Go, higiene de pendências e testes executáveis com Task.
globs: ["**/*.go", "**/*_test.go"]
regras: [COD-01, COD-02, COD-04, COD-06, TST-01, TST-02, TST-05]
---

# Guia Prático — Código Limpo e Testes Executáveis

Rotina técnica para `COD-01`, `COD-02`, `COD-04`, `COD-06`, `TST-01`, `TST-02` e `TST-05` (`GOV-04`).

## 1. Código (`COD-01`, `COD-02`, `COD-04`, `COD-06`)
- Go idiomático. `go fmt` obrigatório: o CI falha com arquivo sem `gofmt`.
- Funções com uma responsabilidade. Trate todo erro de forma explícita; não ignore nem "engula" falha (`COD-03`).
- Remova código morto e referências órfãs na mesma entrega. `TODO` só no formato `// TODO(feat-XX): descrição`.
- Verificação ao fim de cada task: `task check` (`go fmt` e `go vet`) sem alertas (`COD-02`).

## 2. Testes (`TST-01`, `TST-02`, `TST-05`)
- Cada critério de aceitação vira teste `Test*` ao lado do código (`*_test.go`). Cubra entrada válida e violações (XML malformado, DTD, `stc` inválido, corpo acima do limite).
- Use `t.TempDir()` e `httptest` quando precisar de isolamento; testes sem dependência de rede nem de relógio (`TST-04`).
- Mudou contrato HTTP ou o PDF gerado: atualize os testes e `api/swagger/` no mesmo PR (`TST-02`).
- A task só vai a `review_pending` com `task test` (`go test -race -count=1 ./...`) e `task check` em código de saída `0`. O CI exige cobertura de 70% (`TST-05`).
