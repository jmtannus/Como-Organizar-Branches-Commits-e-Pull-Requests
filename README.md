---

marp: true
theme: gaia
_class: lead
paginate: true
backgroundColor: #fff
backgroundImage: url(https://marp.app/assets/hero-background.svg)
title: Como-Organizar-Branches-Commits-e-Pull-Requests
author: Dicas de guscsales no TabNews (https://www.tabnews.com.br/guscsales/uma-maneira-de-organizar-suas-branches-commits-e-pull-requests)

---

# Uma Boa Maneira de Organizar Suas Branches, Commits e Pull Requests
Eu já trabalhei em diversos projetos com Git, seja hospedando em GitHub, Bitbucket, GitLab ou qualquer outro provedor. Durante essa estrada de programação percebi que muita das vezes as organizações de projetos, commits, nomes de branches e pull requests são negligenciadas.

O problema maior é: quando chega o bug desastroso (e ele vai chegar), onde precisamos entender o histórico para ter uma ideia melhor do que está acontecendo, e também o que precisamos arrumar, a falta de organização em commits, pull requests e afins afeta bastante nosso trabalho.

Por conta de tudo o que vivi eu decidi desenvolver um esquema de documentar o trabalho através desses recursos do git que tem funcionado bem para mim. Nesses padrões fica evidente a tarefa em que estive trabalhando, qual o tipo dela e o que aquele bloco de modificação de código vai afetar no projeto.

Quero muito compartilhar com vocês, então seguem alguns passos:

---

## Passo 1 - Nome da Branch
Normalmente usamos algum provedor de gerenciamento de tarefas, seja JIRA, Trello, Asana ou qualquer outro. O ponto é: provavelmente você vai ter algum tipo de identificador que esteja atrelado à aquela tarefa, no caso do JIRA ele sempre cria um sufixo seguido de um número. Um ponto legal é que existem integrações entre o GitHub e o JIRA por exemplo (e também entre outras aplicações) então fica tudo conectado quando se cria uma branch num estilo mais ou menos assim:

    # Padrão:
    <id-da-sua-tarefa>/<super-resumo-da-feature>
    
    # Exemplo:
    git checkout -b TL-100/create-post-api
      
Isso é um exemplo de quando seu GitHub está integrado com o JIRA e usando esse padrão acima:
 ![Conexão JIRA e GitHub](https://gsales.io/images/blog/uma-maneira-de-organizar-suas-branches-commits-e-pull-requests/conexao-github-jira.png)

---

## Passo 2 - Utilizar Padrões de Commit
Já não é de hoje que a convenção de commits do Angular é extremamente popular. E sim, ela ajuda demais à organizar os nossos commits, simplesmente porque conseguimos dividir em algo como: "tipo(escopo): descriçao". Nessa ideia temos os mais famosos tipos de commits que são os seguintes:

- **feat**: Um novo recurso para a aplicação, e não precisa ser algo grande, mas apenas algo que não existia antes e que a pessoa final irá acessar.
- **fix**: Correções de bugs
- **docs**: Alterações em arquivos relacionados à documentações
- **style**: Alterações de estilização, formatação etc
- **refactor**: Um codigo de refatoração, ou seja, que foi alterado, que tem uma mudança transparente para o usuário final, porém uma mudança real para a aplicação
- **perf**: Alterações relacionadas à performance
- **test**: Criação ou modificação de testes
- **chore**: Alterações em arquivos de configuração, build, distribuição, CI, ou qualquer outra coisa que não envolva diretamente o código da aplicação para o usuário final
  
      # Exemplo
      feat(posts): creating hook to integrate with posts API
      test: add missing tests for posts hook
  
*Vale lembrar que o "escopo" é opcional.*

---

## Passo 3 - Padrão de Título na Pull Request
Depois que você já subiu sua branch com tudo feito, uma das partes cruciais para se ter uma boa documentação são as pull requests. Quando você executa um "Squash and Merge" dentro do GitHub por exemplo, é o título da sua pull request que fica como commit principal e dentro da mensagem do commit ficam os outros commits. Então um padrão bem interessante é seguir a mesma ideia da convenção, com alguns upgrades:

     # Padrão:
     [<id-da-sua-tarefa>] tipo(escopo): descriçao
     
     # Exemplo:
     [TL-100] feat(posts): creating hook to integrate with posts API

Com esse título já da para ter uma ideia do que rolou por cima e também acesso fácil ao identificador da tarefa, onde possívelmente terá mais detalhes sobre aquele recurso.

---

## Passo 4 - Fazer Uma Boa Descrição na Pull Request
Eu sei que escrever a parte técnica pode ser muito chato as vezes, mas é parte do trabalho do dia a dia de um dev. Nem sempre faremos só coisas legais. Juntando o título no qual já tem o link para a estória, onde ficam as descrições de regras de negócio, adicionado um breve resumo do escopo no título e mais os detalhes técnicos na descrição seguido de um screenshot da tela (se possível) é um prato cheio para conseguir mitigar problemas e resolve-los rapidamente sem ficar se perdendo no meio do caminho.

Esse é o padrão que adaptei de um outro já existente e tem funcionado bem no meu dia a dia:

     ## Type of change
     
     - [ ] Bug fix (non-breaking change which fixes an issue)
     - [ ] New feature (non-breaking change which adds functionality)
     - [ ] Chore (documentation, packages, or tests updates, nothing that affect the final user directly)
     - [ ] Release (new version of the application - only for production)
     
     ## Description
     
     ...
     
     ## Screenshots
     
     ...
     
     ## Tasks
     
     - [task-id](task-link) or N/A
     
     ## Checklist
     
     - [ ] My changes have less than or equal 400 lines
     - [ ] I have performed a self-review of my own code
     - [ ] The existing tests and linter pass locally with my changes
     - [ ] I have commented my code in hard-to-understand areas (if applicable)
     - [ ] I have created tests for my fix or feature (if applicable)
     
     ## Dependencies
     
     This pull request has a dependency on the following others:
     
     - link-to-depency PR or N/A
     
Explicando:

- Item 1: Tipo da alteração, se é bug fix, feature, chore ou uma release (para o caso de fazer releases com alguma branch que não é a "main")
- Item 2: Descrição com mais detalhes, principalmente se esse recurso altera pontos fundamentais do sistema. Pontos esses que nós precisaremos lembrar no futuro, uma vez que não podemos confiar em nossas memórias
- Item 3: Se for possível e fizer sentido, capturas de telas que explicam melhor o recurso
- Item 4: Os links para as tarefas na aplicação que gerencia as estórias e tarefas
- Item 5: Checklist básico para subir uma PR:
   - Menos de 400 linhas
   - Revisão no próprio código antes de abrir PR
   - Todos os testes existentes passaram
   - Comentários em lugares necessários foram escritos
   - Criação de testes para o novo recurso

***Eu sei, nem sempre vai dar pra seguir certinho, mas é bom ter um alvo né?***

- Item 6: Outras pull requests que são dependentes, por exemplo: alguma pull request com uma API do backend que é necessária estar entregue antes de subir uma tela no frontend.
Lembrando que para adicionar padrões de template na pull request você pode utilizar essa forma: criar uma pasta .github na raiz do seu projeto e dentro um arquivo chamado pull_request_template.md com o template acima. Automaticamente o GitHub vai entender isso e abrir as pull requests com esse template.

## Dicas Bônus - Assignees, Labels e Multiplos Commits
Em uma pull request procure preencher outros dois campos (exemplos usando o GitHub):

 - "Assignees": as pessoas que trabalharam naquele recurso, porque em caso de problemas, é fácil achar quem tem mais contexto no time.
 - "Labels": uma ou mais labels que fazem sentido para aquele recurso (eu gosto de bastante informação, pra mim quanto mais melhor)
   
Por último crie multiplos commits para suas alterações, procure não alterar uma feature inteira e jogar um número enorme de arquivos (salvo exceções) em um único commit, assim fica mais fácil também de entender o que rolou no processo de desenvolvimento daquele recurso.

Obrigado por lerem e obrigado pela oportunidade de contribuir com a comunidade por aqui. Deixe seu feedback, adoraria ler!

Autor: Gustavo Sales <!--(https://www.tabnews.com.br/guscsales/uma-maneira-de-organizar-suas-branches-commits-e-pull-requests#:~:text=Autor%3A-,Gustavo%20Sales,-Fonte%3A%20https)-->

 Fonte: https://www.youtube.com/watch?v=oVnenyWTndY
