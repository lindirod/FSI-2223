# Logbook 4

## Tarefa 1: Manipulação de variáveis de ambiente
Na primeira tarefa, é pedido que se utilize comandos como `printenv` e `env` para se poder visualizar uma lista das variáveis de ambiente (variáveis que podem alterar o comportamento dos programas).
Ao executar os dois comandos foi possível concluir que se não for apresentado nenhum argumento à frente, estes apresentam o mesmo resultado.

É também pedido que executemos os comandos `export` e `unset`, para a definição e “desmontagem” das variáveis de ambiente. Em seguida, apresentamos uma breve descrição do que cada comando faz, seguido de exemplos que executamos para melhor compreensão da tarefa.

- `export`: define uma variável de ambiente. Nomeia-se a variável para depois se conseguir ter acesso a esta através do nome na shell.


Sintaxe: `export NAME=VALUE`


![](https://i.imgur.com/q46O6wE.png)

- `unset`: remove a existência da variável anteriormente definida. Substituir a variável de ambiente com uma string vazia não a remove e pode causar problemas no sistema.


Sintaxe: `unset VARIABLE_NAME`


![](https://i.imgur.com/ZiFd7Yi.png)


## Tarefa 2: Passar variáveis de ambiente do processo-pai para o processo-filho
Nesta tarefa é sugerida a exploração acerca de como é que um processo-filho recebe as variáveis de ambiente do pai. 
Para melhor entendimento de como se efetuava este processo, executamos o comando man environ onde se pode concluir que quando um processo-filho é criado via comando fork, herda uma cópia do ambiente do pai.
De seguida, procedeu-se à execução dos dois passos, onde foi possível verificar que o resultado era o mesmo.

![](https://i.imgur.com/B97trJ1.png)



## Tarefa 3: Variáveis de ambiente e `execve()`

Segundo a definição, a função `execve()` executa o programa referido no *pathname* (`/usr/bin/env`) e tem a seguinte síntaxe:

`int execve(const char *filename, char *const argv[], char *const envp[])`

O 3º argumento, `envp`, é um array de strings passado como ambiente para o novo programa, como neste passo esse argumento está a NULL, não existem variáveis de ambiente para ser imprimidas.

De seguida, após a alteração do 3º argumento, já é possível verificar o output esperado visto que `environ` contém as variáveis de ambiente do processo. 


## Tarefa 4: Variáveis de ambiente e `system()`

A função `system` aceita como argumento um comando da shell, e depois da sua execução (através de chamadas a fork e a `execl`), devolve o valor retornado pelo comando executado. 
Já a função `execve` faz com que o programa que está a ser atualmente executado seja substituído por um programa novo, com uma stack inicializada, heap e segmentos de dados (inicializados ou não).
O programa tem como output as variáveis de ambiente, o que significa que a chamada do `system` “passa” o array das variáveis de ambiente para a chamada do `execve`.


## Tarefa 5: Variáveis de ambiente e programas Set-UID
Depois de compilado o programa, procedeu-se à execução dos comandos seguintes:

`sudo chown root foo`  e  `sudo chmod 4755 foo`

A execução do primeiro comando indica que o dono passa a ser o root em vez de seed como era anteriormente. No segundo comando, os últimos 3 dígitos dão permissão ao dono (root) para ler, escrever e executar e permissão ao grupo e ao utilizador para ler e executar. O número 4 define a flag set-UID.  
É também possível verificar que todas as variáveis na shell são imprimidas no processo filho.
- set-UID: Permite que os utilizadores corram programas com privilégios temporários para poderem executar uma determinada tarefa.

![](https://i.imgur.com/ZmPZ9Py.png)


## Tarefa 6: A variável ambiente `PATH` e programas Set-UID
Nesta última tarefa é pedido que tentemos fazer com que o programa Set-UID corra o nosso código malicioso em vez de `/bin/ls`.

Começamos por mudar o dono para root e passar o programa apresentado para um programa Set-UID como é pedido, através dos comandos da tarefa anterior. Em seguida, executamos o comando apresentado no enunciado para manipular a variável ambiente `PATH` &rarr; `export PATH=$PWD:$PATH`.
Criamos também um programa "malicioso" cuja única função era imprimir "Malicious code" e no final conseguimos correr o malicious code em vez do original, task6.c.


![](https://i.imgur.com/mvbYayj.png)
