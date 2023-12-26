# Apply For Flag II

Com o pequeno texto associado a este ctf fomos informados que este desafio consistia numa "versão melhorada" do desafio 1 da semana 10. A vulnerabilidade de XSS teria sido removida através da separação do site.

Ao abrirmos a página relativa ao ctf, encontramos uma página parecida à do desafio previamente realizado mas agora com um título que indicaria o *id* da página onde o pedido da flag irá ser verificado. Ao dar *refresh* à página, o id associado à página de verificação mudava.

![](https://i.imgur.com/rhCnx0K.png)

Numa tentativa de perceber se o input avaliado ainda seria sensível a *javascript*: 

![](https://i.imgur.com/Iz5t1eN.png)
![](https://i.imgur.com/B13dzUc.png)
![](https://i.imgur.com/taCn2dx.png)
![](https://i.imgur.com/V1hbwmO.png)

Inicialmente, foi ultilizada a abordagem de acessar o botão *Give the flag* da página de verificação e com *javascript* realizar um click. Foi utilizados vários inputs para testar método mas nada parecia funcionar, visto que o pedido nunca era aprovado.


```
<script>jQuery('#giveflag').load('http://ctf-fsi.fe.up.pt:5005/request/4d8e67001230759d4ce9bca00be3dcc370c3ccde #giveflag').click()</script>

<script>('http://ctf-fsi.fe.up.pt:5005/request/192ab45111b8e653675240bb467a14bfc04df20c).contentWindow.document.getElementById('giveflag').click()</script>

<script>var url ='http://ctf-fsi.fe.up.pt:5005/request/981392087107d0b674a0b26465250f2b8748a29b' ;  url.document.getElementById('giveflag').removeAttribute('disabled')</script>
```


Decidimos então mudar de abordagem. Sabendo que o input ("Beg for flag...") seria colocado dentro do html da página, poderíamos criar um botão igual ao botão *Give the flag* (com o mesmo id) dentro de um form com um pedido para a página de verificação. Ao clicar nele com *javascript*, iria ser criado um pedido que forçaria a ação de receber a flag através de um clique no botão já existente. O código formado : 

![](https://i.imgur.com/mULfakE.png)

No entanto, ao colocar este input fomos redirecionados para esta página que nos indicou que este método de pedido não era permitido para a página em questão.

![](https://i.imgur.com/ZOMTSWo.png)

Sem saber como resolver, tentamos algumas variações do código antes de desistir completamente desta abordagem. Uma delas foi acrescentar ao URL */approve*.

![](https://i.imgur.com/Il4zmgD.png)

Ao colocar este input, fomos redirecionados para uma página diferente. A página exibida apresentava uma mensagem de erro "Forbidden" e a mensagem "You don´t have the permission to access the requested resource. It is either read-protected or not readable by the server.".

![](https://i.imgur.com/dJuox4L.png)
![](https://i.imgur.com/rBx4kgS.png)

Aí percebemoss que estavamos no caminho certo, visto que era permitido fazer um pedido para aquele URL. Depois de alguma pesquisa percebemos que o *javascript* estaria a "proibir" o acesso à página. Experimentámos desativá-lo e atualizar a página. Conseguimos assim ter acesso à página "Justification for the application", onde aparecia como suposto o botão criado "Give the flag". Depois de algum tempo com a frase "Your request hasn't been evaluated yet",
esta foi substituida pela flag.

![](https://i.imgur.com/jJMX5Tm.png)

![](https://i.imgur.com/CTsldgs.png)

