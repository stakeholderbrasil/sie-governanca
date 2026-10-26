# ADR-0001 — Monorepositório único e privado

**Data:** 2026-07-30
**Status:** Aceito · **restrito parcialmente pelo [ADR-0010](0010-repositorio-do-novo-site.md)** — permanece válido para conhecimento e governança; deixa de valer para código publicável, que vive em repositório próprio
**Decisores:** Márccio (PO) · ChatGPT · Claude

## Contexto
O conhecimento do SIE estava disperso entre conversas de IA, pastas do OneDrive e a memória local do Claude. Nenhum repositório Git existia. Volume atual: dezenas de documentos, não milhares.

## Decisão
Um único repositório privado (`sie`) concentra código, documentação, conhecimento e versionamento.

## Alternativas consideradas
| Opção | Prós | Contras | Por que não |
|---|---|---|---|
| Múltiplos repositórios | Isolamento por domínio | Coordenação, referências cruzadas quebram | Sobrecarga sem benefício no volume atual |
| Repositório público | Transparência | Expõe estratégia comercial e dados de cliente | Inaceitável |
| Sem repositório | Zero esforço | Conhecimento continua volátil | Contraria o Princípio 1 |

## Consequências
- Um ponto de busca, backup e histórico
- Referências entre conhecimento, playbooks e produtos funcionam nativamente
- O repositório cresce com conteúdo heterogêneo — exige disciplina de estrutura
- Extrair um produto para repositório próprio depois é simples; fundir seria caro

## Revisão
Se um produto passar a ter pipeline de deploy próprio e ciclo independente.
