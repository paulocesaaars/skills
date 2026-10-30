---
name: especificar-projeto
description: >-
  Conduz o Spec-Driven Development do projeto em greenfield (sem código de
  produto) e em brownfield (código já existente). Classifica o terreno pela
  árvore do repositório e, se ela não mostrar, pergunta se é monorepo ou parte
  de um projeto maior; com links, monta o contexto pelo Octocode. Em
  greenfield, o PRD nasce da conversa e a stack é definida pelo usuário ou
  proposta a partir do PRD. Em brownfield, lê o código antes de perguntar e
  registra o produto e a stack que já existem, sem propor outra. Preenche o
  TDD, oferece revisão cruzada e só cria o DESIGN.md se houver frontend. Pode
  gerar os requisitos de negócio no BDD.md e, no final, oferece o plano no
  modo de planejamento. Não implementa. Use quando o usuário pedir para
  especificar o projeto, conduzir o SDD, escrever o PRD, o TDD ou o design,
  definir a stack, documentar um sistema existente, ou situar um repositório
  num monorepo ou num projeto maior.
---

# Especificar Projeto

Conduz a especificação do projeto, uma etapa por vez, em tom consultivo:
pergunta pouco, explica o porquê e não despeja um questionário. Vale em
**greenfield** (ainda não há código de produto) e em **brownfield** (o código
de produto já existe). Roda **uma vez**. Se `PRD.md` e `TDD.md` já tiverem
conteúdo, diga que o projeto já foi especificado e só continue se o usuário
pedir para refazer. Para alinhar esses documentos a mudanças posteriores no
código, a skill é `atualizar-docs`. Explicações ao usuário em **português
(Brasil)**. Não implementa código.

## Regras

- Siga o `AGENTS.md`. Em conflito, vale o `HARNESS.md`, se existir. Sem
  `AGENTS.md`, avise e siga: não bloqueie a especificação por isso.
- Não antecipe a etapa seguinte. Não escreva o TDD durante o PRD, nem o
  design durante a revisão.
- Até 3 perguntas por rodada, da mais importante para a menos. Não pergunte
  o que o usuário, um documento ou o código já respondeu.
- Não invente requisito de produto. Se uma lacuna for irrelevante, registre
  a suposição em uma linha e siga. Se mudar escopo, público ou regra, pergunte.
- Em brownfield, a especificação descreve o produto que o código já é. O que
  o código não mostra não entra como capacidade. Troca de stack, arquitetura
  ou escopo só entra se o usuário pedir.
- Use a ferramenta de perguntas estruturadas quando houver opções discretas.
- Grave na raiz, no arquivo que o projeto já usa. Se não houver, crie
  `PRD.md` e `TDD.md`. Crie o `DESIGN.md` só na etapa de design. Um
  `design.md` já existente é o arquivo de design: use-o, não crie outro.
- Greenfield, arquivo ausente ou vazio: mostre o texto e grave ao fim da etapa.
- Brownfield: o rascunho é lido do repositório. Mostre e grave só com
  confirmação, mesmo com o arquivo vazio.
- Arquivo com conteúdo: mostre o que muda e grave só com confirmação.
  Não apague decisão anterior que esta conversa não revisou.
- Sem frontend no escopo, a etapa de design não existe: não a sugira, não
  pergunte estilo e não crie `DESIGN.md`.

## Fluxo

```
Progresso:
- [ ] 0. Terreno e onde o projeto se encaixa
- [ ] 1. PRD
- [ ] 2. TDD e stack
- [ ] 3. Revisão e validação
- [ ] 4. DESIGN.md — só com frontend no escopo
- [ ] 5. Requisitos de negócio no BDD.md
- [ ] 6. Oferecer o plano
```

### 0. Terreno e onde o projeto se encaixa

**Terreno.** Antes de perguntar, olhe a árvore do repositório.

