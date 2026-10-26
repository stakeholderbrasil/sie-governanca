# ADR-0012 — Stack do novo site

**Data:** 2026-08-20
**Status:** **Aceito** — aprovado pelo PO em 20/08/2026
**Decisores:** Márccio (PO) · Claude (análise)
**Etapa:** 4 do [documento-mestre](../replatforming-sie.md)

---

## Contexto

O que o site precisa ser, com base em fatos já levantados:

| Fato | Fonte |
|---|---|
| 12 páginas institucionais · ~3.950 palavras de conteúdo próprio | [inventário do legado](../inventario-site-legado.md) |
| 12 rotas indexadas, conectadas ao Search Console | idem |
| Design system com 28 tokens CSS · Montserrat + Open Sans | [design system](../../marca/design-system.md) |
| Markdown como fonte de conteúdo, com camada de acesso trocável | [ADR-0011](0011-gestao-de-conteudo.md) |
| GitHub fonte de verdade · Vercel · agentes sob aprovação humana | [ADR-0009](0009-replatforming-e-engenharia-multiagente.md) |
| Operador solo, que edita conteúdo e não código | — |
| Defeitos a não repetir: `og:image` ausente, 5 páginas sem `h1`, CTA em 404 | inventário e CHANGELOG |

**Princípio declarado pelo PO:** *não montar uma stack de SaaS. Um website institucional de alto nível, sobre fundação capaz de evoluir.*

## Descartadas sem análise longa

| Opção | Por que não |
|---|---|
| **React SPA (Vite, CRA)** | Sem renderização no servidor, o conteúdo não existe no HTML inicial. Para um site cuja função é ser encontrado, isso é desqualificante. Descartado por requisito, não por preferência |
| **Nuxt / SvelteKit** | Tecnicamente competentes, mas nenhuma vantagem sobre as finalistas neste caso, com ecossistema menor e menos material para agentes |
| **WordPress headless** | Mantém integralmente a superfície de manutenção que motivou o ADR-0009 |

---

# A · Stack mínima

**Eleventy (11ty) + markdown + CSS**

O menor conjunto capaz de produzir um site profissional de verdade.

| | |
|---|---|
| **Vantagens** | Gera HTML estático puro. Markdown nativo. Praticamente sem conceitos a aprender. Build em segundos. Zero JavaScript enviado ao navegador, salvo o que se escrever à mão. Custo computacional e cognitivo mínimos |
| **Desvantagens** | Sem sistema de componentes com tipos. Sem validação de frontmatter — erro de conteúdo só aparece em produção. Ecossistema pequeno. Interatividade exige JavaScript escrito à mão |
| **Riscos** | O diagnóstico interativo (MVP-008) e a área do cliente exigiriam ferramenta adicional ou migração. Menos material de referência para agentes, o que aumenta a chance de código improvisado |
| **Custo** | R$ 0 de ferramenta |
| **Complexidade** | Muito baixa |
| **Adequação ao SIE** | **Boa para o site de hoje, fraca para o ecossistema de amanhã.** Entregaria as 12 páginas com excelência e travaria na primeira funcionalidade interativa |

**Vale considerar se** o objetivo fosse apenas publicar as 12 páginas e nada além. Não é o objetivo declarado.

---

# B · Stack recomendada

**Astro + TypeScript + Markdown (Content Collections) + CSS com os tokens do design system**

### Por que cada peça existe

**Astro** — construído para exatamente este caso: site majoritariamente de conteúdo, com interatividade pontual. Envia **zero JavaScript por padrão**; JS só existe onde se pede explicitamente (ilhas). Content Collections leem markdown com **schema tipado do frontmatter**, validado em tempo de build.

Isso resolve estruturalmente dois problemas reais do legado:
- Frontmatter inválido, título longo demais ou `og:image` ausente **quebram o build**, não a produção
- Página sem conteúdo não é publicada por engano

**TypeScript** — com agentes escrevendo código, tipo é a forma mais barata de revisão automática que existe. Erro de contrato aparece antes do pull request, não na revisão humana. Não é preferência estética: é redução de carga de revisão.

**Markdown com Content Collections** — cumpre o ADR-0011 e, melhor, **impõe** a camada de acesso: em Astro se consulta a coleção por API tipada, não se importa arquivo. As quatro regras de reversibilidade do ADR-0011 passam a ser o caminho natural, não disciplina a manter.

**CSS com os tokens** — `styles/tokens.css` já existe e é cópia controlada do `sie`. Astro tem escopo de estilo nativo por componente: estilo não vaza entre componentes sem convenção nem ferramenta extra.

