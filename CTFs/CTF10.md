
# CTF : Semana 10

## Desafio 1

Seguindo as indicações do enunciado, inicialmente exploramos a plataforma como um utilizador comum.Para ter noção do que aconteceria, fizemos um pedido de experiência e esperamos pela visualização do administrador.

![](https://i.imgur.com/RbsYTaB.png)

Ao receber a resposta do administrador, aperecem dois inputs desativados. Percebemos pelo nome, que para obter a flag teriamos de alguma forma ativar o input *Give the flag*. 

![](https://i.imgur.com/5pwwFgo.png)

Investigando a página, foi notado que o pedido o utilizador era colocado dentro de uma divisão no documento html da página. Concluímos então, que o site continha a vulnerabilidade XSS (Cross‑site Scripting).

![](https://i.imgur.com/fd6DjfW.png)

Sendo assim, pensamos que para ativar o input *Give the flag* poderíamos criar um script que seria introduzido na página e posteriormente analisado pelo administrador. De seguida, verificamos o id do input correspondente e introduzimosum script como pedido que executava um click no input desejado.

![](https://i.imgur.com/asQEdLB.png)

![](https://i.imgur.com/NB4uZhs.png)


Depois do pedido ser verificado pelo administrador, a flag foi apresentada na página web.

![](https://i.imgur.com/SvAPJFa.png)


## Desafio 2


Para completar o segundo desafio, começou-se por perceber as funcionalidades que eram acessiveis a um utilizador sem este estar autenticado.

![](https://i.imgur.com/65a4Kg0.png)

Ao clicar no *Here* sublinhado, fomos encaminhados para uma página onde é possível dar *ping* a um host.

![](https://i.imgur.com/smpfc3l.png)

Como teste colocamos o site *www.google.pt*. E a resposta foi a seguinte:

![](https://i.imgur.com/4SANuMn.png)

Após a utilização desta funcionalidade, tendo em conta o feedback, conseguimos assemelhar a mesma ao comando ping executado numa *shell* do sistema operativo linux. 
De modo a explicar o modo como esta funcionalidade poderia ser implementada, pensamos na possilidade da página web utilizar uma *bash* onde executava o comando "ping" + input do utilizador.
Metendo um teste em prática colocámos ";" para colocar o comando ls de seguida.

![](https://i.imgur.com/zHSPut3.png)

![](https://i.imgur.com/5O1z7Ob.png)


Assim, tivemos a confirmação de que o site teria uma vulnerabididade de colocar o input recebido pelo utilizador na shell sem verificação prévia do mesmo.
Portanto, sabendo que a flag se encontraria no ficheiro /flags/flag.txt, foi utilizado o comando "cat /flags/flag.txt". 

![](https://i.imgur.com/YxWhAIp.png)

Por fim, a flag apareceu como resultado dos comandos submetidos.

![](https://i.imgur.com/rr3vVGR.png)






