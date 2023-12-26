
# CTF : Semana 12/13

> **Objetivo:** Compreender utilizações inseguras de RSA

## Desafio 1
Inicialmente foi descarregado o ficheiro fornecido pela plataforma do CTF. 
Como era descrito no enunciado do ctf, na porta 6000 do servidor ctf-fsi.fe.up.pt encontrava-se um serviço onde estaria disponível uma flag cifrada através da função RSA.
![](https://i.imgur.com/8DxfUc5.png)

O ficheiro template.py incluia duas funções: uma para codificar e outra para descodificar uma mensagem. Estas funções utilizariam variáveis que eram referenciadas no início do código, mas que estavam ainda por definir.
Começamos então por tentar descobrir os valores de p e q. Tendo conhecimento de que p é um primo próximo de 2^512 e q é próximo de 2^513, inicialmente, desenvolvemos uma função com *list comprehension* para obter o primo seguinte de um determinado número. Porém para números tão grandes como 2^512, era necessário muito tempo para obter o resultado. 
Depois de alguma pesquisa descobrimos que havia uma função *nextprime* da classe *ntheory* do módulo *sympy*. Assim, descobrimos os valores de p, q e n.
Para o valor de d foram implementadas duas funções que retornavam o inverso modular de um número.

![](https://i.imgur.com/JCk2zfs.png)

Ao colocar na variável enc_flag a flag codificada e de seguida correr o ficheiro de python, a flag é apresentada no terminal.

![](https://i.imgur.com/LFF38Ji.png)

## Desafio 2
Como descrito no enunciado, ao correr o servidor encotravamos as duas mensagens cifradas com o mesmo n e e's diferentes:
![](https://i.imgur.com/PDpMAR6.png)

Depois de algumas pesquisa sobre possíveis ataques ao RSA, deparamos-nos com o tipo específico que se enquadrava na situação em que nos encontrávamos.
![](https://i.imgur.com/cOGIaw4.png)

Tendo esta informação, primeiramente, decidimos verificar se mdc(e1,e2) = 1:

![](https://i.imgur.com/MFyTV0N.png)

Depois numa calculadora online obtemos os dois valores:

![](https://i.imgur.com/NNC2qwc.png)

Contendo os dois valores formulamos um ficheiro em python que nos permitia decirar a mensagem em decimal com as formúlas que estavam presentes na nossa pesquisa.
NOTA: As mensagens foram passadas para decimal antes de serem colocadas no ficheiro.
![](https://i.imgur.com/su5Cgu9.png)

![](https://i.imgur.com/yYMWysR.png)

Correndo o ficheiro recebemos então a flag em decimal:

![](https://i.imgur.com/UZiJ7J3.png)


Por último, modificamos o ficheiro para traduzir a mensagem que recebemos no terminal. Assim que corremos o código a flag apareceu printada no terminal.

![](https://i.imgur.com/WiAIAS5.png)

![](https://i.imgur.com/G3iF4Ai.png)

