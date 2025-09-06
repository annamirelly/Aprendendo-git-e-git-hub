# Github Testes
Testando agora git no terminal

## Ciclo de vida / Status dos arquivos
**Untracked** -> O arquivo foi removido, ou foi adicionado sem ser reconhecido no git

**Unmodified** -> O arquivo reconhecido no git e não modificado, será Unmodified

**Modified** -> O arquivo reconhecido no git foi modificado

**Staged** -> O arquivo está selecionado para estar no commit e depois se tornar unmodified

## Adicionar Arquivo
Para que o git reconheça seu arquivo, digite

`git add <Nome do arquivo>`

## Commit
Para fazer um commit, digite

`git commit -m "<Mensagem do commit>"`

## Logs
Com seus primeiros commits, você pode ver as logs das commits digitando

`git log` 
`git log --decorate` (Aplica mais detalhes) 
`git log --author="<autor>"` (Filtra pro autores) 
`git shortlog` (Log em ordem alfabética de cada autor)
`git log --graph` (Gráfico das logs) entre outras variantes
## Monitorar commits
Você pode monitorar os commits usando suas hashs

`git show <hash do commit>`

## Diferenciar commits
Após suas mudanças no código, você pode diferenciar o último commit com o recém modificado digitando

`git diff`
`git diff --name-only` (para saber apenas os arquivos modificados)
## Desfazer alterações
Para qualquer imprevisto e reverter uma alteração de um arquivo, digite

`git checkout <nome do arquivo>`

Esse comando vai voltar o estado do seu arquivo para o último commit

Caso você tenha dado um git add, você pode reverter digitando

`git reset HEAD <nome do arquivo>`

Caso já tenha comittado, você também pode reverter digitando

`git reset --soft` -> Vai reverter o commit, mas com os arquivos em staged
`git reset --mixed` -> Vai reverter, voltando os arquivos antes do staged
`git reset --hard `-> Vai ignorar completamente o commit e destruir todas as alterações
## Branches
### 1. O que é um Branch?
Um **branch** (ramo, em português) é uma linha de desenvolvimento independente. É essencialmente um **ponteiro leve e móvel para um commit.**

Quando você inicia um projeto, o Git cria um branch padrão chamado `main` (ou `master` em repositórios mais antigos). Cada vez que você faz um commit, o branch `main `avança, apontando para o seu último trabalho.

Criar um novo branch significa criar um novo ponteiro, que começa apontando para o mesmo commit que o branch atual. A partir daí, os commits feitos nesse novo branch não afetarão o branch original, permitindo que eles "divirjam" e sigam

## 2. Como Funciona na Prática?
O Git não duplica todos os seus arquivos ao criar um branch. Ele é muito mais inteligente e eficiente.

Vamos ver o passo a passo:

**Estado Inicial**: Você tem um branch `main` com alguns commits. O `HEAD` é um ponteiro especial que indica em qual branch você está trabalhando no momento.

```
(HEAD)
  ↓
[main] → [Commit A] → [Commit B] → [Commit C]
```
**Criando um Novo Branch:** Você decide trabalhar em uma nova funcionalidade, como um sistema de login. Você cria um novo branch chamado feature/login.

Comando: git branch feature/login

Agora, você tem um novo ponteiro (feature/login) apontando para o mesmo commit que o main. O HEAD ainda está no main.

  ```
(HEAD)
    ↓
  [main] --------→ [Commit C]
                     ↑
[feature/login] ---┘
```
**Trocando para o Novo Branch:** Para começar a trabalhar na nova funcionalidade, você precisa "mudar" para o novo branch
Comando: `git checkout feature/login` (ou o mais moderno `git switch feature/login`)

O `HEAD` agora aponta para `feature/login`. Seu diretório de trabalho agora reflete o estado deste branch.
```
  [main] --------→ [Commit C]
                     ↑
[feature/login] ---┘
    ↑
  (HEAD)
```
**Fazendo Novos Commits:** Você trabalha na funcionalidade de login e faz um novo commit.

