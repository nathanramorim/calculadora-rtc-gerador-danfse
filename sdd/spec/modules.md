# Módulos — calculadora-rtc-gerador-danfse

Módulo Go `danfse-service`. Dependências apontam para dentro: `cmd` → `app` → `controller`/`service` → `renderer`/`domain` (`ARQ-02`).

## cmd/danfse e cmd/danfse-wasm
**Responsabilidade:** pontos de entrada. Leem configuração e entrada do ambiente, chamam o núcleo e formatam a saída.
- `danfse`: sobe o servidor HTTP, logger, telemetria e graceful shutdown.
- `danfse-wasm`: recebe `{xml, stc, canhoto}` do JavaScript e devolve `{pdf (base64), size}`.

## internal/app
**Responsabilidade:** composição manual das dependências (ADR001). `application.go` monta o handler HTTP com opções (`WithLogger`, `WithMiddleware`, `WithReadiness`, `WithMaxInputSize`); `pdf_application.go` monta o serviço de PDF.

## internal/controller
**Responsabilidade:** borda HTTP.
- `router.go`: `POST /api/danfse`, `GET /health/live`, `GET /health/ready`, Swagger em `/api/swagger-ui`.
- `danfse.go` (`GerarPdf`), `health.go` (readiness), `logging.go` (`request_completed`), `exception_handler.go` e `error_response.go` (erro padrão, ADR004).

## internal/service
**Responsabilidade:** núcleo de geração.
- `danfse_preparation.go`: lê e enriquece o XML da NFS-e. `xml_security.go`: política de entrada (ADR006).
- `danfse_presenter.go` e `danfse_tax_totals.go`: montam o documento de apresentação.
- `danfse_service.go`: `ServicoGeracaoPdfDanfseDireto.Executar` encadeia preparar, apresentar e renderizar.

## internal/renderer
**Responsabilidade:** desenhar o PDF com Folio (`layout`, `style`, `text`, `labels`, `assets/logo-nfse.png`).

## internal/domain
**Responsabilidade:** modelos e regras sem dependência de infraestrutura.
- `enums` (situação da nota: válida, substituída, cancelada), `localidade` (catálogo gerado, ADR002), `models`, `presentation`, `utils`.

## internal/config e internal/telemetry
**Responsabilidade:** `config` lê e valida variáveis de ambiente (falha na inicialização se inválidas); `telemetry` liga o OpenTelemetry só com `OTEL_SDK_DISABLED=false` (ADR007).
