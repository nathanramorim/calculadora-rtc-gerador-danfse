# Fluxos — calculadora-rtc-gerador-danfse

## 1. Gerar o DANFSE por HTTP (`POST /api/danfse?stc=&canhoto=`)
```
XML da NFS-e no corpo (limite XML_MAX_SIZE_BYTES, padrão 5 MiB)
  ↓ BodyLimit e RequestLogger (request_id gerado pelo servidor)
Controller: lê stc (0 válida, 1 substituída, 2 cancelada) e canhoto (padrão true)
  ↓
Preparação: política de entrada do XML (ADR006) → modelo da nota + localidades
  ↓
Apresentação: documento com totais e campos formatados
  ↓
Renderização direta com Folio
  ↓
PDF na resposta (erro: formato padrão, 400 para entrada inválida)
```

## 2. Gerar o DANFSE no navegador (WebAssembly)
```
JavaScript → { xml, stc, canhoto }
  ↓ mesmo núcleo de internal/service (ADR005), limite padrão de 5 MiB
{ pdf (base64), size }
```

## 3. Política de entrada do XML (ADR006)
Tamanho máximo → rejeita DTD e entidades → valida estrutura mínima → corrige `&` não escapado de forma controlada. A mesma política vale em HTTP e WASM.

## 4. Health e encerramento (ADR007)
`/health/live` responde 200 enquanto o processo vive. `/health/ready` responde 200 quando pronto e 503 no graceful shutdown; o estado vira "não pronto" antes de aguardar as requisições em andamento.

## 5. Observabilidade
Um log JSON `request_completed` por requisição em stderr, sem XML, PDF, querystring nem dados fiscais. Traces e métricas OTLP só com `OTEL_SDK_DISABLED=false`; configuração OTel inválida falha a inicialização.

## 6. Tratamento de erro
Exceção ou falha sobe ao tratador central, que devolve o corpo padrão. 5xx é sempre genérico (não expõe `err.Error()`).

## 7. Atualização do catálogo de localidades
`cmd/gen-localidades/data/localidades.json` → `task generate-localidades` → `localidades_generated.go` → compilação (ADR002).
