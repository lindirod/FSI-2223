# Final Format 

Inicialmente descarregamos o programa apresentado na plataforma do ctf. Como se travava de um ctf opcional, não havia enúnciado que nos guia-se, por isso, tentámos recolher toda a informação que nos era dada. 
Sendo o nome do desfio *Final Format* chegámos à conclusão que provavelmente o método string format iria ser-nos útil neste ctf. Acrescentando a isto, através de uma das frases associados ao *challenge* ("A flag encontra-se no working directory, no ficheiro flag.txt.") previmos que para obter a flag, numa fase final iria ser necessário usar o comando "cat flag.txt".
Passando para a análise do programa descarregado, esta teve de ser realizada dentro da virtual machine visto que através do wsl do windows o ficheiro não era reconhecido, apesar de estar a ser corrido no diretório certo. 
![](https://i.imgur.com/JENcsQg.png)

Começamos a análise inspirada no procedimento utilizado nos ctf's obrigatórios, ou seja, com o comando checksec. 
![](https://i.imgur.com/hwLrD7R.png)
O ficheiro apresentava uma canário, uma técnica de proteção que dificulta a realização de um ataque de buffer overflow.
Mas, em contrapartida, não existia proteção PIE (Executáveis Independentes de Posição), ou seja, não nos precisávamos de preocupar com possíveis trocas aleatórias de localização na memória virtual sempre que o programa fosse executado. 

Antes de analisar mais o programa decidimos correr o servidor no ubuntu para perceber como era o ser funcionamento.

![](https://i.imgur.com/DRWcZb3.png)

Para confirmar de que vulnerabilidade se tratava, algo que já tínhamos imaginado pelo título do desafio, decidimos colocar uma string aleatória com caractéres especiais.

![](https://i.imgur.com/3ro2DVI.png)

Depois disto, decidimos que necessitávamos de obter mais informação sobre o programa então pesquisamos alguns comandos no gdb que nos permitiam recolher mais dados sobre o mesmo.
No terminal, depois de ser executado o comando gdb program, foram executados os seguintes comandos:

- "info functions" - para investigar as funções que o ficheiro continha;

![](https://i.imgur.com/0YR6dT0.png)

- "disas main" - para investigar a função main

![](https://i.imgur.com/D08DlIA.png)

- "disas fflush" - para investigar a função fflush chamada na função main

![](https://i.imgur.com/HJNTQTb.png)

- "disas old_backdoor" - para investigar a função old_backdoor a função posicionada em cima da main

![](https://i.imgur.com/2tbtbVV.png)

- "disas system" - para investigar a função system chamada na função old_backdoor

![](https://i.imgur.com/tW6YCGn.png)

Concluindo a pesquisa, percebemos que :
1. É através da chamada feita à função __isoc99_scanf@plt na função main, que o programa recebe o input do utilizador; 
2. Ainda na função main, é chamada a função fflush que por sua vez realiza um jump. 
3. A função old_backdoor chama a função system;

Como estratégia, decidimos que a melhor opção seria usar o jump da função fflush a nosso favor, modificando o endereço para qual iria ser feito o salto. Este endereço passará a ser então, 0x08049236,
o endereço da função old_backdoor.

De modo o criar o exploit necessário para o desafio, utilizamos vários breakpoints no gdb para tentar perceber o tamanho da string de input. Depois de descobrirmos que seriam 60 bytes,passamos para o desenvolvimento do conteúdo da string.
Sendo o endereço da função old_backdoor 0x08049236, vimo-nos obrigados a cortar em duas partes devido ao seu comprimento.(0804 e 9236).
Traduzindo de hex para dec : 0804 -> 2052; 9236 -> 37430.

No conteúdo da string colocamos dois endereços, o endereço do jump com mais duas unidades adicionadas, onde seriam escritos os bytes 0x0804, e o endereço do jump, onde seriam escritos os bytes 0x9236. 
Antes de cada endereço foram escritos bytes aleatórios para que os %x antes de cada %n fossem consumidos sem ler os endereços colocados na string. Assim, os %n iriam alterar o endereço certo. Foi ainda necessário realizar algumas contas para calcular os número que funcionariam antes dos x's.
![](https://i.imgur.com/wjHmORK.png)


No fim, ao executar o ficheiro de python, foi aberta uma shell e de seguida, como previsto no inicio, para ler a flag corremos o comando cat flag.txt.
![](https://i.imgur.com/JICc4zs.png)


