# ADR-0006 — Inteligência de empresas como categoria de conhecimento permanente

**Data:** 2026-07-31
**Status:** Aceito
**Decisores:** Márccio (PO) · ChatGPT · Claude

## Contexto
A Stakeholder Trading passa a construir relacionamento com empresas fora dos dois ecossistemas já mapeados em `conhecimento/` (Licitações Públicas, Terceiro Setor). Essas relações geram histórico, pessoas, hipóteses e oportunidades que precisam sobreviver entre sessões e entre as duas IAs — hoje isso viveria só na memória de quem participou da reunião.

## Decisão
Cada empresa em relacionamento ativo ganha um documento vivo em `conhecimento/empresas/{nome-da-empresa}.md`, cobrindo: histórico do relacionamento, pessoas conhecidas, reuniões realizadas, fatos confirmados, hipóteses, oportunidades, riscos, próximos passos e lições aprendidas. Nunca se apaga — atualiza-se.

## Alternativas consideradas
| Opção | Prós | Contras | Por que não |
|---|---|---|---|
| `intelligence/companies/` (topo do repo, inglês) | Isola visualmente do resto | Quebra nomenclatura do ADR-0004 (português, sem acento) | Duas convenções convivendo sem ganho real |
| `inteligencia/empresas/` (topo do repo, português) | Separa fisicamente de `conhecimento/` | Pasta de topo a mais para manter; sobrepõe função que `conhecimento/` já cumpre | `conhecimento/` já é definida como "cresce sempre" — é exatamente esse caso |
| `operacao/clientes/` | Já existe | `operacao/` é definida como datada/perecível (reuniões, casos pontuais) — documento vivo de relacionamento não é perecível | Natureza incompatível |

## Consequências
- `conhecimento/` ganha uma quarta subcategoria, ao lado de licitações, terceiro setor, comercial/marketing
- `ARCHITECTURE.md` seção 4 atualizada para refletir a nova subpasta
- Primeiro documento criado para a primeira empresa em relacionamento ativo

## Revisão
Se o volume de empresas crescer a ponto de precisar de estrutura própria (índice, tags, status de relacionamento), reavaliar promoção para diretório de topo com ADR novo.