Não conte como código de produto: `AGENTS.md`, `CLAUDE.md`, `HARNESS.md`,
documentos de referência vazios (`PRD.md`, `TDD.md`, `DESIGN.md`, `BDD.md`,
`bdd/`), README que não descreve um produto, `.git`, configuração de agente,
`node_modules`, `dist`, `vendor` e lockfile de instalador. Template ou
scaffolding de ferramenta ainda sem código de domínio é ambíguo.

- **Greenfield.** Não há código de produto. A especificação nasce da conversa.
  A stack ainda não existe.
- **Brownfield.** Há código de produto: aplicação, biblioteca, serviço ou
  módulo de domínio. Antes de entrevistar, leia o bastante para rascunhar sem
  inventar: README, manifestos, pontos de entrada, pastas de código,
  interfaces expostas e, se existirem, testes e documentos. Prefira o
  Octocode local; sem ele, leia o repositório direto. Não pare por falta de
  Octocode neste passo.
- **Ambíguo.** Pergunte uma vez: **projeto novo (greenfield)** ou **projeto
  existente (brownfield)**.

Informe o terreno em uma frase e siga. Se o usuário corrigir, reclassifique
antes de continuar.

**Encaixe.** Se o repositório ainda não mostrar, pergunte:

- **Monorepo** — este repositório é o projeto inteiro
- **Projeto dentro de um maior** — este repositório é uma parte

Se workspaces, um único aplicativo ou uma referência explícita a um sistema
maior já responderem, registre e não pergunte.

**Monorepo.** Siga para o PRD. A especificação cobre este repositório. Em
brownfield com vários pacotes, o PRD e o TDD descrevem o conjunto em alto
nível; não abra um documento por pacote.

**Projeto dentro de um maior.** Peça uma das duas, se o repositório ainda não
tiver isso:

- a especificação em alto nível do projeto maior;
- os links dos repositórios dos projetos.

Com a especificação em alto nível, use-a como limite: o PRD deste
repositório só cobre o que cabe nele. Não reescreva o projeto maior.

Com os links, gere essa especificação pelo **Octocode** antes de entrevistar.
Para cada repositório, leia o README e os documentos de especificação que
existirem e confirme no código só o que eles não disserem. Repositório
GitHub entra pela busca e leitura remotas do Octocode; caminho local, pela
busca local. Escreva em alto nível o objetivo, as fronteiras entre os
repositórios e o que cabe neste. Mostre e peça confirmação. Sem Octocode
disponível, pare e diga o que falta. Não invente a especificação a partir
do link.

Não pergunte de novo, no PRD, o que essa especificação ou o código já
respondeu.

### 1. PRD

**Greenfield.** Comece pelo projeto, não pela solução. Na primeira rodada,
pergunte só:

- qual é o objetivo principal;
- qual problema resolve e para quem.

Se ainda não der para escrever o PRD sem inventar, continue em rodadas curtas
até cobrir resultado esperado, o que entra, o que fica de fora e como o
usuário percebe que funcionou. Pare assim que der para redigir. Mostre o PRD
e grave.

**Brownfield.** Não abra com esse questionário. Redija o PRD a partir do que
leu: comportamento visível, README, textos de interface, rotas, ajuda de CLI.
Objetivo, público e fora de escopo que o código não prova viram pergunta, não
frase afirmada. Detalhe de implementação não vira requisito. Mostre o
rascunho e grave só com confirmação.

```markdown
# PRD — <nome do projeto>

## Problema
<quem sofre, com o quê>

## Objetivo
<resultado principal, em uma ou duas frases>

## Para quem
<quem usa>

## Escopo
- **Inclui:** <...>
- **Fora de escopo:** <...>

## Requisitos
1. <o produto deve ...>

## Regras de negócio
- <regra>

## Como saber que funcionou
- <sinal observável>

## Riscos
- <...>

## Suposições
- <uma linha, só se houver>

## No projeto maior
<só se este repositório for uma parte: o que o projeto maior faz e o que fica neste repositório>
```

Omita a seção que não tiver conteúdo.

### 2. TDD e stack

Só comece com o PRD já gravado nesta conversa ou já existente no repositório.
Se o PRD não existir, volte à etapa 1.

