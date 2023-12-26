# Number Station 3

No pequeno texto associado ao ctf foi nos informado que este desafio era "mais uma sample de uma number station". Ao contrário das outras, esta fornecia o programa que gerava o sinal. 
Após descarregar o ficheiro fornecido pela plataforma do CTF, fomos verificar as mensagens codificadas na porta 6002 do servidor ctf-fsi.fe.up.pt

![](https://i.imgur.com/CqHZGWm.png)


O ficheiro challenge.py incluia três funções: uma para geral o sinal, outra para codificar uma mensagem e por último uma para a descodificar. No programa, a flag era lida de um ficheiro, gerava-se um sinal e posteriormente codificava-se a mesma.

![](https://i.imgur.com/e4gxTJX.png)

Primeiramente, como não ia ser utilizado nenhum ficheiro para ler uma flag removemos essa parte. Apenas criámos uma variável flag, aonde colocámos a mensagem recebida pelo servidor do ctf no terminal.

Ao refletir sobre como iríamos descodificar a mensagem, chegamos à conclusão que apenas precisaríamos do k certo para o fazer. 
Sabendo que este sinal era gerado de forma aleatória através da função *gen()*, implementamos um ciclo que a cada iteração gerava um novo sinal e testava se a flag descodificada possuia como 4 primeiros bytes a string "flag".
Para descodificar a mensagem a cada iteração, utilizamos a função *dec()*, cujos parâmetros seriam o novo sinal gerado e flag traduzida de hexadécimal para binário através da função *unhexlify*. 
Caso o resultado desta comparação fosse verdadeiro, o ciclo terminaria e a flag descodificada seria printada no terminal. 

![](https://i.imgur.com/dZkhMpX.png)

Ao correr o ficheiro python no terminal, passado pouco tempo, a flag foi apresentada.

![](https://i.imgur.com/xOvHJjH.png)


