# Logbook 6

## Tarefa 1 

Inicialmente foi criado um ficheiro com o auxílio do sricpt *build_string.py*. Este contêm uma string que será colocada no buffer. Apoś a análise do script, foi percebido que seria colocado no final da string um %n.
![](https://i.imgur.com/EQEp2ul.png)

A finalidade deste símbolo resume-se em colocar num endereço que está contido na stack o número de caracteres escritos até então pela função *printf*. 
Tendo isso em conta, quando o printf tenta escrever a string, vai dar também overwrite à variável de retorno e serão lá colocados o número de caracteres lidos. 
Sendo assim, a função consegue retornar, o que faz o programa *crashar*. 
Apesar deste acontecimento, o servidor não irá também *crashar*.
![](https://i.imgur.com/BVw0zb5.png)

## Tarefa 2 

### A 

Para descobrir quantos %x eram necessários para imprimir os 4 primeiros bytes do input foi utilizado uma abordagem de tentativa erro. 
Primeiramente, foi criado um programa para montar uma string cujos 4 primeiros bytes seriam 0xAABBCCDD e o resto %x's. 

![](https://i.imgur.com/dzGnpi4.png)

Aumentando o valor à qual a string "%x" era multiplicada, conseguimos por fim, chegar à posição dos 4 primeiros bytes do input. No final, foram adicionados 64 %x's à string.
![](https://i.imgur.com/HfwuDci.png)


### B 

O objetivo agora é imprimir a mensagem secreta que se encontra guardada na heap. 
Tendo o endereço da mensagem, este foi colocado na string de input no lugar de 0xAABBCCDD. 
![](https://i.imgur.com/UqtkXoB.png)

Como já sabemos que o endereço estará na stack 64 posições acima, diminuimos uma unidade ao número de %x's colocados na string e adicionamos um %s. Este símbolo no final irá imprimir a string colocada no endereço que está na posição 64. 

![](https://i.imgur.com/POnNDhq.png)

![](https://i.imgur.com/MpQljbV.png)

## Tarefa 3 

### A 

Para agora mudar o valor guardado no endereço "target", repetimos o procedimento anteriormente feito. Colocamos nos primeiros 4 bytes o endereço do "target". 
![](https://i.imgur.com/J3AR6DV.png)

Sabemos que estará na posição 64, por isso, diminuimos uma unidade ao número de %x's colocados na string, mas em vez de adicionarmos %s, usamos %n. Com o uso do *formater* %n no *printf*, será colocado no endereço que se encontra presente nessa posição o número de caracteres impressos até ao %n.

![](https://i.imgur.com/61pjVu1.png)

![](https://i.imgur.com/oo8Z5uV.png)

### B 

Depois de conseguirmos colocar um valor no endereço do "target", o novo objetivo tornou-se em mudar o valor para 0x5000.
Na teoria, temos de imprimir 0x5000 caracteres. Sendo 0x5000 = 20480, terão de ser impressos 20480 antes da posição 64 de memória. 
Os primeiros 4 vem dos 4 bytes do target e d, logo só necessitamos de adicionar 20476. Por adicionar à string no final " %n", diminuimos o número para 20474. Sendo que iremos juntar à string 63 %x, multiplicamos um %x por 62 e ao último colocamos o valor de caractéres que nos faltam  (20474-63=20411). 

![](https://i.imgur.com/ad1vOKK.png)


![](https://i.imgur.com/aUqTTKT.png)