**Greenfield.** Antes de sugerir tecnologia ou escrever o TDD, pergunte, sem
pular:

- **Definir a stack manualmente**
- **Propor a stack a partir do PRD**

Não marque uma das duas como recomendada: a escolha é do usuário.

**Manual.** Pergunte a stack em rodadas curtas (linguagem, interface, dados,
onde roda). Não complete o que ele não disse com uma escolha sua. Se faltar
uma peça sem a qual o TDD não fecha, pergunte só essa peça.

**Proposta.** Proponha uma stack enxuta justificada pelo PRD: problema,
público, escopo e restrições. Para cada escolha, uma frase de porquê. Não
acrescente ferramenta que o PRD não pede. Mostre a proposta e grave-a no TDD
como a stack escolhida por essa opção.

Em seguida preencha o TDD com a stack da opção escolhida.

**Brownfield.** Não pergunte se a stack será manual ou proposta. A stack é a
do repositório: preencha a tabela com o que o código usa e, na coluna
"Por quê", escreva "já presente no repositório". Pergunte só a camada que o
código não revela. Não proponha troca. Se o usuário pedir outra stack, o TDD
passa a descrever o alvo, a stack atual entra como restrição e a troca fica
em Decisões técnicas.

Arquitetura, componentes, dados, interfaces, configuração e segurança ficam
aqui. Em brownfield, saem do código: não desenhe uma arquitetura nova no
lugar da que existe. Motivo de produto fica no PRD.

```markdown
# TDD — <nome do projeto>

## Stack
| Camada | Escolha | Por quê |
|--------|---------|---------|
| <...> | <...> | <ligado ao PRD, "definido pelo usuário" ou "já presente no repositório"> |

## Arquitetura
<como as partes se relacionam>

## Componentes
- <responsabilidade>

## Dados
<o que se guarda e o que não se guarda>

## Interfaces
<por onde o produto se expõe>

## Configuração e segurança
<só o que a stack e o PRD exigem>

## Decisões técnicas
| Decisão | Alternativa afastada | Motivo |
|---------|----------------------|--------|
```

### 3. Revisão e validação

Com PRD e TDD preenchidos, decida se há frontend no escopo: tela, página,
aplicação web ou mobile, ou outra interface visual para uma pessoa. API,
CLI, biblioteca, job ou serviço sem interface não contam. Em brownfield, a
interface conta se já existe no código, não se alguém poderia acrescentá-la.

**Sem frontend.** Não mencione design. Pergunte só se o usuário quer
revisar e validar. Depois da revisão, ou se ele não quiser revisar, vá
para a etapa 5 sem falar de design.

**Com frontend.** Pergunte:

- **Revisar e validar** os dois documentos
- **Seguir para o design**

**Revisar.** Entregue um resumo cruzado, não uma segunda cópia dos arquivos.
Para cada objetivo ou requisito do PRD, diga como o TDD o atende. Depois
destaque, em lista curta, os **pontos críticos e as decisões técnicas** que
dependem da aprovação do usuário: escopo, stack, dados, segurança e qualquer
ponto em que o TDD restringe o que o PRD promete. Em brownfield, marque também
o que o rascunho afirma e o código não mostra.

Espere. Se ele ajustar, atualize o arquivo dono daquele fato e mostre de
novo só o que mudou no cruzamento. Com ponto crítico ainda em aberto, não
avance. Sem frontend, vá para a etapa 5 sem falar de design.

**Seguir.** Vá para a etapa 4. As decisões da etapa 2 permanecem como estão.
Se nesta hora o escopo não tiver frontend, não ofereça este caminho: vá
para a etapa 5 sem falar de design.

### 4. DESIGN.md

Não entre aqui sem frontend no escopo. Se não houver, vá para a etapa 5
sem sugerir esta.

