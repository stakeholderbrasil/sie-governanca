# Governança do SIE

Extrato público dos registros de decisão do **SIE — Sistema de Inteligência Estratégica**, da Stakeholder Trading.

Aqui não está o código. Está o **porquê**: os princípios que orientam o projeto e cada decisão de arquitetura registrada no momento em que foi tomada — com as alternativas que foram descartadas, o motivo de cada descarte, o que a decisão piora, e a condição concreta que obriga a revisá-la.

## Por que isto é público

Decisão técnica costuma ser defendida depois do fato, quando o resultado já é conhecido. Um ADR escrito no momento da escolha não tem esse benefício: ele registra o que se sabia e o que não se sabia, e fica exposto ao que aconteceu depois.

Dois exemplos verificáveis neste repositório:

- O [ADR-0001](governanca/adr/0001-monorepo.md) decidiu monorepositório único. Três semanas depois, o [ADR-0010](governanca/adr/0010-repositorio-do-novo-site.md) o **superou parcialmente** — não porque estivesse errado, mas porque uma premissa mudou: o repositório passaria a entregar acesso de leitura a um serviço externo de build. O ADR-0001 permanece no lugar, com o status alterado. **ADR superado não se edita; se supera.**
- O [ADR-0002](governanca/adr/0002-jira-github.md) registra uma recomendação minha que foi **revista e revertida** depois do contra-argumento do PO. A posição original está no texto.

## Princípios

**[PROJECT_PRINCIPLES.md](PROJECT_PRINCIPLES.md)** — doze princípios e a Regra de Ouro. É o documento mais estável do projeto: alterá-lo exige um ADR.

## Decisões

| ADR | Decisão | Status |
|---|---|---|
| [0001](governanca/adr/0001-monorepo.md) | Monorepositório único e privado | Aceito · superado parcialmente pelo 0010 |
| [0002](governanca/adr/0002-jira-github.md) | Jira para negócio, GitHub para conhecimento | Aceito |
| [0003](governanca/adr/0003-markdown-como-fonte.md) | Markdown como formato-fonte | Aceito |
| [0004](governanca/adr/0004-nomenclatura.md) | Nomenclatura em minúsculas, sem acento, com hífen | Aceito |
| [0005](governanca/adr/0005-repo-fora-do-onedrive.md) | Repositório fora de pasta sincronizada | Aceito |
| [0006](governanca/adr/0006-inteligencia-de-empresas.md) | Inteligência de empresas como conhecimento permanente | Aceito |
| [0007](governanca/adr/0007-operacao-alimenta-o-ecossistema.md) | A operação alimenta o ecossistema | Aceito |
| [0008](governanca/adr/0008-comentarios-desativados.md) | Comentários desativados no site institucional | Aceito |
| [0009](governanca/adr/0009-replatforming-e-engenharia-multiagente.md) | Replatforming e engenharia multiagente | Aceito |
| [0010](governanca/adr/0010-repositorio-do-novo-site.md) | Repositório próprio para o novo site | Aceito |
| [0011](governanca/adr/0011-gestao-de-conteudo.md) | Markdown versionado como fonte de conteúdo | Aceito · provisório e reversível por desenho |
| [0012](governanca/adr/0012-stack-do-novo-site.md) | Stack do novo site | Aceito |

O [template](governanca/adr/template.md) usado em todos.

Se for ler apenas um, leia o **[ADR-0008](governanca/adr/0008-comentarios-desativados.md)**: ele traz a tabela de verificação *antes e depois* da intervenção, com resultado medido e não presumido.

## O que não está aqui, e por quê

Este repositório é um recorte. O repositório de origem é privado e contém dossiês de empresas, narrativa comercial, inventário de ativos digitais e política de credenciais — material que não se publica.

Pelo mesmo motivo, **três ADRs tiveram trechos generalizados** antes da publicação: referências a caminhos de máquina local, o nome de uma empresa em relacionamento ativo, e a identificação numérica de débitos técnicos ainda abertos. Nenhuma decisão, alternativa ou consequência foi removida — só os identificadores.

Este parágrafo existe porque o [Princípio 4](PROJECT_PRINCIPLES.md) diz que transparência inclui dizer o que foi omitido.

---

**Stakeholder Trading** · [stakeholderbrasil.com](https://stakeholderbrasil.com)
Portfólio do autor: [marcciomesquita-png.github.io](https://marcciomesquita-png.github.io/)
