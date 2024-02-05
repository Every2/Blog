---
title: Estudando sobre Git
date: 2024-02-05
---

# O que é Git?

Git é um sistema de controle de versões, é principalmente usado em programação, mas pode ser usado para registrar qualquer tipo de histórico não relacionado a software.

### Sistema de controle de versões

Um sistema de controle de versões é um software que tem como finalidade gerenciar diferentes versões de um software e além disso também podem ser usados para armazenar o histórico de desenvolvimento de um software. Entre eles o mais famoso é o Git, mas existem outras alternativas como:
- `CVS`
- `Mercurial`
- `SVN`

### Destaques de uso para VCS (Version control system)

- `Controle de histórico`: Facilidade em resgastar versões antigas e estáveis. Analise em detalhes do histórico.
- `Trabalho em equipe`: Um vcs permite que varias pessoas trabalhem em um projeto ao mesmo tempo e minimiza o desgate entre conflitos de versões. (No git diferentes branchs separado por features, por exemplo)
- `Ramificações`: Possibilita a divisão em varias linhas de desenvolvimento paralelamente sem que uma interfira na outra.
- `Rastreabilidade`: Com a necessidade de sabermos o local, o estado e a qualidade de um arquivo; o controle de versão traz todos esses requisitos de forma que o usuário possa se embasar do arquivo que deseja utilizar.
- `Confiabilidade` : O uso de repositórios remotos (na nuvem) ajuda a não perder arquivos por eventos inesperados. Além disso, e possível fazer novos projetos sem danificar o desenvolvimento do atual.

### Como funciona?

O Git possui duas estruturas de dados: Um indice mutável que provê informações sobre o diretório de trabalho e a proxima revisão a ser cometida, e um banco de dados de objetos de acréscimo imutável.

O banco de dados de objetos contém quatro tipos de objetos:

- Um objeto `blob` é o conteúdo de um arquivo. Estes objetos não possuem nomes, datações ou outros metadados.
- Um objeto `Tree` é o equivalente ao diretório de um arquivo. Ele contém uma lista de nomes de arquivos, cada um com bits que informam o tipo e o nome do blob, da árvore, ligação simbólica ou o conteúdo de um diretório que pertence a esse nome. Esse objeto descreve o estado da árvore de diretório.
- Um objeto `commit` liga árvores de objetos junto com um histórico. Ele contém o nome de uma árvore de objeto(da raiz dos diretórios), datação, uma mensagem de log, e os nomes de zero ou mais objetos pai de commit.
- Um objeto `tag` é uma camada que referencia outros objetos e pode conter metadados adicionais relacionados a outro objeto. Em geral, é usado para armazenar uma `assinatura digital` de um  objeto commit correspondente áquela release de dados que estão sendo rastreados pelo git.

O indice serve como um ponto de conexão entre o banco de dados de objetos e a árvore de trabalho.

Cada objeto identificado por um hash `SHA-1` de seu conteúdo. O git computa o hash e usa esse valor  como nome para o objeto. O objeto é colocado em um diretório que corresponde aos primeiros dois caracteres de hash. O resto do hash é usado como um nome de arquivo para cada objeto.

O Git armazena cada revisão do arquivo com um único objeto blob. Os relacionamentos entre os blobs podem ser encontrados por examinar à árvore de objetos commit. Objetos recém adicionados são armazenados internamente usando compressão do `zlib`. Isto pode consumir uma grande quantidade de espaço de disco rapidamente. Desta forma, os objetos são combinados em pacotes, que são comprimidos para salvar espaço, gravando blobs como mudanças relativas a outros blobs. 

Servidores Git tipicamente escutam na `porta TCP/IP 9418`. Apesar disso o git pode rodar localmente na sua maquina sem necessidade de rede.

O Git tem três estados principais:
- `Commited`: Signfica que os dados foram armazenados em seu banco local.
- `Modified`: Significa que você alterou algum arquivo, mas ainda não fez o commit no seu banco de dados.
- `Staged`: Significa que você marcou a versão atual de um arquivo modificado para fazer parte do seu proximo commit. 

### Como achar um blob?

```
Vá até a pasta do seu repositório e digite:

git log

Após a isso pegue uma das hashs e rode

git cat-file -p <hash>

Deve aparecer algo assim:

38d32d3f914134dcb73
tree 7a5f4a6185ad3a47d4ebb50f2da1d12de86ebfbc
parent d4770306f5ebb6c0ab0be3d3a86b4cb362adca40
author seu-nick <seu-email> 1706316577 -0300
committer seu-nick <seu-email> 1706316577 -0300
```

### O que git log faz?
O `git log` mostra o histórico de commits de um repositório. Ele vai listar detalhes sobre commits anteriores, exemplo: `data e hora`, `autor`, `mensagem do commit` e o `hash`.

### O que é SHA-1?

SHA-1 (`Secure Hash Algorithm 1`) é um algoritmo de função hash criptográfica que produz um valor hash de 160 bits (20 bytes) para uma entrada de dados. Esse algoritmo foi desenvolvido pela NSA (Agência de Segurança Nacional dos Estados Unidos). Hoje em dia não é considerado tão seguro e utilizam mais o `SHA-256`.