| | |
|---|---|
| **Vantagens** | Melhor performance possível para site de conteúdo. Frontmatter validado em build. Controle total sobre `<head>` — corrige o defeito de `og:image` na raiz. Sitemap e RSS oficiais. Suporta componentes React onde houver interatividade real, sem carregá-los nas demais páginas. Endpoints de servidor para formulário. Escopo de estilo nativo — bom isolamento para trabalho de múltiplos agentes |
| **Desvantagens** | Ecossistema menor que o do Next.js. Menos material público, o que em tese reduz a confiabilidade de agentes. Para uma área do cliente com autenticação e sessão, é menos natural |
| **Riscos** | Se o SIE virar mesmo uma plataforma com login e dados por usuário, essa parte pede outra ferramenta — mitigável, ver seção C |
| **Custo** | R$ 0 de ferramenta. Ver seção de custo real ao final |
| **Complexidade** | Baixa a média. Menos conceitos que o Next.js App Router |
| **Adequação ao SIE** | **Alta.** Serve o site de hoje com excelência e cobre a estrada até o MVP-011 sem mudar de ferramenta |

---

# A decisão real: Astro ou Next.js

As duas fariam o trabalho. A comparação honesta:

| Critério | Astro | Next.js (App Router) |
|---|---|---|
| 12 páginas de conteúdo | **Melhor** — construído para isso | Faz bem, com mais maquinário |
| JavaScript enviado | **Praticamente zero** | Runtime React mesmo em página estática |
| Markdown com frontmatter tipado | **Content Collections, nativo** | Requer biblioteca e integração manual |
| Controle de `<head>` e SEO | Excelente | **Metadata API, excelente** |
| Sitemap, RSS, redirects | Integrações oficiais | Nativo |
| Formulário e endpoints | Suficiente | **Melhor** |
| Autenticação e área do cliente | Possível, não natural | **Claramente melhor** |
| Integração com a Vercel | Muito boa | **Nativa — a Vercel faz o Next.js** |
| Material para agentes | Menor | **Muito maior** |
| Modos de falha | Poucos | **Mais** — fronteira servidor/cliente, hidratação, cache do App Router |
| Contratação futura | Mercado menor | **Mercado maior** |

### Por que recomendo Astro mesmo assim

**1. O risco dominante deste projeto não é "não conseguir construir a área do cliente depois". É "o site quebrar ou regredir em SEO".** Foi exatamente o que aconteceu no legado — `og:image` ausente, páginas sem `h1`, CTA em 404, formulário que nunca funcionou. Menos runtime significa menos coisa para quebrar.

**2. Sobre agentes, o argumento se inverte na prática.** Volume de material de treino ajuda, mas **menos conceitos significa menos formas de errar**. Os erros caros do Next.js — componente de servidor importando componente de cliente, `useState` onde não pode, cache invalidando quando não devia — não existem em Astro, porque o modelo é mais simples. Para trabalho conduzido por agentes com revisão humana, previsibilidade vale mais que popularidade.

**3. A área do cliente é outro produto.** Autenticação, sessão, dados por usuário — nada disso compartilha código com um site institucional. Quando existir, vive em `app.stakeholderbrasil.com`, com a stack que fizer sentido então. Escolher Next.js hoje pelo login de amanhã é pagar complexidade adiantada por uma funcionalidade que ainda não foi justificada — exatamente o que o §20 do documento-mestre proíbe para o banco de dados.

### Quando Next.js seria a escolha certa

Se a área do cliente estivesse no roadmap dos próximos 6 meses com data, ou se houvesse intenção de contratar desenvolvedor. Nenhuma das duas é verdade hoje.

**Esta é uma discordância legítima, não um erro.** Se você preferir Next.js pelo alinhamento com o mercado, é defensável — e nesse caso registro a decisão sem resistência.

---

# C · Stack futura

Nada aqui entra agora. Cada item tem gatilho.

| Camada | Ferramenta | Gatilho | Observação |
|---|---|---|---|
| E-mail transacional | **Resend** | MVP-008, formulário de diagnóstico | Substitui SureMail. Faixa gratuita cobre o volume atual |
| Chatbot | **Botpress** | Diagnóstico conversacional | Integra por script ou API — independe da stack |
| Atendimento | **Chatwoot** | Volume de conversas justificar | Idem |
| Automação | **Mautic** | Base de contatos justificar | Autogerenciado, exige servidor — reavaliar contra alternativas |
| Banco de dados | **Neon/PostgreSQL** | Necessidade real de dado estruturado | §20 do documento-mestre |
| Autenticação | a definir | Área do cliente | §22 |
| Pagamentos | **Stripe** | Produto digital pago | §23 |

**A regra:** cada item entra por API ou webhook, **nunca como dependência estrutural do site**. É o §27 — nenhuma ferramenta se torna o SIE.

---

# Onde eu discordo da proposta implícita

Você citou *"Next.js + TypeScript + Tailwind + Markdown"* como exemplo. Concordo com TypeScript e Markdown. Divirjo dos outros dois.

### Tailwind — não recomendo

