# ADR-0008 — Sistema de comentários desativado no site institucional

**Data:** 2026-08-04
**Status:** Aceito
**Decisores:** Márccio (PO) · Claude (execução e recomendação)

## Contexto

Em 04/08/2026 o site `stakeholderbrasil.com` acumulava **176 comentários aguardando moderação e 339 já classificados como spam** — 515 no total.

A análise da fila revelou uma campanha coordenada de botnet promovendo casas de aposta, com assinatura inequívoca: domínios `vox-casino8/11/13.com` e `888starz-apk1.com`, **todos usando o mesmo endereço de e-mail** (`@bystriy324andy.site`), IPs concentrados na faixa `89.124.x.x`, e 14 links por comentário contra um limite configurado de 2.

O dado que decidiu a estratégia: **zero comentários aprovados em toda a história do site.** Não havia conversa legítima a preservar — apenas superfície de ataque consumindo tempo de moderação.

Auditoria do ambiente encontrou as seguintes portas abertas: comentários e pingbacks habilitados por padrão em conteúdo novo; fechamento automático em posts antigos desligado; listas de termos bloqueados e de moderação **vazias**; XML-RPC respondendo com `system.multicall` exposto; e **77 arquivos de mídia com comentários abertos** — vetor que não aparece em nenhuma tela de configuração.

O site é institucional B2B, voltado a autoridade, captação de leads e apresentação de serviços. Não há comunidade, não há dependência de comentários para SEO nem para conversão.

## Decisão

**O sistema de comentários fica integralmente desativado no site institucional** — em posts, páginas, mídia e qualquer conteúdo futuro.

A proteção escolhida não é filtrar melhor: é **eliminar a superfície**. Filtro sempre deixa passar alguma coisa e exige manutenção permanente; superfície inexistente não é atacável.

Como defesa em profundidade — para o caso de alguém reativar comentários sem contexto —, ficam configurados: exigência de login, moderação obrigatória, limite de zero links, fechamento automático após 1 dia, e 26 padrões de bloqueio incluindo os domínios e faixas de IP desta campanha.

## Alternativas consideradas

| Opção | Prós | Contras | Por que não |
|---|---|---|---|
| Akismet | Padrão de mercado, eficaz | Pago para uso comercial (a Stakeholder é PJ); envia dados de visitantes a terceiro, o que pesa sob LGPD | Custo recorrente e exposição de dados para proteger algo sem valor para o negócio |
| Antispam Bee | Gratuito, sem envio a terceiros | Mais um plugin a manter; ainda deixa passar uma fração | Se ninguém comenta, filtrar é resolver o problema errado |
| Cloudflare Turnstile / reCAPTCHA | Barreira forte a bot | Atrito para o usuário; complexidade; um serviço externo a mais | Desproporcional para superfície que será removida |
| Manter e moderar manualmente | Nenhuma mudança técnica | Consome tempo do PO indefinidamente | É exatamente o problema que se queria resolver |

## Consequências

**O que passa a ser verdade**
- `POST /wp-comments-post.php` retorna **403**
- `POST /wp-json/wp/v2/comments` retorna **401**
- `POST /xmlrpc.php` retorna **"Disabled"**
- Fila de moderação permanentemente em zero
- Nenhum formulário de comentário renderiza no site público

**O que fica pior, e é aceito**
- Se a Stakeholder Academy ou um blog vierem a incentivar discussão, comentários precisarão ser reativados deliberadamente — com proteção adequada desenhada antes, não depois
- Os 515 comentários classificados como spam permanecem na base. O WordPress os remove automaticamente após 30 dias; exclusão imediata é ação manual do PO em Comentários → Spams

**Efeito colateral positivo**
A desativação do XML-RPC eliminou também a amplificação de força bruta via `system.multicall` e o uso do site como refletor de pingback contra terceiros — riscos que existiam independentemente do spam.

## Evidências e testes

Testes executados **antes e depois** da intervenção, com resultado verificado e não presumido:

| Verificação | Antes | Depois |
|---|---|---|
| `POST /wp-comments-post.php` | aceitava | 403 |
| `POST /wp-json/wp/v2/comments` | aceitava | 401 |
| `POST /xmlrpc.php` (`system.listMethods`) | lista de métodos | "Disabled" |
| Formulário de comentário na home | presente | ausente |
| Fila de moderação | 176 | 0 |
| Conteúdo com comentários abertos | 1 post · 20 páginas · 77 mídias | 0 |

Amostragem de 10 comentários pendentes confirmou padrão único de campanha antes de qualquer ação em massa — nenhum foi classificado sem inspeção prévia da amostra.

## Revisão

Reavaliar quando houver decisão de negócio de criar blog ou Academy com discussão ativa. Nesse cenário, reativar **apenas no tipo de conteúdo específico**, nunca globalmente, e com camada de proteção definida antes da reativação.

Não reativar por conveniência, por padrão de tema ou por atualização de plugin — este ADR existe justamente para que a decisão não seja desfeita sem contexto.