### O que são commits?

Os commits são as `unidades estruturais` de um cronograma de projeto Git. Podem ser considerados instantâneos ou marcos ao longo do cronograma de um projeto Git. São criados com o comando `git commit` para capturar o estado de um projeto naquele momento. Os commits do Git sempre são feitos no `repositório local` em primeira  instância.

![Commits](/static/img/commits.png)

### O que é uma branch?

Uma branch no git é um ponteiro móvel para os commits que você faz. Você recebe um ponteiro que aponta para o seu último commit e cada vez que você faz um novo, ele avança para o proximo. Resumindo, seria como um mesmo repositório coexistindo no mesmo local, porém em posições diferentes e um não interferindo no outro.

O que acontece se você criar um novo branch? Bem, fazer isso cria um novo ponteiro para você mover. Digamos que você crie um novo branch chamado: testing. Você faz assim:

`git branch testing`

Isso cria um novo ponteiro para o mesmo commit em que você está atualmente.

![Branches](/static/img/Branches.png)

Duas branches apontando para a mesma série de commits. Como o Git sabe em qual branch você está atualmente? Ele mantém um ponteiro especial chamado `HEAD`.

O `HEAD` é um ponteiro para o branch local em que você está. Neste caso, você ainda está em master. O comando git branch apenas criou um novo branch,ele não mudou para aquele branch.

![HEAD](/static/img/HEAD.png)

Você pode checar para onde o ponteiro aponta com o comando `git log`:

```
Usando as flags --oneline e decorate, para facilitar a visualização

git log --oneline --decorate

f30ab (HEAD -> master, testing) add feature #32 - ability to add new formats to the central interface
34ac2 Fixed bug #1328 - stack overflow under certain conditions
98ca9 The initial commit of my project
```
Você pode trocar de branch com o comando `git checkout testing` e o ponteiro `HEAD` ficará assim.

![HEAD MOVIDO](/static/img/HEADMOVIDO.png)

E agora quando você fizer qualquer commit dentro de testing, o ponteiro se moverá para o proximo commit dentro de testing, mas dentro de master permanecerá no mesmo lugar.

![Commit testing](/static/img/COMMITTESTING.png)

### O que é merge?

O `git merge` é utilizado para unificar duas branches diferentes, ambas em seus commits mais recentes. Basicamente, ele vai procurar o ponto de encontro entre ambas as branches e juntar. Se o git encontrar algum ponto de divergência entre as duas branches ele não vai conseguir executar o merge e nesse cenário o usuário precisa intervir.

Caso não, você pode checar se o ponteiro `HEAD` está apontando para a branch certa, se não, trocar. Nesse caso,  vamos voltar para a branch main usando `git checkout main`. 

Agora para atualizarmos o histórico das alterações  recentes na branch que vai sofrer o merge digitamos `git fetch`. Assim que concluido esse passo executamos `git pull` para darmos merge nos commits.

### Como resolver conflitos no merge

Caso você tenha conflitos entre o merge, uma mensagem será exibida:

`error: Entry '<fileName>' would be overwritten by merge. Cannot merge. (Changes in staging area)`

O git é bem inteligente e descritivo para ajudar a encontrar os conflitos. Usando o comando `git status` recebemos uma mensagem descritiva sobre o que está gerando o conflito, exemplo: 
```
git status
On branch main
You have unmerged paths.
(fix conflicts and run "git commit")
(use "git merge --abort" to abort the merge)

Unmerged paths:
(use "git add <file>..." to mark resolution)

both modified:   merge.txt
```
Isso irá gerar uma saida no arquivo e você pode abrir um editor de texto para editar, por exemplo:
```
If you have questions, please
<<<<<<< HEAD
open an issue
=======
ask your question in IRC.
>>>>>>> branch-a
```
Então escolha as alterações que quer fazer no arquivo, remova os marcadores de conflito 
`<<<` ou `>>>>` ou `====` e rode os comandos: 

```
git add .

// O comentário pode ser diferente.
git commit -m "Resolve merge conflict by incorporating both suggestions"
```

Outras alternativas úteis podem ser os comandos:
- `git log --merge`: Usando a flag --merge o `git log` vai mostrar os conflitos entre as branchs.
- `git diff`: o `diff` ajuda a encontrar as diferenças de estado.
- `git checkout`: Pode ser usado para desfazer mudanças de arquivo.
- `git reset --mixed`: Pode ser usado para desfazer mudanças.
- `git reset`: Pode mudar o estado de conflito entre estados de arquivos para estado valido.
- `git merge --abort`: A flag `--abort` encerra o processo de merge entre branchs e volta ao estado anterior ao merge.

### O que é rebase?

Rebase é a combinação de uma sequência de commits pra um novo commit base. A ideia do rebase é fazer parecer que você criou uma nova branch a partir de um commit diferente.

