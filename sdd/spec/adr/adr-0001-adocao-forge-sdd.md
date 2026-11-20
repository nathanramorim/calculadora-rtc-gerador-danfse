# ADR-0001: Adoção do Forge-SDD e alinhamento da constituição

- **Data:** 2026-11-20
- **Status:** `proposto`
- **Regras afetadas (IDs da constitution):** `ARQ-01`, `ARQ-05`, `ARQ-12`, `ARQ-13`, `GOV-01`
- **Autor / Responsável:** Nathan Ramos Amorim

## Contexto

O repositório já tem governança (`GOVERNANCE.md`, `CONTRIBUTING.md`) e sete ADRs em `docs/adr/`. O Forge-SDD v5.0.0 foi instalado depois. Seus modelos de constituição e de rules vieram escritos para o próprio Forge (Cobra, `embed.FS`, `--dry-run`, golden files), que não existem neste serviço.

## Opções Consideradas

1. **Copiar os ADRs para `sdd/spec/adr/`:** duplica a fonte e quebra `DUP-01`.
2. **Manter `docs/adr/` como fonte e referenciá-la:** uma casa por decisão; `sdd/spec/adr/` guarda só decisões do método.

## Decisão

Opção 2. A constituição e as rules em `.agents/rules/` foram adaptadas a este serviço. Regras alteradas: `ARQ-01` (pontos de entrada enxutos, lógica em `internal/`), `ARQ-05` (serviço sem estado, sem gravar dados fiscais); regras criadas: `ARQ-12` (mesmo núcleo em HTTP e WASM) e `ARQ-13` (toda entrada XML passa pela política de validação). O processo da `GOVERNANCE.md` §4.2 continua valendo para mudança arquitetural.

## Consequências

- **Positivas:** uma fonte por decisão; rules coerentes com o código.
- **Negativas / Trade-offs:** duas pastas de ADR exigem o índice em `sdd/spec/decisions.md`.
- **Ações de acompanhamento:** validar com o Líder Técnico; conferir se `forge gate` roda neste projeto.
