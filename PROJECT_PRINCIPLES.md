# Princípios do SIE

**Este é o documento mais estável do repositório.**
Mudar um princípio exige ADR aprovado pelo Conselho de Arquitetura. Não se altera por conveniência de um projeto.

---

## Princípio fundador

> **A inteligência deve servir para compreender, conectar e criar valor para todas as partes legítimas envolvidas.**

Consequência prática: não trabalhamos para explorar assimetria de informação. Trabalhamos para reduzi-la — entre quem tem uma solução legítima e quem tem uma necessidade pública ou social real.

---

## 1. Conhecimento só gera valor quando pode ser compreendido, reutilizado e evoluído

Conhecimento que existe apenas na cabeça de uma pessoa ou na memória de uma IA não é ativo — é dependência.

**Como se aplica:** toda decisão relevante vira documento versionado. Toda descoberta vira conhecimento reutilizável. Nada essencial permanece só em conversa.

---

## 2. Diagnóstico antes de proposta

Proposta sem diagnóstico é chute com vocabulário técnico.

**Como se aplica:** antes de recomendar, medir. Antes de implementar, entender o estado atual. Quando não houver medição possível, declarar explicitamente que é estimativa.

---

## 3. Medição acima de opinião

Painel dizendo "conectado" não é prova de que está funcionando. Aparência de correto não é correção.

**Como se aplica:** toda entrega relevante é verificada de forma independente do que a ferramenta afirma. Quando a medição contraria a expectativa, a medição vence.

**Corolário:** o instrumento de medição também precisa ser validado. Um teste que produz falso negativo é pior que nenhum teste, porque gera confiança indevida.

---

## 4. Transparência inclui os próprios erros

Corrigir a si mesmo custa menos que sustentar um diagnóstico errado.

**Como se aplica:** quando uma análise anterior se mostra incorreta, corrigimos publicamente e registramos o motivo. O rastro do erro tem valor — evita repeti-lo.

---

## 5. Credibilidade se constrói com contexto, não com média favorável

Número sem contexto que não se sustenta ao ser questionado destrói mais reputação do que constrói.

**Como se aplica:** quando um resultado tem concentração, ressalva ou limitação, ela é apresentada junto. Nunca se publica prova social fabricada, depoimento inventado ou parceiro que não existe.

---

## 6. A marca é protagonista; pessoas reforçam

Credibilidade institucional não deve depender da imagem de um indivíduo.

**Como se aplica:** a Stakeholder Trading é o sujeito da comunicação. A presença do fundador acrescenta confiança, mas não sustenta a identidade sozinha.

---

## 7. Cultivo, não entrega

O símbolo é o bonsai. Árvore qualquer cresce sozinha, de qualquer jeito. Bonsai é resultado de poda constante e paciência.

**Como se aplica:** preferimos processo documentado a improviso brilhante. Cada ciclo alimenta a base de conhecimento. O objetivo é deixar a organização mais capaz do que a encontramos — inclusive a nossa.

---

## 8. Menor privilégio, sempre

Nenhum acesso é concedido "por precaução".

**Como se aplica:** permissões mínimas necessárias, revistas periodicamente. Credencial nunca reside em pasta versionada. Ferramenta que não está em uso não mantém acesso.

---

## 9. Uma ferramenta, um papel

Backlog em dois lugares é pior que backlog em nenhum.

**Como se aplica:** cada ferramenta tem responsabilidade única e declarada. Adotar uma nova exige justificar o custo de manutenção e definir a fronteira com as existentes.

---

## 10. Sprint é definida pelo incremento, não pelo volume de trabalho

Acumular tarefas não fecha uma Sprint. Mudar o objetivo do incremento abre outra.

**Como se aplica:** melhorar o mesmo incremento é ciclo de refinamento. Sprint nova começa quando o objetivo muda.

---

## 11. Regra de Ouro

> Toda funcionalidade criada deve responder: **"Como isso ajuda a Stakeholder Trading a gerar mais valor para seus clientes e a conquistar novos negócios?"**

Se não contribui para esse objetivo nem para a base necessária para alcançá-lo, é reavaliada ou repriorizada.

---

## 12. A tecnologia evolui; a identidade permanece

O SIE não depende de fornecedor específico. ChatGPT, Claude, GitHub, Jira e qualquer outra ferramenta são meios — substituíveis.

**Como se aplica:** quando houver conflito entre conveniência tecnológica e os princípios acima, os princípios prevalecem.

---

*Alterações neste documento exigem ADR. Ver `governanca/adr/`.*