Comando: `git commit -m "Adiciona formulário de login"`

Apenas o branch `feature/login` avança. O branch `main` permanece intocado, seguro e estável.
```
  [main] → [Commit C]

                  ↘
                    [feature/login] → [Commit D]
                                        ↑
                                      (HEAD)
```
**Unindo o Trabalho (Merge):** Quando a funcionalidade de login estiver pronta e testada, você pode uni-la de volta ao branch main.

Primeiro, volte para o branch `main`: `git switch main` Depois, execute o merge: `git merge feature/login`

O Git criará um "merge commit" que combina as mudanças do `feature/login` com o `main`. Agora, o `main` contém todo o seu novo trabalho.
```
  [main] → [Commit C] -----------------→ [Commit E (Merge)]
      \                                    /
       ↘                                  /
        [feature/login] → [Commit D] ---┘
```
### 3. Por que Usar Branches? (Vantagens)
Usar branches não é apenas uma "boa prática", é a base para um fluxo de trabalho de desenvolvimento moderno e eficiente.

**1. Isolamento e Segurança:**

Você pode desenvolver novas funcionalidades, mesmo que grandes e complexas, sem qualquer risco de quebrar o código principal (main). A main deve sempre representar uma versão estável e, idealmente, "implantável" (deployable) do seu projeto.
**2. Experimentação Sem Medo:**

Tem uma ideia maluca ou quer testar uma nova biblioteca? Crie um branch! Se não der certo, você pode simplesmente deletá-lo (git branch -d nome-do-branch) sem deixar rastros no projeto principal.
**3. Trabalho em Equipe (Desenvolvimento Paralelo):**

Esta é a maior vantagem para equipes. Vários desenvolvedores podem trabalhar em funcionalidades diferentes ao mesmo tempo, cada um em seu próprio branch. Isso evita que um desenvolvedor precise esperar o outro terminar para poder começar seu trabalho.
**4. Organização e Clareza:**

O histórico do seu projeto fica muito mais limpo. Em vez de uma única linha de commits com mensagens como "comecei a funcionalidade X" e "corrigi bug na funcionalidade Y", você tem branches dedicados: feature/user-profile, hotfix/login-bug, release/v1.1.0. Fica fácil entender o que aconteceu e quando.
**5. Facilita o Code Review (Revisão de Código):**

Branches são a base para Pull Requests (ou Merge Requests) em plataformas como GitHub, GitLab e Bitbucket. O fluxo é:
Você cria um branch para sua tarefa.
Termina o trabalho e envia (push) seu branch para o repositório remoto.
Abre um Pull Request, que é um pedido formal para "unir meu branch ao branch main".
Outros desenvolvedores podem revisar seu código, deixar comentários e sugerir melhorias antes que ele seja integrado à base principal.
**6. Gerenciamento de Versões e Hotfixes:**

Imagine que você lançou a versão 1.0 do seu software (que está no branch main). Enquanto a equipe já trabalha em novas funcionalidades para a versão 2.0 em seus próprios branches, um bug crítico é descoberto na versão 1.0.
Sem branches, seria um pesadelo. Com branches, é simples:
1. Crie um branch chamado hotfix/bug-critico a partir do main.
2. Corrija o bug nesse branch.
Faça o merge do hotfix de volta para o main e lance a versão 1.0.1.
Tudo isso sem interferir no desenvolvimento da versão 2.0.
### 4. Exemplos de Comandos Essenciais
# Lista todos os branches locais
git branch

 Cria um novo branch chamado "nova-feature"
git branch nova-feature

 Muda para o branch "nova-feature"
git checkout nova-feature

 Atalho: cria e já muda para um novo branch
git checkout -b outra-feature

 Use `switch` (mais moderno e claro para navegar)
