# Skills

Skills de agente reutilizáveis e independentes de projeto, em português (Brasil).
Funcionam no Claude Code, Cursor, Copilot, Devin Desktop e demais agentes
compatíveis com o formato `SKILL.md`.

Juntas, elas cobrem o fluxo de **desenvolvimento orientado a especificação
(SDD)**: a especificação e os critérios de aceite vêm antes do código, e cada
etapa termina com uma aprovação sua.

| Skill | Uso |
|-------|-----|
| [`implementar-sdd`](skills/implementar-sdd/SKILL.md) | Conduz uma funcionalidade pelo fluxo inteiro, da especificação à documentação commitada, com a opção de publicar a release no fim |
| [`iniciar-projeto`](skills/iniciar-projeto/SKILL.md) | Instala o `AGENTS.md` (a partir do [modelo padrão](skills/iniciar-projeto/assets/AGENTS.template.md)) e o `CLAUDE.md`, cria os documentos de referência vazios e confere o CLI do host, o Context7 e o Octocode |
| [`especificar-feature`](skills/especificar-feature/SKILL.md) | Entrevista você e escreve a especificação da funcionalidade, com os cenários BDD sugeridos. Não implementa |
| [`criar-testes`](skills/criar-testes/SKILL.md) | Cria os testes que faltam para os cenários de `bdd/` sem `@manual`, com o mesmo ID. Não altera cenário nem teste já existente |
| [`revisar-codigo`](skills/revisar-codigo/SKILL.md) | Revisa o código (duplicação, SOLID, arquitetura, cobertura dos cenários) e sugere melhorias sem implementar |
| [`atualizar-docs`](skills/atualizar-docs/SKILL.md) | Sincroniza `PRD.md`, `TDD.md` e `README.md` com o que foi implementado |
| [`commitar`](skills/commitar/SKILL.md) | Roda os testes, commita por assunto, sobe a versão e envia a branch, com a opção de abrir o pull request. Sem tag nem release |
| [`gerar-guardrails`](skills/gerar-guardrails/SKILL.md) | Gera o CI do pull request e a regra da branch que exige o CI verde (GitHub) |
| [`publicar-release`](skills/publicar-release/SKILL.md) | Confere que não há pendências de commit, cria e envia a tag de release. Não commita |
| [`gerar-skill`](skills/gerar-skill/SKILL.md) | Cria uma skill abstrata e reutilizável, independente do projeto e das tecnologias. Não implementa |

As skills leem o `AGENTS.md` do projeto e os documentos de referência dele
(`PRD.md`, `TDD.md`, `DESIGN.md`, `BDD.md` e `bdd/`, `HARNESS.md`, ou equivalentes); nada de
produto ou stack fica aqui. O que o projeto não tem simplesmente não se aplica.

## Documentos do projeto

| Documento | Papel |
|-----------|-------|
| `AGENTS.md` | Método de trabalho do agente, igual em qualquer projeto (vem do modelo) |
| `HARNESS.md` | Regras só deste projeto. Importado pelo `AGENTS.md`; em conflito, vale ele |
| `PRD.md` | O quê e por quê: escopo, requisitos, histórias, regras de negócio |
| `TDD.md` | Como: arquitetura, stack, interfaces, configuração |
| `DESIGN.md` | Estilo visual: cores, tipografia, espaçamento, componentes. A regra de segui-lo vale em todo projeto; o arquivo nasce vazio |
| `BDD.md` e `bdd/` | Critérios de aceite em Gherkin: o índice dos casos de uso e um arquivo por caso |

## Fluxo de SDD

Pedir para desenvolver ou implementar uma funcionalidade dispara
[`implementar-sdd`](skills/implementar-sdd/SKILL.md). Ela lê cada skill da etapa e
para nas aprovações. A ordem dela commita o código antes da documentação:

```
implementar-sdd: especificar-feature → plano → implementar → criar-testes
               → revisar-codigo → commitar → atualizar-docs
               → commitar a documentação | publicar-release
```

O passo a passo abaixo continua valendo para rodar uma etapa sozinha. `iniciar-projeto` e `gerar-guardrails` ficam de fora do atalho: são feitos uma vez.