**O design system já existe como 28 propriedades CSS**, extraídas do site em produção e publicadas em `styles/tokens.css`. Adicionar Tailwind criaria uma **segunda representação da mesma verdade**: os tokens teriam que ser mapeados para a configuração do Tailwind, e as duas divergiriam com o tempo. Divergência entre fontes é o problema que o ADR-0010 evita ao proibir edição paralela do design system.

Some-se: 12 páginas e talvez 15 componentes não produzem o volume de CSS que justifica Tailwind. E o Astro tem escopo de estilo nativo — estilo não vaza sem convenção nem plugin.

**Contra-argumento honesto:** agentes escrevem Tailwind com muita consistência, e ele impede deriva de CSS. É real. Mas o escopo nativo do Astro resolve a deriva sem introduzir uma segunda fonte de verdade.

**Recomendação: CSS puro com os tokens, dentro do escopo de componente.** Se em seis meses o CSS estiver difícil de manter, Tailwind entra — e aí com dor concreta para justificar.

### Biblioteca de componentes — não

shadcn/ui, Material, Chakra. A identidade da Stakeholder é própria, com paleta, tipografia e botão em pílula de 50px já definidos. Biblioteca de componentes brigaria com isso, e o trabalho seria sobrescrever padrões alheios em vez de escrever os próprios.

### Gerenciamento de estado — não

Não há estado compartilhado num site institucional. Se o diagnóstico do MVP-008 precisar, resolve-se dentro da ilha.

---

# O que deliberadamente NÃO adicionar agora

- ❌ **Tailwind** — segunda fonte de verdade sobre a identidade visual
- ❌ **Biblioteca de componentes** — briga com a identidade própria
- ❌ **CMS headless** — ADR-0011 é provisório justamente para permitir isso depois
- ❌ **Banco de dados** — §20, sem necessidade real
- ❌ **Autenticação** — §22
- ❌ **Stripe** — §23
- ❌ **Gerenciamento de estado**
- ❌ **Suíte de testes end-to-end** — 12 páginas estáticas não justificam Playwright hoje
- ❌ **Docker** — a Vercel constrói; ambiente local é `npm run dev`
- ❌ **Monorepo com workspaces** — um site, um pacote

## O que adicionar, e é barato

**Uma verificação de build que impeça os defeitos do legado de voltarem.** Cerca de 50 linhas, sem dependência nova, rodando no build e no CI:

- Exatamente um `<h1>` por página
- `<title>` até 60 caracteres
- `description` entre 120 e 158
- `og:image` presente
- Toda imagem com `alt`
- Nenhum link interno apontando para rota inexistente

Isso transforma os *"requisitos que atravessam todo MVP"* de checklist manual em **falha de build**. Dado o histórico — `og:image` ausente em 100% das páginas, CTA em 404, formulário que nunca funcionou — é o item de melhor retorno de toda esta ADR.

---

# Custo real

| Item | Custo |
|---|---|
| Astro, TypeScript, Markdown, CSS | R$ 0 |
| GitHub — repositório privado | R$ 0 |
| GitHub — proteção de branch | Indisponível no plano gratuito |
| Vercel — plano Hobby | R$ 0 |
| **Vercel — uso comercial** | ⚠️ **Verificar antes do go-live** |

⚠️ **Ponto de atenção não previsto no documento-mestre:** o plano Hobby da Vercel é destinado a projetos pessoais e não comerciais. O site da Stakeholder Trading é comercial. Isso normalmente exige o plano **Pro, na casa de US$ 20/mês por usuário**.

**Não afeta desenvolvimento nem Preview** — afeta o momento de apontar o domínio para produção. Precisa ser confirmado nos termos vigentes antes do go-live, e entra no cálculo comparativo: a Hostinger já está paga.

---

# Decisão

**Astro + TypeScript + Content Collections em markdown + CSS com os tokens do design system + verificação de qualidade no build.**

Nada além disso até haver dor concreta.

**Confiança:** alta para o site institucional. Média-alta no geral — a incerteza está em quão rápido o SIE caminha para área do cliente. Se esse horizonte for menor que seis meses, Next.js passa à frente.

## Consequências

- Site com carga de JavaScript próxima de zero — o oposto do legado com WordPress, Astra, Spectra e LiteSpeed
- Erro de conteúdo falha no build, não em produção
- Os quatro requisitos de reversibilidade do ADR-0011 passam a ser o caminho natural
- Área do cliente, quando existir, será outro produto em outro subdomínio — decisão a registrar em ADR próprio
- Se a escolha se mostrar errada, migrar 12 páginas de markdown e ~15 componentes é trabalho de semanas, não de meses. **A decisão é reversível a custo conhecido**

## Revisão

Reavaliar no MVP-008 (diagnóstico), quando a primeira funcionalidade genuinamente interativa expuser os limites da escolha. Ou antes, se a área do cliente ganhar data.
