# 📚 Git — Guia Prático de Comandos

> Guia pessoal de consulta rápida para Git, organizado por **situação → comando → exemplo → cuidados**.
>
> **Objetivo:** estudar Git e encontrar rapidamente o comando certo durante o trabalho, com exemplos prontos para copiar.

---

## 🧭 Como usar este guia

- Use o **índice** para navegar por assunto.
- Os comandos estão em blocos `bash`, então podem ser copiados diretamente.
- Antes de executar comandos destrutivos, confira o estado do repositório com `git status`.
- Substitua valores como `NOME_DA_BRANCH`, `HASH_DO_COMMIT` e `URL_DO_REPOSITORIO` pelos valores reais.
- Quando houver `⚠️`, leia a observação antes de executar o comando.

> 💡 **Dica:** o GitHub gera links automáticos para títulos, mas este guia usa âncoras nomeadas no índice para manter a navegação estável mesmo quando um título contém emojis, acentos ou formatação.

---

## 📑 Índice

- [1. Modelo mental do Git](#sec-1)
- [2. Verificar instalação e ajuda](#sec-2)
- [3. Configuração do Git](#sec-3)
- [4. Criar ou clonar um repositório](#sec-4)
- [5. Consultar o estado do projeto](#sec-5)
- [6. Alterações e staging](#sec-6)
- [7. Commits](#sec-7)
- [8. Desfazer alterações: restore × reset × revert](#sec-8)
- [9. Branches](#sec-9)
- [10. Repositórios remotos](#sec-10)
- [11. Fetch × Pull × Push](#sec-11)
- [12. Merge](#sec-12)
- [13. Resolver conflitos](#sec-13)
- [14. Stash](#sec-14)
- [15. Histórico e investigação](#sec-15)
- [16. Tags](#sec-16)
- [17. .gitignore](#sec-17)
- [18. Remover arquivos já rastreados](#sec-18)
- [19. Recuperação com reflog](#sec-19)
- [20. Fluxos práticos do dia a dia](#sec-20)
- [21. Comandos perigosos](#sec-21)
- [22. Referência rápida](#sec-22)

---

<!-- Anchor: sec-1 -->
<a name="sec-1"></a>

# 1. 🧠 Modelo mental do Git

Antes dos comandos, pense no Git em quatro áreas:

```text
┌─────────────────────┐
│ Working Tree        │  ← arquivos que você está editando
└──────────┬──────────┘
           │ git add
           ▼
┌─────────────────────┐
│ Staging / Index     │  ← alterações preparadas para o commit
└──────────┬──────────┘
           │ git commit
           ▼
┌─────────────────────┐
│ Local Repository    │  ← histórico de commits
└──────────┬──────────┘
           │ git push
           ▼
┌─────────────────────┐
│ Remote Repository   │  ← GitHub / GitLab / etc.
└─────────────────────┘
```

Comandos principais:

```bash
git status
git add .
git commit -m "mensagem"
git push
```

Para trazer informações do remoto:

```bash
git fetch
```

Para buscar e integrar alterações:

```bash
git pull
```

---

<!-- Anchor: sec-2 -->
<a name="sec-2"></a>

# 2. 🔎 Verificar instalação e ajuda

### Ver versão instalada

```bash
git --version
```

### Ver ajuda geral

```bash
git help
```

### Ajuda de um comando

```bash
git help <comando>
```

Exemplo:

```bash
git help restore
```

Ou, de forma rápida:

```bash
git <comando> --help
```

---

<!-- Anchor: sec-3 -->
<a name="sec-3"></a>

# 3. ⚙️ Configuração do Git

## Configuração global

A configuração global normalmente é usada como padrão para seus repositórios.

### Listar configurações

```bash
git config --global --list
```

### Definir nome

```bash
git config --global user.name "Seu Nome"
```

### Definir e-mail

```bash
git config --global user.email "seu.email@exemplo.com"
```

### Remover nome

```bash
git config --global --unset user.name
```

### Remover e-mail

```bash
git config --global --unset user.email
```

## Configuração local

A configuração local vale somente para o repositório atual.

### Listar

```bash
git config --local --list
```

### Definir nome

```bash
git config --local user.name "Seu Nome"
```

### Definir e-mail

```bash
git config --local user.email "seu.email@exemplo.com"
```

### Remover

```bash
git config --local --unset user.name
git config --local --unset user.email
```

### Ver de onde cada configuração veio

Muito útil para descobrir por que um nome/e-mail está sendo usado:

```bash
git config --list --show-origin
```

---

<!-- Anchor: sec-4 -->
<a name="sec-4"></a>

# 4. 📦 Criar ou clonar um repositório

## Criar um repositório local

Dentro da pasta do projeto:

```bash
git init
```

## Clonar um repositório existente

```bash
git clone URL_DO_REPOSITORIO
```

Exemplo:

```bash
git clone https://github.com/usuario/projeto.git
```

Entrar na pasta:

```bash
cd projeto
```

---

<!-- Anchor: sec-5 -->
<a name="sec-5"></a>

# 5. 🔍 Consultar o estado do projeto

## Ver situação atual

```bash
git status
```

> ⭐ **Um dos comandos mais importantes do Git.** Use antes de executar operações que alteram o histórico ou descartam alterações.

## Ver alterações não preparadas para commit

```bash
git diff
```

## Ver alterações que já estão no staging

```bash
git diff --staged
```

Também pode ser usado:

```bash
git diff --cached
```

## Ver arquivos alterados de forma resumida

```bash
git status --short
```

---

<!-- Anchor: sec-6 -->
<a name="sec-6"></a>

# 6. 📥 Alterações e staging

## Adicionar um arquivo

```bash
git add caminho/do/arquivo
```

## Adicionar vários arquivos

```bash
git add arquivo1.py arquivo2.py
```

## Adicionar todas as alterações

```bash
git add .
```

> ⚠️ Antes de usar `git add .`, confira `git status` para evitar adicionar arquivos que não deveriam fazer parte do commit.

## Remover um arquivo do staging

Forma recomendada e explícita:

```bash
git restore --staged caminho/do/arquivo
```

Alternativa tradicional:

```bash
git reset caminho/do/arquivo
```

> Ambos removem o arquivo do staging sem apagar suas alterações da working tree.

## Adicionar partes de um arquivo

Quando você quer escolher apenas determinados trechos:

```bash
git add -p
```

---

<!-- Anchor: sec-7 -->
<a name="sec-7"></a>

# 7. 📝 Commits

## Criar commit

```bash
git commit -m "mensagem do commit"
```

Exemplo:

```bash
git commit -m "Adiciona validação do formulário"
```

## Ver o último commit

```bash
git show HEAD
```

## Alterar a mensagem do último commit

```bash
git commit --amend -m "Nova mensagem do commit"
```

> ⚠️ Evite alterar commits que já foram compartilhados com outras pessoas, pois isso reescreve o histórico.

## Adicionar alterações ao último commit

```bash
git add arquivo.py
git commit --amend --no-edit
```

> ⚠️ O `--amend` modifica o último commit em vez de criar outro.

---

<!-- Anchor: sec-8 -->
<a name="sec-8"></a>

# 8. ↩️ Desfazer alterações: `restore` × `reset` × `revert`

Esta é uma das distinções mais importantes do Git.

| Comando | Principal finalidade | Altera histórico? |
|---|---|---:|
| `git restore` | Restaurar arquivos | Não |
| `git reset` | Mover/remodelar o histórico local | Sim |
| `git revert` | Criar um novo commit que desfaz outro | Não |

## `git restore`

### Descartar alterações de um arquivo

```bash
git restore caminho/do/arquivo
```

### Descartar alterações da working tree

```bash
git restore .
```

> ⚠️ As alterações não commitadas descartadas dessa forma podem ser perdidas.

### Restaurar arquivo do staging para a versão do `HEAD`

```bash
git restore --staged caminho/do/arquivo
```

### Restaurar staging e working tree a partir do `HEAD`

```bash
git restore --source=HEAD --staged --worktree caminho/do/arquivo
```

---

## `git reset`

### Tirar arquivo do staging

```bash
git reset caminho/do/arquivo
```

Preferencialmente, para deixar a intenção mais clara:

```bash
git restore --staged caminho/do/arquivo
```

### Remover o último commit, mantendo as alterações

```bash
git reset HEAD~1
```

### Remover o último commit e manter as alterações preparadas

```bash
git reset --soft HEAD~1
```

### Remover o último commit e descartar alterações

```bash
git reset --hard HEAD~1
```

> 🚨 **CUIDADO:** `--hard` pode descartar alterações não commitadas.

---

## `git revert`

Use quando você quer desfazer um commit por meio de **um novo commit**, preservando o histórico existente.

### Reverter um commit

```bash
git revert HASH_DO_COMMIT
```

Exemplo:

```bash
git revert a1b2c3d
```

Isso é especialmente apropriado quando o commit já foi compartilhado com outras pessoas.

### Reverter um merge

```bash
git revert -m 1 HASH_DO_MERGE
```

- `-m 1` seleciona o primeiro parent como linha principal.
- `HASH_DO_MERGE` é o commit de merge que será revertido.

---

<!-- Anchor: sec-9 -->
<a name="sec-9"></a>

# 9. 🌿 Branches

## Listar branches locais

```bash
git branch
```

## Listar branches locais e remotas

```bash
git branch -a
```

## Listar branches remotas

```bash
git branch -r
```

## Mostrar último commit de cada branch

```bash
git branch -v
```

## Criar uma branch

```bash
git branch NOME_DA_BRANCH
```

## Criar e trocar para a nova branch

Forma moderna e explícita:

```bash
git switch -c NOME_DA_BRANCH
```

Exemplo:

```bash
git switch -c feature/login
```

## Trocar de branch

```bash
git switch NOME_DA_BRANCH
```

Exemplo:

```bash
git switch main
```

## Voltar para a branch anterior

```bash
git switch -
```

## Renomear a branch atual

```bash
git branch -m NOVO_NOME
```

## Renomear outra branch

```bash
git branch -m NOME_ANTIGO NOVO_NOME
```

## Excluir branch local

```bash
git branch -d NOME_DA_BRANCH
```

Se precisar forçar a exclusão:

```bash
git branch -D NOME_DA_BRANCH
```

> ⚠️ `-D` força a exclusão mesmo quando o Git entende que existem commits que podem ser perdidos.

## Excluir branch remota

```bash
git push origin --delete NOME_DA_BRANCH
```

---

<!-- Anchor: sec-10 -->
<a name="sec-10"></a>

# 10. 🌐 Repositórios remotos

## Adicionar remoto

```bash
git remote add origin URL_DO_REPOSITORIO
```

Exemplo:

```bash
git remote add origin https://github.com/usuario/projeto.git
```

## Listar remotos

```bash
git remote -v
```

## Ver detalhes do remoto

```bash
git remote show origin
```

## Alterar URL do remoto

```bash
git remote set-url origin NOVA_URL
```

## Remover remoto

```bash
git remote remove origin
```

---

<!-- Anchor: sec-11 -->
<a name="sec-11"></a>

# 11. 🔄 Fetch × Pull × Push

## `git fetch`

Busca informações e objetos do remoto sem integrar automaticamente as alterações à sua branch atual.

```bash
git fetch
```

Buscar somente uma branch:

```bash
git fetch origin NOME_DA_BRANCH
```

Buscar todos os remotos:

```bash
git fetch --all
```

---

## `git pull`

Busca alterações do remoto e, conforme a configuração/estratégia utilizada, integra essas alterações à branch atual.

```bash
git pull
```

Especificando remoto e branch:

```bash
git pull origin NOME_DA_BRANCH
```

> 💡 Se você quer primeiro **ver o que existe no remoto sem alterar sua branch**, prefira `git fetch`.

---

## `git push`

Enviar a branch atual para o remoto configurado:

```bash
git push
```

Enviar uma branch específica:

```bash
git push origin NOME_DA_BRANCH
```

### Primeiro push de uma branch

```bash
git push -u origin NOME_DA_BRANCH
```

O `-u` configura a branch remota como upstream da branch local.

Depois disso:

```bash
git push
```

pode ser suficiente.

---

<!-- Anchor: sec-12 -->
<a name="sec-12"></a>

# 12. 🔀 Merge

O merge incorpora o histórico de uma branch na branch atual.

## Exemplo

Você está na `main`:

```bash
git switch main
```

Atualize a branch:

```bash
git pull
```

Faça o merge:

```bash
git merge NOME_DA_BRANCH
```

Exemplo:

```bash
git merge feature/login
```

Depois:

```bash
git push
```

> 📌 **Regra mental:** você deve estar na branch que receberá as alterações.

```text
feature/login ───────●────●
                         \
main ─────●────●─────────●  ← merge
```

---

<!-- Anchor: sec-13 -->
<a name="sec-13"></a>

# 13. ⚔️ Resolver conflitos

Quando o Git não consegue integrar automaticamente duas alterações:

```text
<<<<<<< HEAD
código da branch atual
=======
código da outra branch
>>>>>>> feature/login
```

## Passo a passo

### 1. Ver arquivos em conflito

```bash
git status
```

### 2. Abra os arquivos e resolva os conflitos

Remova os marcadores:

```text
<<<<<<<
=======
>>>>>>>
```

### 3. Adicione os arquivos resolvidos

```bash
git add caminho/do/arquivo
```

### 4. Finalize o merge

```bash
git commit
```

### Cancelar um merge em andamento

```bash
git merge --abort
```

> 💡 Antes de resolver manualmente, entenda quais alterações pertencem a cada branch.

---

<!-- Anchor: sec-14 -->
<a name="sec-14"></a>

# 14. 🧳 Stash

Use `stash` quando precisa guardar temporariamente alterações não commitadas e deixar a working tree limpa.

## Criar stash

```bash
git stash
```

Com uma descrição:

```bash
git stash push -m "WIP - formulário de login"
```

## Listar stashes

```bash
git stash list
```

## Aplicar o último stash sem removê-lo

```bash
git stash apply
```

## Aplicar um stash específico

```bash
git stash apply stash@{0}
```

## Aplicar e remover o stash

```bash
git stash pop
```

## Remover um stash

```bash
git stash drop stash@{0}
```

## Excluir todos os stashes

```bash
git stash clear
```

> ⚠️ `stash clear` remove todos os stashes armazenados.

---

<!-- Anchor: sec-15 -->
<a name="sec-15"></a>

# 15. 🔎 Histórico e investigação

## Histórico completo

```bash
git log
```

## Histórico resumido

```bash
git log --oneline
```

## Histórico com gráfico

```bash
git log --oneline --graph --decorate --all
```

⭐ **Comando excelente para entender branches e merges:**

```bash
git log --oneline --graph --decorate --all
```

## Últimos 10 commits

```bash
git log -10 --oneline
```

## Mostrar alterações de um commit

```bash
git show HASH_DO_COMMIT
```

## Descobrir quem alterou cada linha

```bash
git blame caminho/do/arquivo
```

> 💡 `git blame` mostra o commit associado às linhas do arquivo. Use como ferramenta de investigação, não para atribuir culpa.

## Ver diferença entre duas branches

```bash
git diff main..feature/login
```

## Ver commits de uma branch que não estão na outra

```bash
git log main..feature/login --oneline
```

---

<!-- Anchor: sec-16 -->
<a name="sec-16"></a>

# 16. 🏷️ Tags

Tags são úteis para marcar versões ou pontos importantes do histórico.

## Criar tag simples

```bash
git tag v1.0.0
```

## Criar tag anotada

```bash
git tag -a v1.0.0 -m "Versão 1.0.0"
```

> ⭐ Para versões de projeto, tags anotadas são uma opção apropriada.

## Listar tags

```bash
git tag
```

## Mostrar uma tag

```bash
git show v1.0.0
```

## Enviar uma tag

```bash
git push origin v1.0.0
```

## Enviar todas as tags

```bash
git push origin --tags
```

## Excluir tag local

```bash
git tag -d v1.0.0
```

## Excluir tag remota

```bash
git push origin --delete v1.0.0
```

---

<!-- Anchor: sec-17 -->
<a name="sec-17"></a>

# 17. 🚫 `.gitignore`

O `.gitignore` define arquivos e diretórios que o Git deve ignorar quando ainda não estão sendo rastreados.

Exemplo:

```gitignore
# Ambiente virtual
.venv/
venv/

# Variáveis de ambiente
.env
.env.*

# Python
__pycache__/
*.py[cod]

# Django
db.sqlite3
media/

# IDE
.vscode/
.idea/

# Sistema operacional
.DS_Store
Thumbs.db
```

## Verificar se um arquivo está sendo ignorado

```bash
git check-ignore -v caminho/do/arquivo
```

## Importante: `.gitignore` não remove arquivo já rastreado

Se um arquivo já foi adicionado ao Git, colocar o arquivo no `.gitignore` posteriormente não faz o Git parar de rastreá-lo.

Use:

```bash
git rm --cached caminho/do/arquivo
```

Para diretório:

```bash
git rm -r --cached caminho/do/diretorio
```

Depois:

```bash
git add .gitignore
git commit -m "Atualiza arquivos ignorados"
```

> 🚨 **SEGURANÇA:** se uma senha, chave de API, token ou outra credencial já foi enviada para um repositório remoto, removê-la do rastreamento não torna a credencial segura novamente. A credencial deve ser revogada/rotacionada e, conforme o caso, o histórico também precisa ser tratado.

---

<!-- Anchor: sec-18 -->
<a name="sec-18"></a>

# 18. 🧹 Remover arquivos já rastreados

## Remover arquivo do Git e do computador

```bash
git rm caminho/do/arquivo
```

## Remover arquivo somente do Git

```bash
git rm --cached caminho/do/arquivo
```

## Remover diretório do Git e do computador

```bash
git rm -r caminho/do/diretorio
```

## Remover diretório somente do Git

```bash
git rm -r --cached caminho/do/diretorio
```

Depois confirme:

```bash
git status
```

---

<!-- Anchor: sec-19 -->
<a name="sec-19"></a>

# 19. 🛟 Recuperação com `reflog`

O `reflog` é uma ferramenta extremamente útil para localizar referências anteriores do `HEAD` e recuperar situações em que você acredita ter perdido um commit.

## Ver reflog

```bash
git reflog
```

Exemplo:

```text
a1b2c3d HEAD@{0}: commit: Corrige formulário
e4f5g6h HEAD@{1}: reset: moving to HEAD~1
```

Você pode inspecionar um estado anterior:

```bash
git show e4f5g6h
```

E, se necessário, criar uma branch apontando para ele:

```bash
git switch -c recuperacao e4f5g6h
```

> ⚠️ O `reflog` é uma ferramenta de recuperação. Quanto antes você procurar uma referência perdida, melhor.

---

<!-- Anchor: sec-20 -->
<a name="sec-20"></a>

# 20. 🚀 Fluxos práticos do dia a dia

## 20.1 Começar uma nova funcionalidade

```bash
git switch main
git pull
git switch -c feature/nova-funcionalidade
```

Trabalhe normalmente:

```bash
git status
git diff
```

Prepare:

```bash
git add .
```

Confira:

```bash
git diff --staged
```

Commit:

```bash
git commit -m "Adiciona nova funcionalidade"
```

Envie:

```bash
git push -u origin feature/nova-funcionalidade
```

---

## 20.2 Atualizar sua branch com a `main`

Primeiro, verifique seu estado:

```bash
git status
```

Busque alterações:

```bash
git fetch origin
```

Troque para `main`:

```bash
git switch main
```

Atualize:

```bash
git pull
```

Volte para sua branch:

```bash
git switch feature/nova-funcionalidade
```

Integre a `main`:

```bash
git merge main
```

---

## 20.3 Fiz alterações, mas preciso trocar de branch

Se você ainda não quer fazer commit:

```bash
git stash push -m "Alterações temporárias"
```

Troque de branch:

```bash
git switch outra-branch
```

Depois volte:

```bash
git switch feature/nova-funcionalidade
```

Recupere as alterações:

```bash
git stash pop
```

---

## 20.4 Fiz um commit local errado

### Quero manter as alterações

```bash
git reset HEAD~1
```

### Quero manter as alterações no staging

```bash
git reset --soft HEAD~1
```

### Quero apagar tudo

```bash
git reset --hard HEAD~1
```

> 🚨 Use `--hard` somente quando tiver certeza de que as alterações podem ser descartadas.

---

## 20.5 Fiz um commit errado e já enviei para o remoto

Em vez de reescrever o histórico compartilhado:

```bash
git revert HASH_DO_COMMIT
```

Depois:

```bash
git push
```

---

## 20.6 Fiz um merge errado e ainda não fiz push

Se o merge ainda está apenas localmente e você quer retornar ao estado anterior:

```bash
git reset --hard HEAD~1
```

> ⚠️ Confirme primeiro com `git status` e `git log --oneline --graph --decorate --all`.

---

## 20.7 Fiz um merge errado e já fiz push

Primeiro identifique o commit de merge:

```bash
git log --oneline --graph --decorate
```

Depois, conforme o caso:

```bash
git revert -m 1 HASH_DO_MERGE
```

E envie:

```bash
git push
```

> ⚠️ Em um merge real, confirme qual parent representa a linha principal antes de executar o `revert -m`.

---

<!-- Anchor: sec-21 -->
<a name="sec-21"></a>

# 21. 🚨 Comandos perigosos

Tenha atenção especial com:

```bash
git reset --hard
```

```bash
git clean -fd
```

```bash
git branch -D NOME_DA_BRANCH
```

```bash
git stash clear
```

```bash
git push --force
```

```bash
git push --force-with-lease
```

### Regra prática

Antes de qualquer comando destrutivo:

```bash
git status
git log --oneline --graph --decorate --all
```

E, se necessário:

```bash
git reflog
```

### `--force` × `--force-with-lease`

Evite:

```bash
git push --force
```

Quando uma reescrita do histórico for realmente necessária, prefira avaliar:

```bash
git push --force-with-lease
```

O `--force-with-lease` possui verificações adicionais para reduzir o risco de sobrescrever alterações remotas que você ainda não viu.

> ⚠️ Mesmo `--force-with-lease` deve ser usado com entendimento do histórico compartilhado.

---

<!-- Anchor: sec-22 -->
<a name="sec-22"></a>

# 22. ⚡ Referência rápida

## 🟢 Consultar

```bash
git status
git diff
git diff --staged
git log --oneline
git log --oneline --graph --decorate --all
git show HASH_DO_COMMIT
```

## 🟢 Trabalhar

```bash
git add .
git commit -m "mensagem"
git switch NOME_DA_BRANCH
git switch -c NOME_DA_BRANCH
git merge NOME_DA_BRANCH
```

## 🟢 Remoto

```bash
git fetch
git pull
git push
git push -u origin NOME_DA_BRANCH
```

## 🟡 Desfazer

```bash
git restore arquivo
git restore --staged arquivo
git reset HEAD~1
git revert HASH_DO_COMMIT
```

## 🟡 Temporário

```bash
git stash
git stash list
git stash pop
```

## 🔴 Cuidado

```bash
git reset --hard HEAD~1
git clean -fd
git branch -D NOME_DA_BRANCH
git push --force
git stash clear
```

---

# 🧩 Mapa mental: "Qual comando devo usar?"

```text
QUERO...

├── Ver o que mudou?
│   ├── git status
│   ├── git diff
│   └── git diff --staged
│
├── Preparar alterações?
│   └── git add
│
├── Salvar no histórico?
│   └── git commit
│
├── Descartar alteração em arquivo?
│   └── git restore
│
├── Tirar algo do staging?
│   └── git restore --staged
│
├── Desfazer commit local?
│   └── git reset
│
├── Desfazer commit compartilhado?
│   └── git revert
│
├── Criar/trocar de branch?
│   └── git switch
│
├── Juntar branches?
│   └── git merge
│
├── Ver alterações do remoto sem integrar?
│   └── git fetch
│
├── Buscar e integrar alterações?
│   └── git pull
│
├── Enviar alterações?
│   └── git push
│
├── Guardar alterações temporariamente?
│   └── git stash
│
└── Recuperar algo aparentemente perdido?
    └── git reflog
```

---

## 📌 Regras de ouro

1. **`git status` antes de tomar decisões.**
2. **`git diff` antes de fazer commit.**
3. **`git diff --staged` antes de confirmar o commit.**
4. Prefira `git switch` para operações de branch e `git restore` para restauração de arquivos.
5. Use `git revert` quando precisar desfazer alterações que já foram compartilhadas.
6. Tenha muito cuidado com `git reset --hard`.
7. Evite `git push --force` em branches compartilhadas.
8. Nunca coloque senhas, tokens ou chaves de API no repositório.
9. `.gitignore` não apaga segredos que já entraram no histórico.
10. Commits pequenos e com mensagens claras facilitam investigação e recuperação.

---

## 🔗 Referências oficiais

- [Documentação oficial do Git](https://git-scm.com/docs)
- [Git Reference](https://git-scm.com/docs)
- [Documentação do GitHub — README](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-readmes)
- [GitHub — Best practices for repositories](https://docs.github.com/en/repositories/creating-and-managing-repositories/best-practices-for-repositories)

---

> 📖 **Este documento é um guia de consulta, não substitui a documentação oficial.**
>
> Quando houver dúvida sobre o comportamento de um comando, consulte:
>
> ```bash
> git <comando> --help
> ```
>
> ou a documentação oficial do Git.
