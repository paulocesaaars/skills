# Skills

Skills de agente reutilizáveis e independentes de projeto, em português (Brasil).
Funcionam no Claude Code, Cursor, Copilot, Devin Desktop e demais agentes
compatíveis com o formato `SKILL.md`.

Juntas, elas cobrem o fluxo de **desenvolvimento orientado a especificação
(SDD)**: a especificação e os critérios de aceite vêm antes do código, e aprovações são solicitadas nas etapas previstas em cada skill.

| Skill | Uso |
|-------|-----|
| [`implementar-sdd`](skills/implementar-sdd/SKILL.md) | Especifica, planeja e implementa uma funcionalidade. Depois pergunta se gera os testes do BDD, se revisa e se commita. Não publica a versão. No início, roda os testes; sem suíte, avisa e segue; se falharem, pergunta se a correção entra na especificação |
| [`iniciar-projeto`](skills/iniciar-projeto/SKILL.md) | Instala o `AGENTS.md` (a partir do [modelo padrão](skills/iniciar-projeto/assets/AGENTS.template.md)) e o `CLAUDE.md`, cria os documentos de referência vazios e confere o CLI do host, o Context7 e o Octocode |
| [`atualizar-projeto`](skills/atualizar-projeto/SKILL.md) | Traz o `AGENTS.md` ao modelo mais novo de `iniciar-projeto` depois que as skills são atualizadas. Não refaz a iniciação |
| [`especificar-projeto`](skills/especificar-projeto/SKILL.md) | Conduz a especificação do projeto: monorepo ou parte de um projeto maior, PRD, TDD com stack definida ou proposta e revisão cruzada opcional. O `DESIGN.md` só entra se houver frontend. Pode preencher o `BDD.md` com requisitos de negócio para testes futuros. Não implementa |
| [`especificar-feature`](skills/especificar-feature/SKILL.md) | Entrevista você e escreve a especificação da funcionalidade, com os cenários BDD sugeridos. Roda os testes no início; sem suíte, avisa e segue; se falharem, pergunta se a correção entra na especificação. Não implementa |
| [`criar-testes`](skills/criar-testes/SKILL.md) | Cria, quando solicitado, testes para cenários preenchidos de `bdd/` sem `@manual`, preservando a rastreabilidade por ID. Não altera cenário nem teste já existente |
| [`revisar-codigo`](skills/revisar-codigo/SKILL.md) | Roda os testes no início, revisa o código (duplicação, SOLID, arquitetura, cobertura dos cenários) e sugere melhorias sem implementar. Sem suíte, avisa e segue |
| [`atualizar-docs`](skills/atualizar-docs/SKILL.md) | Deixa `PRD.md`, `TDD.md` e `README.md` como a foto atual do código. PRD e TDD não versionam nem guardam histórico do que já foi. No início, pergunta se no final commita ou publica a release |
| [`versionar`](skills/versionar/SKILL.md) | Roda os testes no início, commita por assunto, sobe a versão e envia a branch, com a opção de abrir o pull request. Sem suíte, avisa e segue. Sem tag nem release |
| [`gerar-guardrails`](skills/gerar-guardrails/SKILL.md) | Gera, uma vez, o CI do pull request e a regra da branch que exige o CI verde (GitHub). Sem suíte de testes, avisa e não gera |
| [`publicar-release`](skills/publicar-release/SKILL.md) | Roda os testes no início, confere que não há pendências de commit, cria e envia a tag de release. Sem suíte, avisa e segue. Não commita |
| [`gerar-skill`](skills/gerar-skill/SKILL.md) | Cria uma skill abstrata e reutilizável, independente do projeto e das tecnologias. Não implementa |

As skills leem o `AGENTS.md` do projeto e os documentos de referência dele
(`PRD.md`, `TDD.md`, `DESIGN.md`, `BDD.md` e `bdd/`, `HARNESS.md`, ou equivalentes); nada de
produto ou stack fica aqui. O que o projeto não tem simplesmente não se aplica.

## Documentos do projeto

| Documento | Papel |
|-----------|-------|
| `AGENTS.md` | Método de trabalho do agente, igual em qualquer projeto (vem do modelo) |
| `HARNESS.md` | Convenção desta coleção para regras específicas do projeto, lidas por instrução no `AGENTS.md`; em conflito entre os dois, vale ele |
| `PRD.md` | O quê e por quê: escopo, requisitos, histórias, regras de negócio |
| `TDD.md` | Technical Design Document (documento de projeto técnico): arquitetura, stack, interfaces e configuração. Aqui, TDD não significa Test-Driven Development |
| `DESIGN.md` | Estilo visual: cores, tipografia, espaçamento, componentes. A regra de segui-lo vale em todo projeto; o arquivo nasce vazio |
| `BDD.md` e `bdd/` | Critérios de aceite em Gherkin: o índice dos casos de uso e um arquivo por caso |

BDD é uma prática de colaboração para esclarecer comportamento com exemplos;
Gherkin é uma linguagem possível para registrá-los. `BDD.md` e `bdd/` são
convenções desta coleção, não exigências gerais de BDD.

Release é uma versão publicada; tag é uma referência Git; deploy é a
implantação em um ambiente. Uma tag pode disparar publicação ou deploy, mas
isso depende do pipeline. Guardrails e release são opcionais; esta skill de
guardrails exige uma suíte de testes existente.

## Fluxo de SDD

Três fases. Preparar o projeto e gerar os guardrails acontecem uma vez.
A funcionalidade pode ir inteira pela [`implementar-sdd`](skills/implementar-sdd/SKILL.md)
ou skill por skill. A publicação de uma versão não começa sozinha no fim da funcionalidade.

