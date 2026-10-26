# Skills

Skills de agente reutilizáveis e independentes de projeto, em português (Brasil).
Funcionam no Claude Code, Codex, Cursor e demais agentes compatíveis com o
formato `SKILL.md`.

| Skill | Uso |
|-------|-----|
| [`revisao-codigo`](skills/revisao-codigo/SKILL.md) | Revisa código (duplicação, SOLID, arquitetura) e sugere melhorias sem implementar |
| [`atualizar-documentacao`](skills/atualizar-documentacao/SKILL.md) | Sincroniza `PRD.md`, `TDD.md` e `README.md` com o que foi implementado |
| [`gerar-release`](skills/gerar-release/SKILL.md) | Commita por assunto, sobe a versão, cria e envia a tag de release |

As skills leem `AGENTS.md`, `PRD.md` e `TDD.md` (ou equivalentes) do projeto
onde rodam; nada de produto ou stack fica aqui.

## Instalação

Global (vale para todos os projetos da máquina):

```bash
npx skills add paulocesaaars/skills -g -a claude-code -a codex -a cursor --copy -y
```

Só no projeto atual (versionadas junto com o repositório):

```bash
npx skills add paulocesaaars/skills -a claude-code -a codex -a cursor --copy -y
```

## Atualização

```bash
npx skills update -g -y
```
