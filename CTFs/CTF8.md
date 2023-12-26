# CTF : Semana 8/9

> **Objetivo:**  Explorar vulnerabilidades de injeção.

## Desafio 1

Inicialmente foi descarregado o código fonte utilizado pelo servidor para interagir com a base de dados.
Seguindo as recomendações do enúnciado, começou-se por explorar a aplicação. O servidor tem uma funcionalidade de login que utiliza uma base de dados sql para guardar os dados dos utilizadores.
Ao analisar o documento descarregado, foi identificada uma vulnerabilidade. A query utilizada para a confirmação das credenciais apoia-se nos inputs do utilizador. Estes parâmetros não são verificados, estando o sistema suscetível a inputs maliciosos que podem alterar a semântica do comando SQL construído dinamicamente.
![](https://i.imgur.com/jXcE0zg.png)

Relembrando a aula teórica Segurança Web (Parte 2), esta vulnerabilidade denomina-se de *SQL Injection*.
Foi apresentado um exemplo muito semelhante ao sistema deste ctf:

![](https://i.imgur.com/BdAW0O9.png)

Tendo isto em conta, com o objetivo de fazer login como utilizador admin, foram utilizadas as seguintes credenciais:
![](https://i.imgur.com/840Dsrg.png)

Assim, a flag foi apresentada na página web.
![](https://i.imgur.com/QUwRto0.png)


## Desafio 2


Para completar o segundo desafio, foram inicialmente descarregados dois ficheiros fornecidos pela plataforma do CTF.
Começou-se por  correr o checksec e analisar quais seriam as proteções do programa e que tipo de ataques seria possível fazer.

![](https://i.imgur.com/SYGpMs0.png)


Correndo o programa, verificamos que o endereço da variável buffer é printado e posteriormente através da função gets, é guardada na mesma, um input.
![](https://i.imgur.com/52IW6AQ.png)
![](https://i.imgur.com/RNhW0gM.png)

Perante estes acontecimentos, concluímos que a vulnerabilidade estaria na linha de código onde é lido o buffer. Esta permite que seja implementado o método do buffer overflow injetando código malicioso na stack.
Com objetivo de implementar esta estratégia, foi criado um ficheiro em python para formular a string que iria ser colocada no buffer.
 
![](https://i.imgur.com/htz3nYG.png)


Sendo a ideia colocar código malicioso na stack e posteriormente executá-lo, teremos que fazer duas coisas: 
- 1.- Colocar no início do buffer *shellcode* que permitirá abrir uma bash para depois puder encontrar a flag;
- 2.- Modificar o endereço de retorno do programa para o endereço do buffer para que o *shellcode* colocado seja executado.

Para a parte do *shellcode*, foi utilizado um código fornecido pelo logbook 5.
![](https://i.imgur.com/ZOdCjGj.png)

Tendo em conta que o buffer é a única variável do programa, os endereços seguintes serão o framepointer e o endereço de retorno.
Por isso, a string criada deve conter 100 bytes (correspondentes ao shellcode de 23 bytes e a qualquer outros 77 bytes aleatórios) e ainda as substituições do framepointer e do endereço de retorno.
Para modificar o framepointer, foram utilizados 8 bytes aleatórios.
Já o endereço de retorno será substituído pelo endereço do buffer, sendo este obtido através de funções do ficheiro em python. Com estas funções é possível adquirir *substrings* dos prints realizados durante a execução do programa, que neste caso, como já foi referido, o endereço do buffer. 
Ao correr o ficheiro python, foi aberta uma shell. Nesta foi possível obter a flag através do comando "cat flag.txt".

![](https://i.imgur.com/SKuMzFV.png)

