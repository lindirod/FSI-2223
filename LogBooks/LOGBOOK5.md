## Tarefa 1

A primeira tarefa tinha como objetivo inserir um shellcode malicioso no programa alvo, para que o código possa ser executado usando os privilégios do programa alvo. Para execução da tarefa e melhor compreensão do exercícios, fomos pesquisar o significado de comandos utilizados.

- `execve()` - executa o programa referido no “pathname”, fazendo com que o programa que está a ser corrido atualmente seja substituído por outro novo, com uma nova stack e heap inicializados e segmentos de dados (inicializados ou não).

- Sintaxe: `int execve(const char *pathname, char *const argv[], char *const envp[]);`

![](https://i.imgur.com/dMRBxje.png)

- Com o comando `env` conseguimos ver o que são as variáveis de ambiente (valor de nome dinâmico que pode afetar a maneira como os processos em execução se comportam num computador). 

![](https://i.imgur.com/cJp7Ztw.png)

- Com `pwd` é possível saber o nome do diretório local.

![](https://i.imgur.com/A5in9BV.png)

- O comando `ps` - process Status - permite visualizar uma lista dos processos em execução e os seus PIDs (Process Identification number), além de outras informações como TTY (terminal type), Time (o tempo total em que o programa tem estado a correr) e CMD (nome do comando que "lança" o processo)

![](https://i.imgur.com/qqmBLxk.png)

- `whoami` mostra o username do utilizador atual quando o comando é invocado.

![](https://i.imgur.com/EHtQXs0.png)

- O comando `logname` - retorna o nome do utilizador atualmente "logado".

![](https://i.imgur.com/d4qGygD.png)

- Por último, com o comando `id` -  mostra o ID do utilizador efetivo (UID) e o ID do grupo (GIDs) que pode conter a identidade de mais do que um utilizador.

![](https://i.imgur.com/yGdkJzc.png)


## Tarefa 2

Stack - Área na memória do computador que armazena variáveis temporárias criadas por uma função. Aqui as variáveis são declaradas, armazenadas e inicializadas durante o “runtime”.

set-UID -  "set user ID upon execution" - Permite aos utilizadores correr programas com privilégios temporários para poderem executar uma determinada tarefa.

![](https://i.imgur.com/3MaVPRp.png)

![](https://i.imgur.com/CsbU4Su.png)

Permissões:
- ‘-’ arquivo comum (tipo de objeto)
- ‘rws’ - permissão de leitura, escrita e SUID + permissão de execução para o dono (executar o programa como se fosse o dono) (dono) * 
- ‘r-x’ - (grupo)
- ‘r-x’ - (outros)


4 = 100 (binário) -> significa: SUID

## Tarefa 3 

#### Investigation:
![](https://i.imgur.com/ttAKbXP.png)

![](https://i.imgur.com/fpRynDm.png)

![](https://i.imgur.com/18H2E5I.png)

![](https://i.imgur.com/086Cgy6.png)

![](https://i.imgur.com/xqmyyy2.png)

![](https://i.imgur.com/UzGPYny.png)

Nesta útlima tarefa temos também de preencher o ficheiro exploit.py em 4 lugares distintos.

 1º - **Preencher o shellcode no exploit**. No ficheiro “call_shellcode”, estão disponibilizadas duas opções de shellcode para uma máquina de 64 bits ou 32 bits. Escolhemos fazer a tarefa com o último, tendo chegado à conclusão que não havia diferenças significativas entre as duas máquinas.

 2º - **Decidir onde a shellcode começa nos 517 bytes do buffer.** Para estar seguro coloca-se no final dos 517 bytes (extrai-se o tamanho da shellcode e o 1 para não haver problemas), visto que se ainda não estiver dentro da função `str` não pode ser antes de 100 bytes, mas também não pode estar perto da casa dos 100, tomando em consideração o que está descrito na nota 2 desta tarefa.

3º - **Endereço do ponto de retorno da função.** Sabendo que o ponto de retorno está guardado logo a seguir a `ebp` (base pointer for the current stack frame), temos que ir buscar este valor ao 5.1(Investigation) e somar o tamanho do `str` para se encontrar exatamente no endereço do ponto de retorno.
Para melhor entendimento da tarefa, foi feito o seguinte esquema: 

![](https://i.imgur.com/BBdz1Id.png)

4º - `offset` para encontrar o local exacto onde substituir o return address. Este local exacto é calculado através da seguinte fórmula: `&ebp - &buffer + 4`, com os endereços encontrados no 5.1(Investigation)

![](https://i.imgur.com/YZ5Uadc.png)




