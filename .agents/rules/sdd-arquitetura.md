---
description: Como cumprir as diretrizes de arquitetura, camadas, núcleo compartilhado entre HTTP e WASM e declaração de modelo de módulo.
globs: ["cmd/**", "internal/**", "sdd/spec/**"]
regras: [ARQ-01, ARQ-02, ARQ-03, ARQ-04, ARQ-05, ARQ-06, ARQ-11, ARQ-12, ARQ-13]
---

# Guia Prático — Arquitetura e Fronteiras de Módulos

Instruções operacionais para os IDs `ARQ-*` de `sdd/memory/constitution.md` (sem reproduzir o texto normativo, `GOV-04`).

## 1. Pontos de entrada e camadas (`ARQ-01`, `ARQ-02`, `ARQ-12`)
- `cmd/danfse` e `cmd/danfse-wasm` só leem configuração e entrada, chamam `internal/` e formatam a saída. Regra de negócio nova entra em `internal/service`, `internal/renderer` ou `internal/domain`, nunca só em um `main`.
- Direção das dependências: `cmd` → `internal/app` → `controller`/`service` → `renderer`/`domain`. `domain` não importa `service`, `controller` nem `renderer`. Sem ciclos.
- A composição das dependências é manual, em `internal/app`, por construtor (ADR001). Não adicione container de injeção.
- Código com `syscall/js` fica sob `//go:build js && wasm`.

## 2. Recursos, dependências e estado (`ARQ-03`, `ARQ-04`, `ARQ-05`)
- Configuração só por variável de ambiente, lida em `internal/config`; valor inválido rejeita a inicialização.
- Logo e catálogo de localidades vão embutidos no binário. Mudou `cmd/gen-localidades/data/localidades.json`: rode `task generate-localidades` e comite o arquivo gerado (ADR002).
- Antes de nova dependência no `go.mod`, verifique se a `stdlib` atende. Ela também precisa compilar para `js/wasm` (`task build-wasm`).
- O serviço não grava XML, PDF nem dados fiscais em disco.

## 3. Contrato HTTP (`ARQ-06`)
- Rota nova ou mudada exige anotação swag e `task swagger` para atualizar `api/swagger/`. Mudança incompatível pede ADR.
- Erros saem pelo tratador central (`internal/controller/exception_handler.go`); handlers não montam resposta de erro própria (ADR004).

## 4. Entrada XML (`ARQ-13`)
- Todo XML de entrada passa por `internal/service/xml_security.go` antes da preparação, em HTTP e em WASM (ADR006).

## 5. Declaração de modelo na Spec (`ARQ-11`)
- Toda spec traz `## Modelo do módulo`: **`enxuto`** para ajuste de rótulo, layout, texto ou documentação; **`completo`** para regra nova na preparação ou no cálculo de totais, mudança de contrato HTTP ou da política de entrada.