**Greenfield.** Peça o estilo visual ou uma referência. A referência pode ser
uma descrição, um arquivo ou uma página.
[getdesign.md](https://getdesign.md) é um exemplo de catálogo: cada item é um
`DESIGN.md` de um produto real, com cores, tipo, espaçamento e componentes.
Se o usuário indicar uma URL ou um arquivo, leia antes de escrever. Sem
referência, proponha um estilo a partir do PRD e confirme com o usuário
antes de gravar.

**Brownfield.** Leia o estilo já presente: tokens, CSS, biblioteca de
componentes, telas. Proponha o `DESIGN.md` a partir disso e confirme antes de
gravar. Referência externa só se o usuário pedir para mudar o visual. O que
não estiver no código não entra.

O `DESIGN.md` gerado é orientado pelo PRD e pelo TDD, não pela referência.

- Adote da referência só o que combina com o público, o tom, a plataforma e
  as restrições já escritas.
- O que contradizer o PRD ou o TDD fica de fora. Diga o que ficou de fora e
  por quê.
- Não prescreva recurso visual que a interface escolhida no TDD não comporta.

Grave o `DESIGN.md` com cores, tipografia, espaçamento, componentes e o
raciocínio de cada escolha. Cada escolha aponta para o trecho do PRD ou do
TDD que a sustenta. Em brownfield, aponte também onde o código já usa essa
escolha.

Siga para a etapa 5.

### 5. Requisitos de negócio no BDD.md

Pergunte se o usuário quer gerar os requisitos de negócio para preencher o
`BDD.md`:

- **Gerar os requisitos de negócio**
- **Não gerar agora**

**Não gerar.** Vá para a etapa 6. Não crie cenário nem teste.

**Gerar.** Extraia os requisitos de negócio do PRD. Não invente regra que
não esteja nele nem no que o usuário acabou de decidir. O TDD só informa
por onde o produto se expõe; o cenário descreve o comportamento de negócio,
não a implementação. Em brownfield, o cenário descreve comportamento que o
PRD confirmou e que o produto já tem. Teste existente informa o
comportamento; não copie o teste para o cenário. Se `bdd/` já tiver cenário
equivalente, não duplique.

Mostre o rascunho e grave só com a confirmação. Se o `BDD.md` ou `bdd/` já
tiverem conteúdo, mostre o que entra e não apague cenário existente sem
confirmação.

O `BDD.md` é o índice. Cada caso de uso ganha um arquivo em `bdd/`, em
Gherkin e em português. ID estável (`@US-01.1`). O que não dá para
automatizar leva `@manual`. Não escreva testes agora: estes cenários ficam
prontos para uma geração de testes futura.

```markdown
# BDD

Índice dos casos de uso. Os cenários em `bdd/` são a fonte da geração de
testes futura. Cenário com `@manual` não vira teste automático.

| ID | Caso de uso | Arquivo |
|----|-------------|---------|
| US-01 | <nome> | [bdd/us-01.md](bdd/us-01.md) |
```

```gherkin
# language: pt
Funcionalidade: <caso de uso>

  @US-01.1
  Cenário: <requisito de negócio observável>
    Dado <contexto>
    Quando <ação>
    Então <resultado>
```

Siga para a etapa 6.

### 6. Oferecer o plano

Ao terminar, pergunte se o usuário quer gerar um plano de implementação a
partir do PRD, do TDD e, se existirem, do `DESIGN.md` e do `BDD.md`.

- **Se sim:** o plano sai do **modo de planejamento nativo do agente** em uso,
  com esses documentos como insumo.
  - **Agente que entra no modo sozinho:** entre no modo de planejamento.
  - **Agente que não entra no modo sozinho:** **não** escreva o plano. Pare,
    informe o comando do agente e entregue a especificação pronta para colar
    depois dele.

  Cada agente tem o seu jeito de entrar no modo de planejamento (em geral
  chamado *Plan mode*, ou um comando `/plan`). Se não souber como ativá-lo no
  agente em uso, consulte a documentação dele ou peça ao usuário para ativar.

  **Não** use skill para gerar o plano, nem esta nem outra. Escrever o plano
  no chat também não substitui o modo de planejamento.
- **Se não:** encerre. Não implemente nada.
