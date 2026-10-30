---
name: atualizar-projeto
description: >-
  Atualiza o AGENTS.md do projeto para o modelo mais novo da skill
  iniciar-projeto, sem refazer a iniciação. Use quando o usuário atualizar as
  skills e pedir para atualizar o projeto, o AGENTS.md ou as instruções de
  agente conforme o template mais recente.
---

# Atualizar Projeto

Traz o `AGENTS.md` da raiz para o modelo atual da skill `iniciar-projeto`.
Não inicia o projeto de novo: não cria documentos de referência, não mexe em
`HARNESS.md` sem confirmação e não instala ferramentas. Explicações ao
usuário em **português (Brasil)**.

O modelo é a fonte do conteúdo. Copie-o byte a byte. Não reescreva, resuma
nem adapte ao produto. Produto, stack e arquitetura ficam na documentação do
projeto, nunca no `AGENTS.md`.

## Fluxo

```
Progresso:
- [ ] 1. Localizar a raiz e o modelo mais novo
- [ ] 2. Conferir se o projeto já foi iniciado
- [ ] 3. Atualizar o AGENTS.md
- [ ] 4. Conferir o CLAUDE.md
- [ ] 5. Relatar
```

### 1. Localizar a raiz e o modelo mais novo

```bash
ROOT=$(git rev-parse --show-toplevel 2>/dev/null || pwd)
```

O modelo é `assets/AGENTS.template.md` ao lado do `SKILL.md` de
`iniciar-projeto`. Procure nesta ordem:

1. A cópia de `iniciar-projeto` que este agente carregou.
2. `.agents/skills/iniciar-projeto/assets/AGENTS.template.md`
3. `.claude/skills/iniciar-projeto/assets/AGENTS.template.md`
4. A skill global do agente, se não houver cópia no repositório.

Se houver mais de uma cópia e o conteúdo diferir, use a de modificação mais
recente e diga quais ficaram de fora. Se nenhuma existir, pare e peça para
atualizar ou reinstalar a skill `iniciar-projeto`.

### 2. Conferir se o projeto já foi iniciado

| Situação | Ação |
|----------|------|
| Não existe `AGENTS.md` | Pare. O projeto ainda não foi iniciado. Peça para rodar `iniciar-projeto` |
| Idêntico ao modelo | O passo 3 não altera o arquivo. Siga para o passo 4 |
| Diferente | Siga para o passo 3 |

Compare com `git diff --no-index -- "$ROOT/AGENTS.md" <modelo>`.

### 3. Atualizar o AGENTS.md

Mostre o diff antes de gravar.

Separe o que no `AGENTS.md` atual não está no modelo:

- **Sem trecho próprio.** Substitua o arquivo pelo modelo, byte a byte.
- **Trecho próprio** (regra que o projeto acrescentou). Não deixe no
  `AGENTS.md` o que contradiz o modelo ou repete o que ele já diz. Prefira
  mover para o `HARNESS.md`. Só mova ou mantenha com a confirmação do
  usuário. Produto, stack e arquitetura não entram: aponte o documento do
  projeto e não copie para o `AGENTS.md`.

Ao mesclar, parta do modelo e preserve só o acréscimo confirmado que não
contradiz nem duplica o modelo.

Não altere o arquivo do modelo. Não apague conteúdo do `HARNESS.md`. Se for
mover uma regra para lá, acrescente ao fim e preserve o que já existe.

### 4. Conferir o CLAUDE.md

O Claude Code lê o `AGENTS.md` pela linha `@AGENTS.md`.

| Situação | Ação |
|----------|------|
| Não existe | Crie `$ROOT/CLAUDE.md` com a única linha `@AGENTS.md` |
| Já contém `@AGENTS.md` | Nada a fazer |
| Existe sem a importação | Insira `@AGENTS.md` na primeira linha e preserve o resto |

### 5. Relatar

```markdown
# Projeto atualizado

- **Modelo:** <caminho do AGENTS.template.md usado>
- **AGENTS.md:** <já era o modelo | substituído | mesclado | mantido>
- **Trechos do projeto:** <nenhum | movidos ao HARNESS.md | mantidos no AGENTS.md | aguardando confirmação>
- **CLAUDE.md:** <criado | já importava o AGENTS.md | importação adicionada>
- **HARNESS.md:** <inalterado | regra acrescentada>
```

Não ofereça refazer `iniciar-projeto`. Documentação de produto e release são outro pedido.
