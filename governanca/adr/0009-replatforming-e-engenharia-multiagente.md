# ADR-0009 — Replatforming do website e engenharia multiagente

**Data:** 2026-08-20
**Status:** Aceito
**Decisores:** Márccio (PO) · ChatGPT · Claude

Especificação completa em [replatforming-sie.md](../replatforming-sie.md).

## Contexto

O website institucional foi construído em WordPress no ecossistema Hostinger. O ambiente permitiu validar ideias e chegar a 12 páginas com funil, identidade própria e prova social — mas passou a restringir a evolução pretendida: ambiente de desenvolvimento, controle de alterações, testes, publicação, integração entre ferramentas e uso avançado de agentes de IA.

O inventário de 04/08/2026 documentou sintomas concretos: ausência de tema filho impedindo customização em PHP, alterações feitas diretamente em produção sem versionamento, dependência de sessão de navegador para automação e nenhum ambiente de homologação.

**A limitação não é das ferramentas de IA.** É do ambiente.

## Decisão

O SIE será **reconstruído** — não migrado — em arquitetura versionada e desacoplada da hospedagem: GitHub como fonte de verdade, desenvolvimento local, Vercel para preview e produção, e engenharia conduzida por agentes especializados sob aprovação humana.

O website atual passa a ser **sistema legado de referência**. Nada é descartado antes de inventariado.

Domínio e DNS permanecem na Hostinger. Hospedagem e registro são responsabilidades distintas.

## Alternativas consideradas

| Opção | Prós | Contras | Por que não |
|---|---|---|---|
| Manter WordPress e criar tema filho | Menor esforço; preserva o CMS | Não resolve versionamento, homologação nem trabalho de agentes | Trata o sintoma, não a restrição |
| Migrar o WordPress para outra hospedagem | Simples | Mesmo modelo, outro fornecedor | Não muda nada do que motivou a decisão |
| WordPress headless + frontend na Vercel | Preserva a edição visual de conteúdo | Mantém dependência do WordPress e sua superfície de manutenção | Considerado viável; decisão adiada para a ETAPA 4 |
| Reconstrução completa em stack moderna | Versionamento, preview, agentes, performance | Perde a edição visual de conteúdo se nada a substituir | **Escolhida**, com a ressalva registrada abaixo |

## Consequências

**O que passa a ser verdade**
- Nenhuma alteração de produção sem passar por branch, review e aprovação humana
- Todo o código auditável e recuperável
- Preview como ambiente obrigatório de homologação
- Agentes com escopo delimitado e sem autorização para publicar em produção
- Trabalho organizado em MVPs com critérios de aceite e evidências

**O que fica pior, e precisa de resposta antes da ETAPA 4**
- **A edição visual de conteúdo desaparece.** Hoje o PO altera texto pelo editor de blocos. Numa aplicação em código, cada mudança de conteúdo vira alteração de código, commit e deploy. Sem substituto definido, isso é regressão operacional para um operador solo. Três caminhos: markdown versionado no repositório — o que é coerente com o [ADR-0003](0003-markdown-como-fonte.md) e com a natureza do projeto —, CMS headless, ou WordPress headless
- **O baseline de SEO precisa ser capturado antes**, não depois. O site tem Rank Math, Open Graph e Search Console conectado. Sem registro do estado atual — URLs, títulos, descriptions, posições — não haverá como detectar regressão após a troca
- Dois ambientes conviverão durante a transição: o legado em produção e o novo em preview. **O legado continua exposto e continua exigindo manutenção de segurança** enquanto for o site público
- Curva de aprendizado de stack e de fluxo Git para quem hoje edita visualmente

**Interação com débitos existentes**
- Os débitos técnicos do ambiente legado não são anulados por esta decisão. O legado permanece público durante toda a transição e segue sendo a maior superfície de risco a tratar
- Os débitos ligados à automação do CMS perdem relevância no destino, mas seguem válidos enquanto o WordPress for a fonte do conteúdo a inventariar
- Os débitos de acesso e de autenticação sobem de prioridade: o GitHub passa a ser a fonte de verdade do produto, não apenas da documentação

## Riscos registrados

| Risco | Mitigação prevista |
|---|---|
| Reconstrução perder conteúdo ou funcionalidade do legado | Inventário obrigatório antes de qualquer reconstrução (ETAPAS 1–3) |
| Perda de posicionamento em busca | Baseline de SEO capturado no inventário; URLs preservadas ou redirecionadas (§28) |
| Agentes revisando agentes e aprovando erros mútuos | Aprovação humana obrigatória nos pontos críticos (§15). Começar com orquestrador + desenvolvimento + revisão humana; acrescentar QA e Review automatizados só quando o volume justificar |
| Escopo crescer para as ferramentas da §19 antes da fundação existir | Sequenciamento das ETAPAS; nenhuma ferramenta entra antes da necessidade real |
| Ambiente novo virar o novo gargalo | Princípio de desacoplamento (§27): nenhuma ferramenta é o SIE |

## Revisão

Reavaliar ao fim da ETAPA 3, quando o inventário e a auditoria estiverem concluídos e a matriz **TEMOS × PRECISAMOS × PRESERVAR × REFAZER × CRIAR** existir. A escolha de stack (ETAPA 4) e a decisão sobre gestão de conteúdo devem ser registradas em ADR próprio.
