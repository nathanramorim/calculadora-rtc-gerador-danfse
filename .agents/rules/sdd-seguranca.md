---
description: Como aplicar validação de entrada XML, higiene de segredos e logs e governança de dependências neste serviço Go.
globs: ["**/*"]
regras: [SEG-01, SEG-02, SEG-04, SEG-05, SEG-07]
---

# Guia Prático — Segurança, Segredos e Dependências

Procedimentos para `SEG-01`, `SEG-02`, `SEG-04`, `SEG-05` e `SEG-07` (`GOV-04`).

## 1. Entrada e escrita (`SEG-01`, `SEG-02`)
- Todo XML recebido passa pela política do ADR006: tamanho máximo (`XML_MAX_SIZE_BYTES`, teto de 32 MiB), rejeição de DTD e entidades, estrutura mínima e correção controlada de `&`. Não crie atalho que a contorne, nem no WASM.
- Valide parâmetros na borda: `stc` só 0, 1 ou 2; `canhoto` booleano. Valor inválido responde 400 com mensagem ao cliente.
- Não aceite caminhos de arquivo vindos da requisição. O serviço não escreve dados fiscais em disco.

## 2. Segredos e logs (`SEG-04`, `SEG-05`)
- Sem credenciais, tokens nem XML real em código, testes, commits ou issues. Use exemplos fictícios ou redigidos (`CONTRIBUTING.md`).
- Nunca logar corpo do XML, PDF, querystring, IDs fiscais nem cabeçalhos de autenticação. 5xx devolve corpo genérico, sem `err.Error()`.
- O `X-Request-ID` do cliente é descartado; o servidor gera o seu (ADR007).
- Configuração OTel inválida falha a inicialização com mensagem que não ecoa URL nem credencial. `OTEL_EXPORTER_OTLP_HEADERS` vem só do ambiente.

## 3. Dependências (`SEG-07`)
- Antes de alterar `go.mod`: confirme que a `stdlib` não atende (`ARQ-04`) e que a biblioteca compila para `js/wasm`. Consulte o context7 (se `ativo` em `sdd/memory/mcps.md`) na versão exata.
- Preencha `## Dependências novas` na Spec com módulo, versão, licença e motivo, ou registre `Nenhuma.`.
- Mantenha `go.sum` versionado e o `Dockerfile` coerente com a versão do Go.