### Preparar o projeto

Uma vez, no início, nesta ordem:

1. **Instalar as skills** com o comando da seção Instalação.
2. **`/iniciar-projeto`** na raiz. Cria `AGENTS.md`, `CLAUDE.md`, `HARNESS.md` e os documentos de referência vazios, e confere as ferramentas. Se isso já existir, não refaz.
3. **`/especificar-projeto`**. Preenche `PRD.md`, `TDD.md` e, se houver frontend, `DESIGN.md`. Se PRD e TDD já tiverem conteúdo, não refaz sem você pedir.

`/especificar-projeto` pergunta se o repositório é um monorepo ou um projeto dentro de um maior. No segundo caso, pede a especificação em alto nível ou os links dos repositórios e, com os links, monta o contexto pelo Octocode. A conversa segue nesta ordem:

1. **PRD.** Objetivo principal e problema que o projeto resolve.
2. **TDD.** Você define a stack ou pede uma proposta a partir do PRD.
3. **Revisão.** Validar o cruzamento dos dois documentos, ou seguir. Na validação, a skill destaca as decisões técnicas que dependem de você.
4. **DESIGN.md.** Só se houver frontend. Estilo ou referência, como um item do [getdesign.md](https://getdesign.md), coerente com o PRD e o TDD. Sem frontend, esta etapa não é sugerida.
5. **BDD.** Pergunta se gera os requisitos de negócio no `BDD.md` e em `bdd/`, para testes futuros. Não escreve testes.
6. **Plano.** Pergunta se você quer o plano no modo de planejamento do agente.

### Adicionar uma funcionalidade

Quando o agente oferece seleção automática, um pedido de desenvolvimento pode acionar [`implementar-sdd`](skills/implementar-sdd/SKILL.md). Ela especifica, planeja, implementa e então pergunta se você quer as etapas opcionais. Documentação e release ficam de fora.

No início ela roda os testes. Sem suíte, avisa que o funcionamento não foi validado e segue. Se falharem, pergunta se a correção entra na especificação.

| # | Etapa | Obrigatória | Como executar sozinho |
|---|-------|-------------|------------------------|
| 1 | **Especificar a funcionalidade** | sim | `/especificar-feature` |
| — | Planejar e implementar | sim, dentro da `implementar-sdd` | O plano sai no fim da especificação; a implementação não é uma skill |
| 2 | **Gerar os testes do BDD** | não | `/criar-testes` |
| 3 | **Revisar o código** | não | `/revisar-codigo` |
| 4 | **Preparar commits, versão e envio** | não | `/versionar` |

### Preparar e publicar uma versão

Não faz parte da funcionalidade. A ordem é:

1. **`/gerar-guardrails`** (opcional, uma vez). Só se o projeto tiver testes. Sem suíte, avisa e não gera. Se o CI e a regra da branch já exigirem teste verde, não roda de novo.
2. **`/atualizar-docs`**. Deixa `PRD.md` e `TDD.md` como a foto atual do projeto, sem versão nem histórico, e alinha o `README.md` ao uso atual.
3. **Commitar ou publicar.** A `atualizar-docs` pergunta no início qual dos dois, ou nenhum. Publicar a release commita antes a documentação pendente e então segue `/publicar-release`.

### Como executar uma skill

Peça pelo nome. Onde o agente aceita comando de skill, use `/nome-da-skill`
(por exemplo `/especificar-feature`). Em qualquer agente, também funciona
escrever o pedido: "use a skill especificar-feature para a funcionalidade X".
A seleção automática de `implementar-sdd` depende do agente e da sua configuração.

### Regras do fluxo

- **Preparação uma vez.** Instalar as skills, `iniciar-projeto` e
  `especificar-projeto` não se repetem. `gerar-guardrails` também é uma vez,
  e só com suíte de testes.
- **Especificação primeiro.** Nenhuma funcionalidade vai ao código sem
  especificação e plano aprovados.
- **Testes sob pedido.** O agente só cria ou altera testes quando você pede
  ou aceita a sugestão. Para testes BDD, `BDD.md` deve estar preenchido e
  `bdd/` deve conter cenários; arquivos vazios apenas mostram a estrutura.
  IDs permitem rastrear cenários e testes sem exigir uma relação de 1:1.
  Executar testes existentes é uma verificação separada da criação de testes.
- **O cenário não muda para o teste passar.** Alterar o comportamento esperado
  exige a sua aprovação.
- **Versão da release como fonte da verdade.** O `versionar` sobe a versão; o
  `publicar-release` lê essa versão, cria a tag e **não publica** se houver
  mudanças pendentes de commit.
- **Host de código.** O GitHub é o caminho completo (`gh`). Em GitLab, Azure
  DevOps e afins, as skills usam o CLI do host quando existe, ou entregam os
  passos manuais; a `gerar-guardrails` só gera arquivos para o GitHub.

## Instalação

Na raiz do projeto. As skills são copiadas para `.claude/skills/` e
`.agents/skills/` e versionadas com o repositório; o `skills-lock.json`
gerado registra a origem e também deve ser commitado. O Devin lê as skills
do projeto em `.agents/skills/`.

```bash
npx skills add paulocesaaars/skills -a claude-code -a cursor -a github-copilot --copy -y
```

Para instalar para todos os projetos da máquina, sem versionar, acrescente `-g`.
Para instalar só uma skill, acrescente `-s nome-da-skill`.

## Atualização

Na raiz do projeto (acrescente `-g` para a instalação global):

```bash
npx skills update -y
```

Depois, no projeto que já foi iniciado, rode `/atualizar-projeto` para trazer o `AGENTS.md` ao modelo mais novo de `iniciar-projeto`.
