# ADR-0002 — Jira para negócio, GitHub para conhecimento

**Data:** 2026-07-30
**Status:** Aceito
**Decisores:** Márccio (PO) · ChatGPT · Claude

## Contexto
A proposta técnica inicial recomendava contra gestores de tarefa externos, por risco de fragmentar o backlog. O PO e o ChatGPT propuseram manter o Jira, separando por domínio em vez de duplicar.

O Claude revisou a posição: a objeção original valia contra *duplicação*, não contra *especialização*.

## Decisão
**Jira** é a fonte única do backlog — épicos, sprints, comercial, marketing e técnico.
**GitHub** é repositório, documentação e versionamento. **GitHub Issues fica desativado.**
Vínculo por chave do Jira em branch e commit.

## Alternativas consideradas
| Opção | Prós | Contras | Por que não |
|---|---|---|---|
| Só GitHub Issues | Uma ferramenta | Sem burndown, sem campos de negócio; hostil a perfil não técnico | Não sustenta PMO |
| Ambos com Issues ativo | Flexibilidade | Duas caixas de entrada — itens caem entre elas | Fragmentação garantida |
| Só Jira, sem versionar decisões | Simples | Decisão técnica perde rastro | Contraria o Princípio 1 |

## Consequências
- Um backlog, dois papéis bem definidos
- Rastreabilidade bidirecional: do ticket ao código e vice-versa
- Exige disciplina de roteamento — registrada no Princípio 9
- Jira Free limita a 10 usuários (suficiente por anos)
- Automação do Jira depende de OAuth ainda não disponível ao Claude

## Revisão
Se o custo de manter duas ferramentas superar o ganho, ou se a equipe crescer além do plano gratuito.