```
iniciar-projeto → especificar-feature → plano → implementar → criar-testes
                → revisar-codigo → atualizar-docs → commitar → (gerar-guardrails) → publicar-release
```

### Como executar uma skill

Peça pelo nome. Onde o agente aceita comando de skill, use `/nome-da-skill`
(por exemplo `/especificar-feature`). Em qualquer agente, também funciona
escrever o pedido: "use a skill especificar-feature para a funcionalidade X".
`implementar-sdd` também começa sozinha quando o pedido é desenvolver ou
implementar uma funcionalidade.

### Passo a passo

| # | Etapa | Como executar | Resultado | Você aprova |
|---|-------|---------------|-----------|-------------|
| 0 | **Iniciar o projeto** (uma vez) | `/iniciar-projeto` na raiz do repositório | `AGENTS.md`, `CLAUDE.md`, `HARNESS.md`, `PRD.md`, `TDD.md`, `DESIGN.md`, `BDD.md` e `bdd/` criados (vazios, se não existiam); GitHub CLI, Context7 e Octocode conferidos | A substituição de um `AGENTS.md` existente, a instalação de ferramentas |
| 1 | **Especificar** | `/especificar-feature <o que você quer>` | Especificação: objetivo, escopo, requisitos, regras, decisões e **cenários BDD sugeridos** | As decisões críticas e a gravação dos cenários em `bdd/` |
| 2 | **Planejar** | Peça o plano ao fim da skill; o agente entra no **modo de planejamento** dele (não é uma skill) | Plano de implementação a partir da especificação | O plano |
| 3 | **Implementar** | Peça ao agente para implementar o plano aprovado | Código e **um teste por cenário** de `bdd/`, com o mesmo ID. Os cenários `@manual` viram checklist | — |
| 4 | **Criar os testes que faltam** | `/criar-testes` | Um teste novo por cenário de `bdd/` ainda sem teste. Os `@manual` continuam checklist | A lista de testes antes de gravar |
| 5 | **Revisar** | `/revisar-codigo` (indique o escopo: arquivo, pasta ou "o que mudou") | Relatório por criticidade, incluindo cenários sem teste. Não edita nada | Quais sugestões aplicar |
| 6 | **Atualizar a documentação** | `/atualizar-docs` | `PRD.md`, `TDD.md` e `README.md` alinhados ao código; cenários sem cobertura aparecem em "Pendências" | O texto alterado |
| 7 | **Commitar** | `/commitar` | Testes rodando, commits por assunto, versão subida em um commit próprio, branch enviada e, se quiser, PR aberto | O plano de commits |
| 8 | **Proteger a branch** (uma vez) | `/gerar-guardrails` | CI de pull request, portão de testes na release e regra que bloqueia o merge sem o CI verde | O plano e a criação da regra |
| 9 | **Publicar** | `/publicar-release` | Tag `vX.Y.Z` criada e enviada, que dispara o pipeline de release. Só roda com o working tree limpo; havendo pendência, manda rodar o `/commitar` | O plano da release e o checklist dos cenários `@manual` |

### Regras do fluxo

- **Especificação primeiro.** Nenhuma funcionalidade vai ao código sem
  especificação e plano aprovados.
- **Um teste por cenário.** Todo cenário de `bdd/` sem `@manual` tem um teste
  com o mesmo ID. O `criar-testes` escreve os que faltam; o `revisar-codigo`
  aponta o que ainda falhar.
- **O cenário não muda para o teste passar.** Alterar o comportamento esperado
  exige a sua aprovação.
- **Versão da release como fonte da verdade.** O `commitar` sobe a versão; o
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

### Migração dos nomes antigos

Quatro skills mudaram de nome. Em projetos que já as instalaram, reinstale com o
comando de instalação acima e apague as pastas antigas.

| Nome antigo | Nome novo |
|-------------|-----------|
| `configurar-agentes` | `iniciar-projeto` |
| `revisao-codigo` | `revisar-codigo` |
| `atualizar-documentacao` | `atualizar-docs` |
| `gerar-release` | `publicar-release` (só cria a tag; o commit por assunto e a versão ficam no `commitar`) |