Fazer rebase é muito útil para manter o histórico de commits linear. Por exemplo: Aconteceram avanços na branch `main` e você precisa fazer merge, mas a base da `main` que você usou é de commits anteriores. O benefico disso é evitar quebras na hora do merge e deixar o histórico "limpo" parecendo que as alterações ocorreram numa mesma linha do tempo, apesar de acontecer em tempos diferentes.

Você pode fazer o rebase de duas formas:
- `Dar merge direto`: Essa opção resulta num merge de 3 vias e um commit de merge.
- `Fazer o rebase e depois o merge`: O segundo resulta em um merge de avanço rápido e um histórico linear.

### Não altere commits publicos com o rebase!!

Nesse caso use `git rebase` com a flag `-i` para fazer um rebase interativo. Dessa forma, você poderá alterar commits individuais no processo ao em vez de só mover todos os commits e apagar o histórico, assim tirando a linearidade do histórico.

### Rebase padrão e Rebase interativo

O rebase padrão seria apenas rodar o comando `git rebase <base>`, se você o fizer, ele irá alterar os dados do commit até o ponto que você está.

Quando você executa `git rebase --interactive <base>` você pode alterar o histórico de commits removendo ou dividindo os commits existentes.

O modo interativo tem alguns comandos como:
- `p, pick = usar commit`
- `r, reword = usar commit, mas altere sua mensagem`
- `e, edit = Usar o commit, mas irá parar nele. Você pode continuar e fazer novas ações e quando terminar, o rebase vai continuar desse ponto`
- `s, squash = juntar o commit com o anterior`,
- `f, fixup = A mesma coisa do squash, mas irá descartar a mensagem desse commit`
- `x, exec = Permite rodar comandos do shell`
- `d, drop = Remove o commit`

O rebase tem alguns comandos adicionais:

- `git rebase --d` significa que durante a reprodução o commit vai ser descartado do bloco de commit combinado final.
- `git rebase --p` deixa o commit como está. Ele não vai modificar a mensagem ou conteúdo do commit e ainda vai ser um commit individual no histórico.
-  `git rebase --x` =  durante a reprodução, executa um script do shell da linha de comandos em cada commit marcado. Um exemplo útil seria executar o conjunto de teste da base de código em commits específicos, o que poderia ajudar a identificar as regressões durante um rebase.

Também tem o comando `--onto <newbase> <oldbase>` digamos que temos branchA e branchB que foi criada usando branchA como  base, porém branchB não depende de nenhuma alteração da branchA e pode ir para a branch `main`. Então, branchA é a oldbase e a newbase é branchB aonde o ponteiro `HEAD` deve apontar. Rodando `git rebase --onto main branchA branchB` agora `HEAD` aponta para branchB.

Caso ocorra errado e a linha do tempo de commits seja alterada de forma errada, o comando `git reflog` pode ajudar a desfazer o rebase e recuperar os commits apagados.

### Cherry Pick

O `git cherry-pick` é um comando que permite o usuario selecionar commits especificos de outra branch. Exemplo: Imagine que existe um bug no sistema, um colega de trabalho resolveu o bug, mas ele não finalizou a task então ainda não pode subir o commit para revisão ou merge. Então com cherry pick, você pode ir lá e pegar o commit. 

Você pode rodar os seguintes comandos:
`git checkout <branch>` para mudar de  branch e você pode rodar `git log` para ver o históricos de commit e pegar o id do commit e voltando pra branch,  você roda `git cherry-pick <id>` e pronto, agora o commit estará na sua branch.

O cherry pick tem outros comandos também:

- `git cherry-pick A^..B` =  Pega commits num dado intervalo.
- `git cherry-pick A..B` =  Ignora o primeiro commit.

Obs: A tem que ser o mais velho dos commits.

Assim como merge e rebase ele pode gerar conflito, você pode usar:

- `git cherry-pick --continue` = Usar após resolver o conflito
- `git cherry-pick --abort` = Cancelar o cherry-pick

ALguns comandos extras:
- `-e` = Permite que a mensagem do commit seja editada
- `-x` = Adiciona uma mensagem avisando que foi feito um cherry pick de outro commit
- `-allow-empty`  = Por padrão, cherry pick não permite commits em branco, com esse parametro, ele sobrescreve esse comportamento
- `-alow-empty-message` = Se o commit não tiver um titulo igual no exemplo anterior, ele não é permitido, mas com esse parametro isso muda

### Como fazer download (clonar outro repositório git) hospedado online

Basta rodar `git clone <url>`

Se o repositório for clonado vazio apenas com o endereço  você deve adicionar o url com os comandos:

`git init` - Para iniciar o repositório git
`git remote add origin  <url>` - Para sincronizar com o repositório.

E aí rodar os comandos:
`git add .` - Para adicionar os arquivos
`git commit -m "mensagem"`  - Para  adicionar uma mensagem no commit.
`git push origin branch`  -  Para dar commit

### Como mudar a mensagem de um commit?
Para  mudar a mensagem do último commit use `git commit --amend` e ele irá abrir o editor para trocar a mensagem do commit.