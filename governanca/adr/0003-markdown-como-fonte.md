# ADR-0003 — Markdown como formato-fonte

**Data:** 2026-07-30
**Status:** Aceito
**Decisores:** Márccio (PO) · ChatGPT · Claude

## Contexto
Documentos centrais do SIE estavam em `.docx` e `.xlsx`. O Git não faz diff de binário: não há histórico legível de mudanças, e cada versão salva duplica o arquivo inteiro no repositório.

## Decisão
Markdown é o formato-fonte de todo conhecimento versionado. Binários são **entregável**, não fonte, e permanecem no OneDrive.

## Alternativas consideradas
| Opção | Prós | Contras | Por que não |
|---|---|---|---|
| Manter `.docx` como fonte | Familiar, formatação rica | Sem diff, incha o repositório | Impede revisão de mudanças |
| Git LFS para binários | Reduz peso | Custo e complexidade | Não resolve a falta de diff |

## Consequências
- Histórico legível: dá para ver exatamente o que mudou em cada versão
- Repositório leve
- Documentos para cliente continuam sendo gerados em Word — a partir do markdown
- Exige migração dos documentos existentes (Fase 3)

## Revisão
Se surgir necessidade de formatação que markdown não suporte em documento vivo.
