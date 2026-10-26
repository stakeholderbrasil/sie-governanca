# ADR-0010 — Repositório do novo site: monorepo ou repositório próprio

**Data:** 2026-08-20
**Status:** **Aceito** — aprovado pelo PO em 20/08/2026
**Decisores:** Márccio (PO) · Claude (análise)

## Contexto

O [ADR-0009](0009-replatforming-e-engenharia-multiagente.md) estabelece GitHub como fonte de verdade e Vercel como ambiente de publicação. Falta decidir **onde o código do novo site vive**.

O [ADR-0001](0001-monorepo.md) determinou monorepositório único e privado, com o argumento de um só ponto de busca, backup e histórico. Aquela decisão foi tomada quando o repositório continha apenas documentação.

A situação mudou: o repositório hoje guarda dossiês de empresas, narrativa comercial, evidências de mercado e o mapa dos ativos digitais. Passará a guardar também código publicado na internet.

## Opções

### A · Monorepo — `sie/produtos/site/`
**Prós:** coerente com o ADR-0001; um ponto de histórico; agentes enxergam narrativa, design system e inventário sem troca de contexto; a Vercel suporta subdiretório como raiz de build.
**Contras:** a Vercel recebe acesso de leitura ao repositório inteiro, incluindo inteligência comercial; agentes de desenvolvimento operam com todo o SIE em contexto, o que aumenta custo e amplia o raio de um erro; histórico de conteúdo e de código se misturam.

### B · Repositório próprio — `stakeholderbrasil/site`
**Prós:** isola o que é publicável do que é confidencial; a Vercel só vê o que precisa; escopo do agente naturalmente delimitado; CI e histórico limpos.
**Contras:** contraria o ADR-0001 e exigiria substituí-lo; dois repositórios para manter; design system e narrativa precisam ser referenciados ou duplicados — e duplicar garante divergência.

## Decisão

**Opção B — repositório próprio `stakeholderbrasil/site`.** Aprovada pelo PO em 20/08/2026, com o argumento de superfície de acesso aceito: o novo site não precisa de acesso aos demais ativos e documentos internos do SIE.

O design system é publicado do `sie` para o `site` por cópia controlada e versionada, **nunca por edição paralela** — o `sie` permanece a fonte.

O [ADR-0001](0001-monorepo.md) passa a valer apenas para conhecimento e governança; deixa de valer para código publicável.

O argumento decisivo não é organização, é **superfície de acesso**. O ADR-0001 foi escrito para um repositório de documentação; nenhuma das suas premissas previa entregar acesso de leitura a um serviço de deploy externo. Dossiês de prospects, evidências comerciais e o mapa de ativos não precisam estar ao alcance de um pipeline de build para que o site funcione.

Se aceito, o ADR-0001 recebe status `Substituído parcialmente por ADR-0010` — permanece válido para conhecimento e governança, deixa de valer para código publicável.

## Consequência para a decisão

**Esta é uma decisão de baixo custo agora e alto custo depois.** Trocar de repositório com 3 arquivos custa minutos; com 200 arquivos, histórico e deploys em produção, custa uma migração.

Por isso está registrada como proposta antes de qualquer código ser escrito.

## O que não depende desta decisão

Governança de agentes, estrutura de MVPs, regras de branch e commit, critérios de aceite e definição de pronto valem em qualquer das duas opções — e por isso foram construídos hoje, em `governanca/engenharia/`.

## Revisão

Decidir antes da ETAPA 5 (arquitetura-base). Nenhum código de produto deve ser escrito antes.
