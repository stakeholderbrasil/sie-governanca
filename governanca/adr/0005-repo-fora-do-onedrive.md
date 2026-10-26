# ADR-0005 — Repositório fora de pasta sincronizada

**Data:** 2026-07-30
**Status:** Aceito
**Decisores:** Claude (recomendação) · Márccio (aprovação)

## Contexto
O caminho natural seria a própria pasta sincronizada em nuvem, onde os arquivos já estavam. Git em pasta de sincronização em nuvem é anti-padrão conhecido.

## Decisão
Repositórios ficam em `~/Development/`, fora de qualquer sincronização.
O OneDrive permanece para documentos de trabalho e binários.

## Alternativas consideradas
| Opção | Prós | Contras | Por que não |
|---|---|---|---|
| Repo no OneDrive | Tudo num lugar | Corrompe `.git`, gera conflitos, lentidão | Risco real de perda de histórico |
| OneDrive com `.git` excluído da sync | Mantém junto | Config manual que se perde ao reinstalar | Frágil |

## Consequências
- `.git` protegido de corrupção por sincronização concorrente
- GitHub assume o papel de backup — que é o dele
- Dois locais para conhecer: repositório e OneDrive
- Estrutura padrão criada: `Development/{sie,tools,templates,sandbox,archive}`

## Revisão
Não se aplica. Restrição técnica do Git com sincronizadores de arquivo.