git switch minha-feature

 Atalho com switch: cria e já muda
git switch -c minha-outra-feature

 Depois de terminar o trabalho no branch, volte para o main
git switch main

 Une as mudanças de "outra-feature" no branch atual (main)
git merge outra-feature

 Deleta o branch localmente (após o merge)
git branch -d outra-feature

 Força a deleção de um branch (cuidado!)
git branch -D outra-feature
## Branches (Parte 2)
### 1. O que é o git rebase?
Se o `merge` é como "unir duas linhas do tempo em um único ponto", o rebase é como "viajar no tempo para reescrever a história".

**Rebase** significa, literalmente, **mudar a base** de um branch. Em vez de criar um "commit de merge" para juntar duas histórias, o rebase pega todos os commits de um branch e os reaplica, um por um, no topo de outro branch.

**Imagine a seguinte situação:**

Você criou um branch `feature/login` a partir do `main.`
Você fez dois commits (C3, C4) na sua `feature.`
Enquanto isso, outra pessoa fez um commit (C5) no main.
Seu histórico se parece com isto:
```
      C3 -- C4  (feature/login)
     /
C1 -- C2 -- C5  (main)
```
Ao fazer um `git rebase main` (estando no branch `feature/login`), o Git move seus commits (C3, C4) para que eles sejam aplicados *depois* do último trabalho da main (C5), criando uma história linear:

```

                  C3'-- C4' (feature/login)
                 /
C1 -- C2 -- C5 -- (main)
```
Seus commits originais são descartados e novos commits (C3', C4') com as mesmas mudanças são criados. A história do seu branch foi reescrita para parecer que seu trabalho começou depois do C5.

> [!Tip]

>Nunca faça rebase em um branch público/compartilhado que outras pessoas estejam usando (como o `main` ou `develop`)**. Fazer isso reescreve a história que outras pessoas já baixaram, causando um caos absoluto quando elas tentarem sincronizar suas mudanças. Use o rebase principalmente em seus branches locais, antes de compartilhá-los.

## 2. Diferença Fundamental: merge vs. rebase
| Característica |	`git merge` |	`git rebase` |
| :--- | :--- | :--- |
| **histórico** |**Preserva a história** exatamente como aconteceu. Cria um novo "merge commit" para unir as duas linhas do tempo.	**Reescreve a história** para ser linear. Não cria um merge commit. O histórico fica limpo e reto.
| **Commit Resultante** | Cria um **commit de merge** explícito (ex: "Merge branch `feature` into `main`").	Não cria um commit de merge. Apenas move os commits existentes para um novo ponto de partida. |
| **Resolução de Conflitos** | Você resolve todos os conflitos de uma vez, em um único momento, antes de finalizar o merge commit.	Você resolve os conflitos commit por commit, à medida que o rebase os reaplica.
| **Filosofia**	| "Quero um registro fiel de tudo que aconteceu, incluindo o desenvolvimento paralelo."	"Quero que a história do projeto seja o mais limpa e fácil de ler possível."
| **Segurança** |	Não-destrutivo. Adiciona novos commits, mas não altera os existentes. Mais seguro para iniciantes.	| Destrutivo (reescreve commits). Requer mais cuidado e compreensão da dica acima. |
### 3. Comparação na Prática: Fluxos de Trabalho
Vamos ver como cada um se comporta em um cenário real.

**Cenário:** Você está trabalhando no branch `feature/perfil.` Enquanto isso, a `main` recebeu novas atualizações. Você precisa atualizar seu branch antes de abrir um Pull Request.

### Fluxo de Trabalho com `git merge`
Você atualiza seu branch de feature trazendo as mudanças da `main` para ele.

