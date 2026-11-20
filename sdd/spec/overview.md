# Overview — calculadora-rtc-gerador-danfse

Serviço em Go, mantido pela Receita Federal e pelo SERPRO, que gera o DANFSE em PDF a partir do XML de uma NFS-e Nacional. O mesmo núcleo roda como servidor HTTP e como WebAssembly. Não usa banco de dados nem navegador: o PDF é desenhado direto com Folio e o catálogo de localidades vai embutido no binário.

## Índice
- `stack.md` — tecnologias e layout
- `modules.md` — componentes
- `flows.md` — fluxos principais
- `decisions.md` — decisões e índice de ADRs
- `concepts.md` — um conceito, uma casa (`DUP-01`)

---

## Arquitetura (C4 Model)

### Nível 1: Contexto
```mermaid
graph TB
  Sist["Sistemas e portais<br/>que emitem NFS-e"] -->|"XML da NFS-e"| Svc["Gerador de DANFSE"]
  Svc -->|"PDF"| Sist
  Nav["Aplicação web"] -->|"WebAssembly"| Svc
  Svc -->|"traces e métricas (opcional)"| Otel["Coletor OTLP"]
```

### Nível 2: Containers
```mermaid
graph LR
  Cli["Cliente HTTP"] --> Http["cmd/danfse<br/>Echo + controller"]
  Js["JavaScript"] --> Wasm["cmd/danfse-wasm"]
  Http --> Nucleo["internal/service<br/>preparar, apresentar, renderizar"]
  Wasm --> Nucleo
  Nucleo --> Dom["internal/domain<br/>enums, localidade, models"]
  Nucleo --> Rend["internal/renderer<br/>Folio"]
```
