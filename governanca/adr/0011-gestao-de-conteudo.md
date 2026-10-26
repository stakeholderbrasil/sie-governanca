# ADR-0011 — Gestão de conteúdo do novo site

**Data:** 2026-08-20
**Status:** **Aceito — provisório e reversível por desenho**
**Decisores:** Márccio (PO) · Claude (análise)

## Contexto

O [ADR-0009](0009-replatforming-e-engenharia-multiagente.md) registrou como consequência a perda da edição visual de conteúdo ao sair do WordPress. Hoje o PO altera texto pelo editor de blocos; numa aplicação em código, cada frase vira commit e deploy.

As 12 páginas do legado já foram extraídas para markdown com frontmatter — rota, title e description — em `legado/conteudo/`.

## Decisão

**Markdown versionado é a fonte de conteúdo da primeira versão do novo SIE**, com uma condição de arquitetura que não é opcional:

> A camada de conteúdo deve ser **substituível por CMS ou headless sem reconstruir o frontend.**

Decisão **provisória e reversível**. Vale enquanto o volume e a cadência de edição não justificarem CMS.

## Por que markdown agora

Coerente com o [ADR-0003](0003-markdown-como-fonte.md), que já estabelece markdown como formato-fonte de todo conhecimento versionado. Dá ao conteúdo do site as mesmas propriedades do resto do SIE: diff legível, histórico, review por PR, e recuperabilidade.

Não exige serviço externo, conta, custo recorrente nem mais uma superfície de acesso a inventariar. E o conteúdo já está nesse formato.

## A condição de reversibilidade

Para que a troca futura não custe uma reconstrução, o frontend **nunca lê arquivo de conteúdo diretamente**. Ele consome uma camada de acesso com contrato estável:

```
   PÁGINA / COMPONENTE
          │  consome tipos, nunca arquivos
          ▼
   CAMADA DE ACESSO A CONTEÚDO
   getPagina(slug) · listarPaginas() · getMetadados(slug)
          │  implementação trocável
          ▼
   markdown  →  (futuro: CMS headless / API)
```

**Regras que tornam a reversão barata:**

1. Nenhum componente importa arquivo `.md` diretamente
2. O formato de retorno é tipado e independente da origem — trocar a fonte não muda a assinatura
3. Frontmatter é normalizado na camada de acesso, não interpretado nas páginas
4. Nenhuma regra de negócio vive dentro do arquivo de conteúdo

Cumpridas as quatro, migrar para CMS significa reescrever uma implementação — não o site.

## Alternativas consideradas

| Opção | Por que não agora |
|---|---|
| CMS headless (Sanity, Contentful, Payload) | Serviço externo, custo, conta e superfície de acesso adicionais antes de haver volume que justifique |
| WordPress headless | Mantém toda a superfície de manutenção que motivou o ADR-0009 |
| Conteúdo dentro do código | Impede edição sem conhecimento de programação e perde o frontmatter |

## Consequências

**O que passa a ser verdade**
- Conteúdo com histórico, diff e review — hoje uma alteração de texto no site não deixa rastro
- Conteúdo extraído do legado preservado como patrimônio do projeto
- Alteração de texto exige commit e deploy — mais lento que o editor visual, e é o custo aceito

**Gatilho de reavaliação**
Quando qualquer um ocorrer: cadência de edição superior a uma vez por semana · alguém sem Git precisando editar · conteúdo dinâmico ou personalizado · mais de 40 páginas.

## Revisão

Reavaliar após o MVP-007, quando as páginas principais existirem e a cadência real de edição for observável — não estimada.
