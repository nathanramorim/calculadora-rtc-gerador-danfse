# Stack — calculadora-rtc-gerador-danfse

## Dependências

| Camada | Escolha | Versão | Motivo |
|--------|---------|--------|--------|
| Linguagem | Go | 1.27 | `go.mod`, `Dockerfile` (golang 1.27.1) |
| HTTP | Echo v5 | 5.3.1 | ADR004 |
| PDF | Folio (`carlos7ags/folio`) | 0.10.1 | ADR003 |
| API docs | swag + echo-swagger v2 | 1.16.6 / 2.0.1 | `task swagger` |
| Observabilidade | OpenTelemetry (traces e métricas OTLP HTTP) + `echo-otel` | 1.46.0 | ADR007 |
| Logs | `log/slog` JSON em stderr | stdlib | ADR007 |
| Tarefas | Task (`Taskfile.yml`) | opcional | — |
| Contêiner | Docker (build golang, imagem final alpine) | — | `Dockerfile` |
| Banco | Nenhum | — | ADR002 |

## Build e testes
- `task check`: `go fmt ./...` e `go vet ./...`.
- `task test`: `go test -race -count=1 ./...`.
- `task coverage`: cobertura por função e `coverage.html`.
- `task build`: binário em `bin/danfse-service`; `task build-wasm`: `bin/danfse.wasm` e `wasm_exec.js`.
- `task generate-localidades`: regenera `localidades_generated.go` a partir do JSON.
- CI: `.github/workflows/go-coverage.yml` (gofmt, vet, testes com race, meta de 70% de cobertura).

## Layout do projeto
```
cmd/
  danfse/            servidor HTTP
  danfse-wasm/       ponto de entrada WebAssembly (build js/wasm)
  gen-localidades/   gerador do catálogo (JSON → Go)
internal/
  app/               composição manual das dependências (HTTP e PDF)
  config/            variáveis de ambiente
  controller/        rotas, health, logging, tratamento de erros
  service/           preparação, apresentação e geração do PDF; segurança do XML
  renderer/          desenho do PDF (layout, estilo, texto, assets)
  domain/            enums, localidade, models, presentation, utils
  telemetry/         SDK OpenTelemetry opt-in
api/swagger/         docs.go, swagger.json e swagger.yaml (gerados)
docs/adr/            ADR001 a ADR007
```
