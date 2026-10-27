# Skills

Skills de agente reutilizáveis e independentes de projeto, em português (Brasil).
Funcionam no Claude Code, Cursor, Copilot e demais agentes compatíveis com o
formato `SKILL.md`.

| Skill | Uso |
|-------|-----|
| [`revisao-codigo`](skills/revisao-codigo/SKILL.md) | Revisa código (duplicação, SOLID, arquitetura) e sugere melhorias sem implementar |
| [`atualizar-documentacao`](skills/atualizar-documentacao/SKILL.md) | Sincroniza `PRD.md`, `TDD.md` e `README.md` com o que foi implementado |
| [`gerar-release`](skills/gerar-release/SKILL.md) | Commita por assunto, sobe a versão, cria e envia a tag de release |
| [`configurar-agentes`](skills/configurar-agentes/SKILL.md) | Cria ou atualiza o `AGENTS.md` e o `CLAUDE.md` do projeto a partir do [modelo padrão](skills/configurar-agentes/assets/AGENTS.template.md) |

As skills leem `AGENTS.md`, `PRD.md` e `TDD.md` (ou equivalentes) do projeto
onde rodam; nada de produto ou stack fica aqui.

## Instalação

Na raiz do projeto. As skills são copiadas para `.claude/skills/` e
`.agents/skills/` e versionadas com o repositório; o `skills-lock.json` gerado
registra a origem e também deve ser commitado:

```bash
npx skills add paulocesaaars/skills -a claude-code -a codex -a cursor --copy -y
```

Para instalar para todos os projetos da máquina, sem versionar, acrescente `-g`.

## Atualização

Na raiz do projeto (acrescente `-g` para a instalação global):

```bash
npx skills update -y
```
