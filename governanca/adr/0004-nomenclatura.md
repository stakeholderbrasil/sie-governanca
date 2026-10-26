# ADR-0004 — Nomenclatura em minúsculas, sem acento, com hífen

**Data:** 2026-07-30
**Status:** Aceito
**Decisores:** Claude · aprovado pelo PO

## Contexto
Os arquivos existentes seguiam padrões inconsistentes: `02_ROADMAP_CEO_TERCEIRO_SETOR.md`, `SIC SIE.xlsx`, `HABILIDADES PARA CRIAR SKILL DIVERSAS.docx`. Maiúsculas, espaços, acentos e prefixos numéricos misturados.

## Decisão
Arquivos e diretórios em minúsculas, sem acento, separados por hífen.
`roadmap-terceiro-setor.md`

## Alternativas consideradas
| Opção | Prós | Contras | Por que não |
|---|---|---|---|
| `snake_case` | Legível | Menos comum em documentação e URLs | Convenção de código, não de conteúdo |
| Prefixo numérico | Ordena visualmente | Quebra ao passar de 09; reordenar exige renomear tudo | Frágil |
| Manter como está | Zero esforço | Quebra scripts, links e automação | Custo recorrente |

## Consequências
- Automação e links não quebram
- URLs limpas quando o conteúdo virar site
- Compatível entre Windows, Linux e macOS
- Exige renomear os arquivos existentes na migração

## Revisão
Improvável. Convenção consolidada na comunidade.
