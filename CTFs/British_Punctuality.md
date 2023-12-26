


# British Punctuality
Não existindo enunciado deste desafio, visto que é um desfio opcional, tivemos de nos contentar apenas com uma pequena "dica" apresentada na plataforma. ("This challenge runs with a strict schedule")

Para dar algum sentido à frase, decidimos começar a explorar o servidor do ctf. Assim que é corrido o servidor no terminal, estavamos no diretório */home/flag_reader*, no qual existiam três diferentes ficheiros. 

Ficheiro *main.c* :
![](https://i.imgur.com/SqFssyj.png)

Ficheiro *myscript.sh* :
![](https://i.imgur.com/t5qMLch.png)

Ao visualizar o ficheiro *myscript.sh*, decidimos investigar o diretório */tmp/env*. Dentro do mesmo, encontramos um ficheiro chamado *last_log*:
![](https://i.imgur.com/5pJJtm4.png)

Fazendo uma análise ao seu conteúdo e tendo em conta os dois ficheiros observados no diretório */home/flag_reader*, pensamos na possibilidade de o servidor estar a correr periodicamente o script *myscript.sh* (como era indicado pela dica do ctf). 

Como exploit foram então criados dois ficheiros diferentes. 
Inicialmente criamos um ficheiro chamado *printenv* onde foi criada uma função para correr o comando *cat flag.txt* num terminal. 


![](https://i.imgur.com/yEowAmn.png)

![](https://i.imgur.com/t4knEI4.png)

De seguida, foi criado um ficheiro chamado env que teria como função mudar o caminho onde seri
am procuradas os códigos de execução das funções. Foi colocado, no início da variável PATH, o diretório tmp, onde foi criado o programa *printenv*.

![](https://i.imgur.com/z3kvSkm.png)
![](https://i.imgur.com/69IsxLG.png)

Sabendo que no script iria ser corrido o ficheiro *env* e posteriormente o *printenv* do diretório */tmp*, esperamos algum tempo para ver se a teoria de execução periodica do script era verdadeira.
Depois de voltarmos a entrar no servidor e lermos de novo o ficheiro last_log no diretório */tmp*, a flag foi encontrada.

![](https://i.imgur.com/frmP5tt.png)


