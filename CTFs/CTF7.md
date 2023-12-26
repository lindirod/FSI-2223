
# CTF : Semana 7 

> **Objetivo:** Explorar vulnerabilidades do format string.

## Desafio 1

Inicialmente foi descarregado e extraído o ficheiro ZIP fornecido pela plataforma do CTF.
Seguindo as recomendações do enúnciado, começou-se por analisar com que proteções é que o programa foi compilado.

![](https://i.imgur.com/gwZIFPQ.png)


De seguida, foi analizado o código source do desafio para que fosse possível responder às três questões disponibilizadas pelo enunciado. 
No início do ficheiro main.c, era possível perceber que a flag seria lida do ficheiro flag.txt pela função load_flag e colocada numa variável global.

![](https://i.imgur.com/DA3zSYC.png)

Acabamos por concluir que o programa tinha uma vulnerabilidade de format string. É criado um array buffer onde será colocado um input do utilizador e de seguida, este será impresso com função printf. 

![](https://i.imgur.com/fZDMAqu.png)

Tendo isto em conta, com o objetivo de ler o que foi colocado na variável global flag, arranjámos um método para que esta fosse impressa com a chamada da função printf. 

Primeiramente, começamos por descobrir o endereço da variável flag através do gdb.

![](https://i.imgur.com/dEfNU9I.png)

De seguida, começamos a montar um fichero python para formar o input. Foi colocada no início da string o endereço da flag e posteriormente, foram feitos alguns testes para conseguir perceber aonde estava posicionado na stack o inicio da format string.

![](https://i.imgur.com/Q9OomOF.png)

Entretanto, percebermos que estava colocado logo na posição seguinte, então bastou juntar ao endereço da flag um "%s". Deste modo, o "%s" irá imprimir a string presente no endereço correspondente. 

![](https://i.imgur.com/L12JiQC.png)
![](https://i.imgur.com/C8SJn7G.png)

No fim, com o ficheiro de python criado para o input, conseguimos obter a flag do primeiro desfaio.

![](https://i.imgur.com/LiQFdEb.png)
![](https://i.imgur.com/eG0OyRj.png)


## Desafio 2


Para completar o segundo desafio, foi novamente descarregado e extraído um ficheiro ZIP fornecido pela plataforma do CTF.

Foi repetido o processo do primeiro desafio, e depois de uma análise, percebemos que o programa continha na mesma uma vulnerabilidade de format string. Também era criado um array buffer onde se guardava um input do utilizador e de seguida, este seria impresso utilizando a função printf.  Este novo programa tinha como única variável global, um inteiro denominado por key.

![](https://i.imgur.com/PRtY53m.png)

No fim da função main, o valor da key era verificado e caso fosse igual a um determinado valor, seria aberto uma shell.

![](https://i.imgur.com/e5VSQPv.png)

Concluímos assim, que teríamos que meter na variável key o valor 0xbeef para assim, abrir uma shell e de seguida, ler o ficheiro flag.txt. Primeiramente, começamos por descobrir o endereço da variável flag através do gdb.

![](https://i.imgur.com/Tnq3Nlx.png)

Seguidamente, foi novamente criado um ficheiro python. Tendo em conta que o objetivo seria colocar numa posição um determinado valor, sabíamos que a nossa string teria de ter no final um "%n". Mas para colocar 0xbeef na key, seria necessário duas coisas:
- O endereço que o "%n" consome ser o endereço da key; 
- Serem contabilizados 48879 impressos antes do %n para que seja colocado em key o valor 0xbeef.

Como solução, e tendo em conta que já sabiamos que a format string estava logo a seguir do input, foi colocada na string, o endereço de key duas vezes, uma para ser consumida pelo "%x" e outra para ser consumida pelo "%n". O "%x" foi juntado para que pudessem ser contabilizados os 48879 caractéres,ou seja, colocando a substring "%48871x". 
Já com a string completa, houve uns problemas com a abertura de shell, por isso modificamos o ficheiro python de modo a que, este realizasse a interação com o servidor e não fosse fechado a shell automaticamente.

![](https://i.imgur.com/aDxpDpX.png)

Ao correr o ficheiro python, foi aberta uma shell. Nesta foi possível obter a flag através do comando "cat flag.txt".

![](https://i.imgur.com/fRXRpqo.png)
