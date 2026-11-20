# Decisões — calculadora-rtc-gerador-danfse

Os ADRs do projeto vivem em `docs/adr/` (fonte única, todos `Aceito`). As decisões do método SDD vivem em `sdd/spec/adr/`.

## Resolvidas

| # | Decisão | Resolução | Motivo | ADR |
|---|---------|-----------|--------|-----|
| D1 | Arquitetura | Camadas com composição manual, sem container de DI | Dependências visíveis | [ADR001](../../docs/adr/ADR001-arquitetura-modular-e-composicao-explicita.md) |
| D2 | Localidades | Catálogo gerado e em memória | Serviço autônomo, sem banco | [ADR002](../../docs/adr/ADR002-catalogo-de-localidades-gerado-e-em-memoria.md) |
| D3 | PDF | Folio direto, sem HTML nem navegador | Mesma saída em servidor e WASM | [ADR003](../../docs/adr/ADR003-renderizacao-direta-de-pdf-com-folio.md) |
| D4 | HTTP e erros | Echo, tratamento centralizado | Consistência para quem consome | [ADR004](../../docs/adr/ADR004-interface-http-e-tratamento-de-erros.md) |
| D5 | Ambientes | Núcleo compartilhado entre HTTP e WASM | Sem regra duplicada | [ADR005](../../docs/adr/ADR005-nucleo-compartilhado-entre-http-e-webassembly.md) |
| D6 | Entrada XML | 5 MiB, sem DTD e entidades, estrutura mínima, correção de `&` | Proteção com compatibilidade legada | [ADR006](../../docs/adr/ADR006-validacao-e-protecao-da-entrada-xml.md) |
| D7 | Observabilidade | Logs JSON, health live/ready, OTel opt-in | Operação em orquestrador | [ADR007](../../docs/adr/ADR007-observabilidade-logs-estruturados-health-e-otel-opcional.md) |
| D8 | Método SDD | Forge-SDD; `docs/adr/` segue como fonte dos ADRs do projeto | Rastreabilidade de spec e portões | [ADR-0001](adr/adr-0001-adocao-forge-sdd.md) |

## Abertas

| # | Questão |
|---|---------|
| D9 | O README diz que erros internos do serviço de geração também viram `400`, e que a separação entre `400` e `5xx` está na fila de melhorias. Falta decidir prazo e contrato. |
| D10 | O `CHANGELOG.md` tem só `[Unreleased]`, mas existe a tag `1.0.0`. Registrar a 1.0.0 no changelog. |
| D11 | O limite de XML do HTTP é configurável (`XML_MAX_SIZE_BYTES`, até 32 MiB), mas o WASM mantém 5 MiB fixos. Confirmar se a diferença é intencional. |
| D12 | `TestRun_BindFalhaRetornaErroSemHang` (`cmd/danfse`) trava até o limite de tempo no macOS: o teste ocupa a porta só em `127.0.0.1` e o servidor sobe mesmo assim. Causa provável (não confirmada no código): o servidor escuta em todas as interfaces; no Linux, onde o CI roda, o teste passa. Medido em `go test -race`: os demais pacotes passaram; `go vet` e a cobertura de 70% não foram rodados. |
