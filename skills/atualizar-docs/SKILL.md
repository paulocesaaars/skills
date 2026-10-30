---
name: atualizar-docs
description: >-
  Atualiza PRD.md, TDD.md e README.md para a foto atual do projeto, comparando
  o código com o que está documentado. PRD e TDD não recebem versão nem guardam
  histórico do que já foi. No início, pergunta se no final o usuário quer
  commitar ou publicar a release. Use quando o usuário pedir para atualizar,
  sincronizar ou revisar a documentação, o PRD, o TDD ou o README após mudanças
  no código.
---

# Atualizar Documentação

Mantém `PRD.md`, `TDD.md` e `README.md` fiéis ao código. Edita **apenas** esses
três arquivos e o marcador `last-reviewed-commit` — nunca código, testes ou
configuração. A etapa 8 é a exceção: se o usuário pediu, segue `versionar`
ou `publicar-release`. Todo o texto escrito é em **português (Brasil)**.

Se o repositório usar outros nomes para os documentos de produto, técnico e de
uso, aplique o fluxo a eles.

## Papel de cada documento

Cada fato mora em **um** documento; os outros apontam para ele.

| Documento | Conteúdo | Não deve conter |
|-----------|----------|-----------------|
| `PRD.md` | O quê e por quê, no presente: requisitos (RF), histórias (os critérios de aceite ficam no `BDD.md` e em `bdd/`, quando o projeto usa BDD), regras de negócio, escopo, riscos, fora de escopo | Nomes de módulos, detalhes de implementação, versão do documento, changelog, lista do que já foi entregue |
| `TDD.md` | Como, no presente: stack, componentes, estrutura de pastas, modelo de dados, pipelines, interfaces, configuração, segurança, desempenho, decisões vigentes | Justificativa de produto, passo a passo de uso, evolução, histórico de decisões |
| `README.md` | Uso atual: instalação, primeiros passos, comandos, interfaces expostas, configuração, publicação | Arquitetura interna, requisitos formais, changelog |

## Foto atual, sem histórico

`PRD.md` e `TDD.md` são a foto do projeto **agora**. Quem abrir o arquivo entende
o produto e o desenho técnico vigentes sem precisar saber o que já foi.

- Escreva no presente: o que o projeto é, faz e como está montado.
- Reescreva o trecho desatualizado. Não acrescente "agora também faz X", "a
  partir da versão Y" nem uma nota do que mudou.
- Não versione esses documentos: sem número de versão, data de revisão, status
  de entrega nem cabeçalho que registre a rodada.
- Não mantenha histórico do que já foi. Apague changelog, evolução, versões
  anteriores, "entregue", "feito em" e qualquer roadmap que só arquive o
  passado. O que deixou de existir sai do documento. O que continua vale como
  fato atual, não como item concluído.
- O que ainda não existe no código não entra como capacidade atual.

O `README.md` descreve o uso atual, no mesmo espírito: sem changelog.

## Fluxo

```
Progresso:
- [ ] 0. Perguntar se commita ou publica no final
- [ ] 1. Definir a base da última revisão
- [ ] 2. Levantar o que mudou no código
- [ ] 3. Confrontar mudanças com cada documento
- [ ] 4. Editar os documentos
- [ ] 5. Verificar consistência
- [ ] 6. Gravar a base revisada
- [ ] 7. Relatar
- [ ] 8. Commitar ou publicar, se o usuário pediu
```

### 0. Perguntar se commita ou publica no final

Antes de qualquer outra ação, pergunte o que fazer depois da documentação.
Esta skill pode preparar a documentação para uma publicação, independente de guardrails:

- **Commitar no final** com a skill `versionar`
- **Publicar a release no final** com a skill `publicar-release`
- **Não commitar nem publicar**

Guarde a resposta. Não commite no meio do fluxo.

Se esta skill estiver rodando como etapa de outra que já commita no fim,
não pergunte e não commite.

### 1. Definir a base

A base fica gravada no marcador `.agents/last-reviewed-commit` (uma linha com
o hash completo; o `HARNESS.md` pode indicar outro caminho). Ordem de escolha:

1. Commit, tag ou data indicados pelo usuário.
2. O hash do marcador, se existir e for ancestral de `HEAD`. Um hash que não é
   ancestral (marcador copiado de outro repositório, histórico reescrito) é
   ignorado.
3. Senão, o último commit que alterou algum dos três documentos.

```bash
MARK=.agents/last-reviewed-commit
BASE=$(cat "$MARK" 2>/dev/null)
if [ -z "$BASE" ] || ! git merge-base --is-ancestor "$BASE" HEAD 2>/dev/null; then
  BASE=$(git log -1 --format=%H -- PRD.md TDD.md README.md)
fi
if [ -n "$BASE" ]; then git log --oneline "$BASE"..HEAD; fi
```

Se nenhuma base for encontrada, faça uma revisão inicial completa do estado
atual dos arquivos e documentos; não use um intervalo Git com base vazia.

Se `BASE` for igual a `HEAD` e não houver mudanças não commitadas fora dos
documentos, informe que a documentação já está em dia e encerre.

- Se `PRD.md`, `TDD.md` ou `README.md` tiverem alterações não commitadas, elas
  são edições em andamento do usuário: **preserve-as** e trabalhe sobre elas.

### 2. Levantar o que mudou

```bash
[ -n "$BASE" ] && git diff --stat "$BASE"..HEAD -- . ':!*.md'
[ -n "$BASE" ] && git diff "$BASE"..HEAD -- . ':!*.md'
git status --porcelain
```