```bash
git switch feature/perfil
git merge main
```
**Resultado no Histórico:** O histórico do seu branch feature/perfil agora terá um merge commit no meio.
```
      C3 -- C4 -- M  (feature/perfil)
     /           /
C1 -- C2 ------ C5  (main)
```
### Prós do Merge
✅ **Rastreabilidade e Fidelidade:** O histórico é 100% fiel ao que aconteceu. Fica claro que houve um trabalho paralelo e um momento exato de integração. É um registro histórico preciso.
✅ **Segurança e Simplicidade:** É uma operação não-destrutiva. Seus commits existentes permanecem intocados. Isso torna o merge mais seguro e fácil de entender, especialmente para equipes com membros menos experientes.
✅ **Contexto Preservado:** O grafo mostra o contexto de onde e quando cada branch foi criado e unido, o que pode ser útil para depuração de processos complexos.
Contras do Merge
❌ **Histórico Poluído:** Em projetos com muitos branches e desenvolvedores, o histórico do main pode se tornar uma teia de aranha de merge commits, tornando muito difícil seguir a linha de desenvolvimento principal.
❌ **Dificulta a Leitura:** Os commits de merge ("Merge branch 'x' into 'y'") adicionam ruído. Encontrar commits específicos que introduziram uma mudança pode se tornar mais complicado.
### Fluxo de Trabalho com git rebase
Você atualiza seu branch de feature reescrevendo sua base para o topo da `main.`

```bash
git switch feature/perfil
git rebase main
```
**Resultado no Histórico:** Sua `feature/perfil` agora parece ter sido criada a partir da `main` mais recente.

```
                  C3'-- C4' (feature/perfil)
                 /
C1 -- C2 ------ C5  (main)
```
#### prós do Rebase
✅ **Histórico Linear e Limpo:** Este é o principal benefício. O histórico do projeto se torna uma linha reta, fácil de ler do início ao fim. É como ler os capítulos de um livro em ordem.
✅ **Facilita a Revisão de Código (Code Review):** Ao abrir um Pull Request, todos os seus commits estarão agrupados e sequenciais no topo da main, sem merge commits intermediários para atrapalhar a revisão.
✅ **Commits mais Atômicos:** Facilita a gestão de commits. Com o rebase interativo (git rebase -i), você pode organizar, juntar (squash) ou reescrever seus commits antes de compartilhá-los, deixando seu trabalho mais coeso.
Contras do Rebase
❌ **Reescreve a História:** É uma operação destrutiva. Usá-la incorretamente em um branch compartilhado (como o main) é a receita para o desastre em equipe. A "dica do rebase" é fundamental.
❌ **Perde o Contexto:** Você perde a informação de quando o trabalho realmente começou em relação ao branch principal. Para auditorias ou depurações históricas, isso pode ser uma desvantagem.
❌ **Resolução de Conflitos mais Complexa:** Se vários dos seus commits entram em conflito com as mudanças da main, você terá que resolver conflitos repetidamente para cada commit que está sendo reaplicado, o que pode ser tedioso e propenso a erros.
### Conclusão: Qual e Quando Usar?
A melhor abordagem, adotada pela maioria das equipes profissionais, é **usar os dois**, aproveitando o melhor de cada um:

**Use `rebase` para o trabalho local:** Ao desenvolver em seu branch de feature (`feature/perfil`), use `git rebase main` periodicamente para manter seu branch atualizado com a `main`. Isso mantém seu trabalho organizado e o histórico da sua feature limpo e linear.

**Use `merge` para integrar na `main`:** Quando seu Pull Request for aprovado, use uma estratégia de **merge** (geralmente um `merge --no-ff` feito pela plataforma como GitHub/GitLab) para integrar a feature inteira na main.

**Resultado desse fluxo híbrido:**

Os **branches de feature** são limpos e fáceis de revisar (graças ao `rebase`).
O histórico da `main` permanece rastreável, mostrando claramente quando cada feature foi integrada através de um único e significativo merge commit (graças ao `merge`).
É a combinação perfeita de um histórico de projeto limpo e uma rastreabilidade robusta.
