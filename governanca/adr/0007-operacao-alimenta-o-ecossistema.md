# ADR-0007 — A operação alimenta o ecossistema: ciclo permanente de aprendizagem

**Data:** 2026-08-01
**Status:** Aceito
**Decisores:** Márccio (PO) · ChatGPT · Claude

## Contexto

Até aqui, reuniões eram tratadas como eventos que produziam documentação: briefing antes, ata depois. A documentação nascia em `operacao/`, cumpria sua função naquela data e morria ali. O conhecimento gerado dependia da memória de quem participou.

Durante a preparação da reunião com uma empresa parceira ficou claro que isso desperdiça o ativo mais valioso da operação. Uma reunião não produz apenas um registro do que foi dito — ela produz evidência de mercado: quais funcionalidades despertam interesse, quais objeções aparecem, quais oportunidades surgem sem serem provocadas, como a conversa evolui quando se demonstra uma ferramenta real.

Esse material, se destilado, não serve só àquela empresa. Ele torna a **próxima** reunião mais inteligente — com qualquer empresa.

O que faltava não era estrutura. `operacao/` (datado, perecível) e `conhecimento/` (permanente, cresce) já existem e a separação já estava certa. Faltava o **ritual de travessia** entre os dois, e a compreensão de que a travessia é o produto, não um subproduto.

## Decisão

**A operação alimenta o ecossistema, e o ecossistema devolve capacidade para a operação.** Toda interação relevante — reunião, demonstração, visita — é tratada como ponto de captura de um ciclo permanente de aprendizagem organizacional, não como evento isolado.

O ciclo tem quatro movimentos:

```
    ┌──────────────────────────────────────────────────┐
    │                                                  │
    ▼                                                  │
 PREPARAR                                          DESTILAR
 briefing + roteiro                          registro → dossiê
 (operacao/)                                      → conhecimento/
    │                                                  ▲
    ▼                                                  │
 EXECUTAR ──────────────────────────────────────► CAPTURAR
 a interação real                          perguntas, reações,
                                        demonstrações, objeções
```

O quarto movimento fecha em cima do primeiro: o conhecimento destilado torna o próximo briefing mais afiado, o próximo roteiro mais preciso e o próximo playbook mais eficaz.

**Quatro regras operacionais:**

1. **Três documentos por interação, nunca mais** — `briefing`, `roteiro` e `registro`, todos em `operacao/reunioes/`. A granularidade dos ativos capturados vive *dentro* do registro, não em arquivos separados.
2. **Mídia nunca entra no repositório** — estende o ADR-0003. Vídeo, áudio e capturas de tela ficam no armazenamento em nuvem, em pasta por data e empresa. No repositório entram a transcrição (texto é formato-fonte) e o ponteiro para a mídia.
3. **Gravação exige acordo prévio** — combinada antes da reunião, preferencialmente por escrito, nunca solicitada ao vivo no início da conversa. Consentimento, finalidade e prazo de retenção registrados no `registro.md`.
4. **Demonstrações de ferramentas viram playbooks** — SigaPregão, Prosas e demais plataformas da operação ganham roteiro executável em `playbooks/`, com o que mostrar, em que ordem, e o que observar na reação. Passam a ser ativo permanente do conhecimento operacional.

Padrões que atravessam múltiplas interações — objeções recorrentes, demonstrações que funcionam, caminhos estratégicos revelados — destilam para `conhecimento/comercial/`, **quando houver corpus que os sustente**. Com uma reunião não há padrão, há anedota.

## Alternativas consideradas

| Opção | Prós | Contras | Por que não |
|---|---|---|---|
| Ata simples por reunião | Familiar, baixo esforço | Morre em `operacao/`; nada retorna para a próxima operação | Não fecha o ciclo — é exatamente o modelo que estamos superando |
| Um documento por tipo de ativo capturado (16) | Granularidade máxima | Dezesseis arquivos por reunião | Mataria a prática em duas reuniões |
| Diretório `/inteligencia` de topo com taxonomia própria | Isola o conceito | Duplica a função de `conhecimento/`; exige taxonomia antes de existir corpus | ADR-0006 já resolveu a localização; taxonomia sem corpus é adivinhação |
| Pipeline de transcrição e análise automática | Escala bem | Infraestrutura congelada na Fase 2; volume atual não justifica | Prematuro. Análise manual de uma reunião é mais rica que qualquer pipeline |
| Versionar mídia com Git LFS | Tudo num lugar só | Custo, peso, e sem diff | ADR-0003 já rejeitou pelos mesmos motivos |

## Consequências

**O que passa a ser verdade**
- Toda interação relevante tem três documentos: briefing (antes), roteiro (condução), registro (captura + destilação)
- O `registro.md` é o ponto de travessia: dentro dele há uma seção explícita declarando o que vai para o dossiê da empresa, o que vai para `conhecimento/`, e o que vira playbook
- Playbooks de demonstração passam a ser ativo permanente — conhecimento que hoje existe só na cabeça do PO fica registrado
- `conhecimento/comercial/` é o destino dos padrões que atravessam empresas, ativado por necessidade real
- Nenhum diretório novo foi criado. A arquitetura absorveu a evolução sem crescer

**O que fica pior, e é aceito**
- Custo de disciplina pós-interação: o registro precisa ser feito enquanto a memória está fresca (até 24h). Registro não feito quebra a corrente — e uma corrente quebrada não é uma reunião perdida, é um ciclo que não aprendeu
- A gravação, quando houver, adiciona uma etapa de acordo prévio ao agendamento
- Há tensão real entre capturar e obter candura: os sinais mais valiosos — hesitação, objeção espontânea, reação não filtrada — são os que as pessoas suprimem quando sabem que estão sendo gravadas. Por isso a regra 3 existe, e por isso não gravar é uma escolha legítima em primeiros contatos

**Exposição legal**
- Voz e imagem de terceiros são dado pessoal sob a LGPD. Gravação sem base legal expõe a Stakeholder Trading. O consentimento prévio registrado é a base adotada

## Revisão

Reavaliar por volta da **quinta interação registrada**, quando houver corpus suficiente para que os padrões emerjam sozinhos — e aí saberemos quais arquivos `conhecimento/comercial/` realmente precisa, com evidência em vez de suposição.

Reavaliar antes disso se o volume de interações crescer a ponto de tornar a destilação manual o gargalo — aí a automação passa a se justificar, e o congelamento de infraestrutura da Fase 2 precisa ser revisto por decisão explícita do PO.
