# Catálogo de Conceitos e Contratos (`DUP-01`) — calculadora-rtc-gerador-danfse

Regra `DUP-01`: **Um conceito = uma casa.** Consulte antes de criar entidade, módulo ou utilitário (`DUP-02`).

| Conceito | Pacote/Domínio Dono | Entidade/Struct Mestra | Contrato Público | Observação |
| --- | --- | --- | --- | --- |
| Geração do PDF | `internal/service` | `ServicoGeracaoPdfDanfseDireto` | `Executar(ctx, xml, situacao, canhoto)` | Núcleo único de HTTP e WASM (ADR005) |
| Política de entrada do XML | `internal/service` | `xml_security.go` | validação antes da preparação | Mesma regra nos dois ambientes (ADR006) |
| Preparação da nota | `internal/service` | `PreparadorDanfse` | `Executar(ctx, xml, situacao)` | — |
| Apresentação do documento | `internal/service` | `ApresentadorDanfse` | `Apresentar(nota)` | Totais em `danfse_tax_totals.go` |
| Desenho do PDF | `internal/renderer` | `RenderizadorPdfDireto` | `Renderizar(ctx, documento)` | Folio (ADR003) |
| Situação da nota | `internal/domain/enums` | `SituacaoNotaTipo` | `SituacaoNotaPorSTC(stc)` | 0 válida, 1 substituída, 2 cancelada |
| Catálogo de localidades | `internal/domain/localidade` | `localidades_generated.go` | consulta em memória | Gerado por `cmd/gen-localidades` (ADR002) |
| Configuração | `internal/config` | `env.go` | leitura das variáveis de ambiente | Valor inválido rejeita a inicialização |
| Erro HTTP padrão | `internal/controller` | `exception_handler.go`, `error_response.go` | corpo de erro único | ADR004 |
| Readiness | `internal/controller` | `Readiness` | `/health/ready` | ADR007 |
| Telemetria | `internal/telemetry` | `telemetry.go` | inicialização opt-in | ADR007 |