- Inclua mudanças não commitadas e arquivos não rastreados (`git diff`,
  `git status`). Ignore caches, artefatos de build e tudo que o `.gitignore`
  exclui.
- Leia os arquivos novos ou muito alterados por inteiro; o diff sozinho não
  mostra o contrato público.
- A versão do software fica no manifesto (`pyproject.toml`, `package.json`,
  `*.csproj`, `Cargo.toml` ou equivalente) e nas tags. Não copie essa versão,
  nem a data da revisão, para o `PRD.md` ou o `TDD.md`.

Sem base, inspecione os arquivos atuais e compare-os integralmente com os
documentos; não execute os comandos de diff acima com `BASE` vazia.

Classifique cada mudança relevante:

- **Capacidade** nova, alterada ou removida (comando, endpoint, ferramenta,
  tela, recurso exposto).
- **Contrato**: parâmetros, retornos, erros, chaves de configuração, padrões.
- **Estrutura**: módulos, camadas, dependências, modelo de dados.
- **Operação**: instalação, publicação, plataformas, requisitos.
- **Interna** sem efeito visível: geralmente não vai para nenhum documento, exceto
  se mudar arquitetura ou decisão registrada no `TDD.md`.

### 3. Confrontar com os documentos

Para cada mudança, decida o documento dono (tabela acima) e procure o trecho
afetado com `rg` ou `git grep`. Pontos que costumam ficar defasados:

- **PRD.md**: requisitos vigentes, histórias (critérios de aceite só se o
  projeto não usa BDD), regras de negócio, escopo, riscos. Sem cabeçalho de
  versão, data ou status de entrega. Não mova item para uma lista de
  "Entregue": incorpore ao estado atual ou apague.
- **TDD.md**: stack e versões de dependências, árvore da estrutura do projeto,
  componentes, modelo de dados, pipelines, interfaces, exemplos de
  configuração, segurança, desempenho, decisões arquiteturais vigentes. Sem
  seção de evolução ou de decisões que já não valem.
- **README.md**: instalação, primeiros passos, comandos e flags, lista de
  capacidades expostas, configuração, publicação.

Valide também o inverso: algo documentado que **não existe mais** no código deve
ser removido ou corrigido.

Se o projeto usa BDD (`BDD.md` preenchido e cenários em `bdd/`), não copie critérios de aceite para os
documentos. Se uma mudança altera comportamento sem cenário correspondente,
registre em **Pendências** e sugira o cenário; não grave em `bdd/`.

### 4. Editar

- Toda afirmação nova deve ser verificada no código (nome exato de comando,
  flag, chave de configuração, valor padrão). Não documente o que não está
  implementado.
- Mantenha estilo, tom, numeração (RF-NN, seções) e formatação de cada documento.
  Novos requisitos recebem o próximo número livre; não renumere os existentes.
  Requisito que não descreve mais o projeto sai por inteiro, sem anotação de
  removido, entregue ou substituído.
- Se o `PRD.md` ou o `TDD.md` tiverem versão, data de revisão, changelog, lista
  de entregas ou evolução, remova isso na mesma edição e deixe só a foto atual.
- Se uma mudança for ambígua ou contradizer o PRD (comportamento diferente do
  requisito), **não decida sozinho**: registre no relatório e pergunte se o
  código ou o documento está certo.

### 5. Verificar consistência

- Mesma contagem e mesmos nomes de capacidades e comandos nos três documentos.
- Mesmas chaves e padrões de configuração entre `TDD.md` e `README.md`.
- Links relativos entre documentos continuam válidos.
- Nenhum fato duplicado em detalhe em mais de um documento.

### 6. Gravar a base revisada

Se houver divergência ou decisão pendente que impeça concluir a revisão,
não avance o marcador: relate os pontos pendentes. Só após resolver as
pendências, grave o `HEAD` atual, sobrescrevendo o valor anterior:

```bash
mkdir -p .agents
git rev-parse HEAD > .agents/last-reviewed-commit
```

Mudanças não commitadas que entraram nesta revisão aparecerão de novo no
próximo diff, depois de commitadas. Confirme rapidamente que já estão
documentadas e siga em frente.

### 7. Relatar

```markdown
# Documentação atualizada — <BASE curto>..<HEAD curto>

## Mudanças no código consideradas
- <mudança> → <documento(s) atualizado(s)>

## Alterações por documento
- **PRD.md:** <seções alteradas>
- **TDD.md:** <seções alteradas>
- **README.md:** <seções alteradas>

## Pendências
- <divergência código × PRD, ambiguidade ou decisão que cabe ao usuário>
```

Omita "Pendências" se não houver nenhuma.

### 8. Commitar ou publicar, se o usuário pediu

Localize a skill pelo nome nas pastas do repositório (`.claude/skills`,
`.agents/skills`) e, se faltar, nas globais. Leia o `SKILL.md` e siga-o.
Não reinterprete as regras dela.

**Commitar.** Siga `versionar`. Pule o passo em que ela oferece atualizar a
documentação: ela acabou de ser feita. Siga o restante, inclusive a oferta
de pull request.

**Publicar a release.** A release só sai com a árvore limpa. Se esta skill
alterou arquivos, commite só esses arquivos pelos passos 3 a 8 de `versionar`,
sem subir a versão de novo e sem oferecer de novo a documentação. Depois siga
`publicar-release` por inteiro.

**Não commitar nem publicar.** Encerre depois do relatório.
