# LOGBOOK 11

## Tarefa 1: Becoming a Certificate Authority (CA)

Como pedido na primeira parte desta tarefa copiamos o ficheiro `open.ssl` para o nosso diretório e em seguida descomentamos a linha `unique_subject`.

![](https://i.imgur.com/k2cdEC2.png)

![](https://i.imgur.com/W5CAmNL.png)


Em seguida, como era pedido, criamos o ficheiro "index.txt" vazio e o ficheiro "serial" com o número 1612, como pode ser visto na imagem seguinte.

![](https://i.imgur.com/A0AWF5u.png)


Depois, era pedido que se executasse o comando apresentado no enunciado para criar um certificado *self-signed*.

![](https://i.imgur.com/aS4GmLr.png)


Por último, tínhamos que executar os seguintes comandos `openssl x509 -in ca.crt -text -noout` e `openssl rsa -in ca.key -text -noout` para responder às seguintes questões:

### 1. *What part of the certificate indicates this is a CA's certificate?*

![](https://i.imgur.com/womEosJ.png)


### 2. *What part of the certificate indicates this is a self-signed certificate?*
![](https://i.imgur.com/10g5ilN.png)

### 3. *In the RSA algorithm, we have a public exponent e, private exponent d, a modulus n, and two secret numbers p and q, such that n=pq. Please identify the values for these elements*
- e:

![](https://i.imgur.com/kxBPluD.png)

- d:

![](https://i.imgur.com/UkLPB34.png)

- modulus n:

![](https://i.imgur.com/CFSgFDU.png)

- p (prime 1) e q (prime 2):

![](https://i.imgur.com/6lrWnAE.png)


## Tarefa 2: Generating a Certificate Request for Your Web Server

Como pedido no enunciado, executamos o comando apresentado, substituindo com o nome do nosso servidor (smith2020).

![](https://i.imgur.com/feaU3AI.png)

Por último, adicionamos dois nomes alternativos ao nosso certificado, `smith2020.org` e `smith2020.io`.

![](https://i.imgur.com/S5lDJGI.png)

## Tarefa 3: Generating a Certificate for your server

Ao ler o enunciado desta tarefa, percebemos que tínhamos que criar o diretório "demoCA", e dentro deste criar a pasta "newcerts" visto que não o tínhamos feito anteriormente na tarefa 1, e passamos os ficheiros "index.txt" e "serial" para dentro de "demoCA". 

![](https://i.imgur.com/LESpFkL.png)

![](https://i.imgur.com/mBcgThw.png)

Depois disso, executamos o comando apresentado no enunciado para passar o certificado `server.csr` (*signing request*) para um `server.crt` (X509). Neste passo alteramos o nome da nossa cópia do openssl para estar de acordo com o exemplo do enunciado e assim, sabermos sempre que este se refere à cópia e não ao original.

![](https://i.imgur.com/x594k61.png)


Na segunda parte desta tarefa era pedido que se retirasse o comentário relativamente à linha apresentada na imagem seguinte para permitir a cópia do campo "extensions" para o certificado final.

![](https://i.imgur.com/WD9xgxh.png)

Depois disto, já possível verificar a existência de nomes alternativos.

![](https://i.imgur.com/3ECd81w.png)
Nota: Na revisão da tarefa, reparamos que onde se lê "www.smith.io" deve-se ler "www.smith2020.io", é de notar que esse erro foi corrigido posteriormente.



## Tarefa 4: Deploying Certificate in an Apache-Based HTTPS Website

O objetivo desta tarefa era configurar um website HTTPS baseado em Apache. Antes de seguir o exemplo, criamos uma pasta dentro do "Labsetup", "Smith2020_HTTPS", que contém todos os ficheiros necessários e para seguir a organização do diretório "image_www" onde se encontram os ficheiros, relativamente a "bank32", que servem de exemplo para a realização da tarefa.

Seguindo o exemplo dado no enunciado para a configuração do nosso site, substítuimos o necessário para estar de acordo com as nossas informações, como se pode ver nas imagens seguintes.

- `smith2020_apache_ssl.conf`

![](https://i.imgur.com/Dzow05c.png)

- Susbtituímos também pelas nossas referências tudo o que estava relacionado ao exemplo "bank32" no `DockerFile`

![](https://i.imgur.com/K8ddZFI.png)

- Por último, substítuimos no `docker-compose.yml` o campo "./image_www" pelo que está apresentado na imagem.

![](https://i.imgur.com/ZHy3KqO.png)


Em seguida, relaizamos os comandos apresentados no enunciado para permitir a visualização do site.

![](https://i.imgur.com/KPxAbiZ.png)


Por último, é pedido que tentemos aceder ao nosso site "https://www.smith2020.com", onde encontramos a seguinte a página:

![](https://i.imgur.com/gzDSEAm.png)

Depois de alguma pesquisa, percebemos que isto ocorre devido ao nosso certificado ser *self-signed* e por isso, apesar do firefox reconhecer que existe um certificado,  não "confia" nele pois não pertence à "lista" de CAs que conhece e confia.

Seguindo as instruções apresentadas no enunciado, adicionamos o certificado "ca.crt" ao Firefox, como se pode ver na imagem seguinte.

![](https://i.imgur.com/QBybEY4.png)

Depois de termos providenciado o certificado, já é possível visualizar o nosso site sem aparecer o aviso que aparecia anteriormente.


![](https://i.imgur.com/r83x3tm.png)

![](https://i.imgur.com/1QoZX07.png)




## Tarefa 5: Launching a Man-In-The-Middle Attack

O objetivo desta tarefa era entender como é que PKI se pode defender de ataques de *Man-In-The-Middle*. Esta tarefa está dividida em duas partes. A primeira parte tem como objetivo a configuração do website malicioso. Para isto, repetimos a mesma configuração feita na tarefa 4 mudando o site de https://www.smith2020.com para https://www.facebook.pt.

![](https://i.imgur.com/FfWo2Dq.png)

![](https://i.imgur.com/itO2vWF.png)

Na segunda parte nós tornámo-nos o *man in the midle*. Para isso, simulamos um ataque ao DNS modificando o ficheiro `/etc/hosts` para mapear o hostname https://www.facebook.pt.

![](https://i.imgur.com/xLE7z0i.png)

Por fim, visitamos o website e num primeiro momento fomos presenciados com um *warning* porque o certificado que tínhamos não conseguia provar a nossa identidade.

![](https://i.imgur.com/kBXYAih.png)

Mas se continuarmos e assumirmos o risco, conseguimos aceder ao website Facebook tal como era esperado.

![](https://i.imgur.com/DW85Vne.png)


## Tarefa 6: Launching a Man-In-The-Middle Attack with a Compromised CA

O objetivo desta tarefa era entender como uma CA compremetida pode permitir um ataque MITM. Para isso usamos o certificado que já esta presente (correspondente ao bank32), adicionando apenas o suporte para o website https://www.facebook.pt. (Com os comandos que aparece nas imagens abaixo)

![](https://i.imgur.com/S1fvb0q.png)

![](https://i.imgur.com/PvdiroO.png)

Por fim, voltamos ao *website* e desta vez não nos apareceu nenhum *warning*, sendo por isso considerado pelo *web browser* uma ligação segura e legítima.

![](https://i.imgur.com/etRHHv5.png)

