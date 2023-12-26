
# CTF : Semana 5 - Pwn

> **Objetivo:** Explorar o funcionamento da stack e buffer-overflows.

## Desafio 1

Inicialmente foi descarregado e extraído um ficheiro ZIP fornecido pela plataforma do CTF.
Seguindo as recomendações do enunciado, começou-se por analisar com que proteções é que o programa foi compilado.
![](https://i.imgur.com/fDeFtdl.png)

De seguida, foi analizado o código source do desafio para que fosse possível responder às três questões disponibilizadas pelo enunciado. Foi concluído que o programa abria e lia um ficheiro, cujo nome seria guardado num array de chars de 8 bytes. O código guardava ainda num array de chars de 20 um input do utilizador.

![](https://i.imgur.com/UudL5Fq.png)


Tendo isto em conta, com o objetivo de ler o ficheiro flag.txt através do programa, arranjámos um método para controlar o ficheiro que é aberto. Sabendo que o buffer array consegue apenas guardar 20 bytes, os outros bytes submetidos no input irão ocupar o espaço de outras variáveis presentes na stack. Neste caso, ocupará o espaço do array onde está colocado o nome do ficheiro.

Por isso, para obter a flag colocamos como input 20 char e "flag.txt" no fim e assim mudar o ficheiro que iria ser lido.

![](https://i.imgur.com/DabB7AV.png)



## Desafio 2

Para completar o segundo desafio, foi novamente descarregado e extraído um ficheiro ZIP fornecido pela plataforma do CTF.

Foi repetido o processo do primeiro desafio, e depois de uma análise, percebemos que foi adicionado um array de 4 bytes. Esse array está preenchido por caracteres que irão ser posteriormente verificados.

![](https://i.imgur.com/FKVns9O.png)

Desta vez, por causa dos caracteres, apenas colocar um input não funcionaria. Tentamos colocar o mesmo input do primeiro desafio com a cópia dos prints dos caractéres no terminal e ainda num ficheiro, mas não resultou visto que em ambos os casos os caracteres eram corrompidos.

Por último, foi criado um código que dava print de todas as partes do input.

![](https://i.imgur.com/Ob0xenp.png)


No terminal, utilizamos o output desse programa para conseguirmos ler o ficheiro flag.txt e chegar à flag.

![](https://i.imgur.com/ItLDyLo.png)




