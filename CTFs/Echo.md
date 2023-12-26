# Echo 
Neste ctf não conseguimos perceber do que exatamente se tratava através da frase: "Não te esqueças, não há nada totalmente seguro." Por isso, descarregamos os ficheiros fornecidos pela plataforma e como "tradição" do procedimento de vários outros ctf's, executamos o comando checksec para o program.Com a sua execução percebemos que estria tudo ativado. Além do programa, foi descarregada uma biblioteca de C.
![](https://i.imgur.com/VNcOiHq.png)

De seguida corremos o programa utilizandos diferentes inputs para testar o programa:
![](https://i.imgur.com/NvXDIpo.png)

![](https://i.imgur.com/5GD7SiO.png)

![](https://i.imgur.com/bhUe0oQ.png)


Depois de colocarmos um input maior do que tamanho máximo, ao tentar sair do programa através do input "q", recebemos uma mensagem. Conclímos então, que o programa continha a vulnerabilidade de buffer overflow, fazendo com que o excesso escrevesse por cima de dados na pilha. 

Para completar este challenge achamos pertinente desativar temporariamente a ASLR para impedir que os endereços fossem modificados e assim, tornar o nosso exploit mais difícil. Para tal, depois de pesquisarmos o comando na net, foi corrido:
![](https://i.imgur.com/tQ1sATO.png)


Sabendo que haveria uma biblioteca dentro do programa, foi desenvolvido um código para encontrar o endereço base da mesma e o offset entre esse endereço e o endereço da função system(), uma função que achamos que poderiamos utilizar para abrir uma shell e conseguir a flag. Através do comando info proc map conseguimos encontrar o intervalo no qual a biblioteca C está mapeada.

![](https://i.imgur.com/G6P0dB8.png)
![](https://i.imgur.com/EF48wB0.png)
![](https://i.imgur.com/ajMGgXJ.png)
![](https://i.imgur.com/pcb48qn.png)
![](https://i.imgur.com/OsRVkuU.png)
![](https://i.imgur.com/rYUvXft.png)

Agora temos que lidar com outro problema, o canário. Uma peculiaridade do canário, é que acaba em 00, por isso, corremos o script várias vezes até estabelcer um padrão, para depois poder dar bypass a esta medida de segurança.

De seguida tentamos encontrar os endereços da função system() na biblioteca C e da string "/bin/sh". Após descobrir os comandos na net:
![](https://i.imgur.com/xdZxAVK.png)
![](https://i.imgur.com/S35xmpU.png)

Por fim, foi escrito um ficheiro em python que aproveitaria vulnerabilidade encontrada e usaria a função system() com a string "/bin/sh" como parâmetro. Era necessário também dar reset ao valor do canário para evitar que o programa desse erro e substituir o endereço de retorno pelo endereço da função system(). Posteriormente, usamos a função system() para executar código.

![](https://i.imgur.com/0hSQfAr.png)
![](https://i.imgur.com/MBKAPhG.png)


Ao executar o ficheiro de python, a flag foi encontrada.
![](https://i.imgur.com/4a4soZF.png)


