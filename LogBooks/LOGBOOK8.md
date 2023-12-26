# Logbook 8

## Lab Environment

Para a realização das tarefas deste logbook era necessário mudar as permissões do ficheiro `/etc/hosts` e correr os comandos relacionados com o `docker-compose`.

## Tarefa 1 - Get Familiar with SQL Statements
A primeira tarefa consiste na familiarização com os comandos SQL. 
Portanto, depois de executarmos o que era pedido no enunciado procedeu-se ao uso de comandos SQL que permitissem obter a informação pedida - mostrar a informação do perfil da funcionária Alice.

![](https://i.imgur.com/ucqeyJt.png)

![](https://i.imgur.com/1K7D8oi.png)



## Tarefa 2 - SQL Injection Attack on SELECT Statement

### 2.1 SQL Injection Attack from webpage
O objetivo da primeira parte da tarefa 2 era fazer *log in* na página web como administradores de maneira a que consigamos ver todas as informações relativas aos funcionários.

Tendo em conta que o *username* é "admin" utilizamos o formato `admin'#` para "fechar" a string e o resto do código ficar comentado. Ao fazer isto é possível aceder ao site como administradores sem ser necessário colocar uma plavra-passe  

![](https://i.imgur.com/EPoMv0b.png)


### 2.2 SQL Injection Attack from command line
Nesta tarefa é pedido que se repita o ataque feito em 2.1, mas desta vez sem recorrer à página web. Para isso utilizamos o comando proposto - `curl`, ficando da seguinte forma: 

`curl 'http://www.seed-server.com/unsafe_home.php?username=admin%27%23&Password=pass'` 

Alterações para incluir caracteres especiais: 

- Alteramos " ' " para %27
- E "#" para %23

Ao executar o comando no terminal obtivemos o resultado apresentado na imagem seguinte.

![](https://i.imgur.com/h696mFj.png)


### 2.3 Append a new SQL statement
Para a realização desta tarefa inserimos, usando `UPDATE`, a seguinte declaração no campo do *username*: 

`admin'; update credential set Password= sha1('pass') where Name='Admin';#`

De seguida, tal como esperado, apareceu-nos o erro:

![](https://i.imgur.com/txge4Bw.png)

Depois de alguma pesquisa chegamos à conclusão de que o erro deveu-se à não possibilidade de "injetar" duas queries dentro de uma devido à função `query`.



## Tarefa 3 - SQL Injection Attack on UPDATE Statement

### 3.1: Modify your own salary

O objetivo da primeira parte da tarefa 3 é aumentar o salário do utilizador autenticado(Alice). Para isso, começamos por fazer login com o *username* = `alice'#` (mesma lógica da tarefa 2.1)

![](https://i.imgur.com/AFuH99H.png)

Depois fomos ao *Edit Profile* e preenchemos todos os campos à nossa vontade com especial atenção ao campo *NickName*. Neste o campo foi colocado `iamtheboss',Salary='999999`. Esta "declaração" preenche o campo *Nickname* com "iamtheboss" e o campo *salary* com 999,999.00€.

- Ataque a ser feito:

![](https://i.imgur.com/hvC7UqO.png)

- Perfil da Alice depois da atualização:

![](https://i.imgur.com/EQ5mrRj.png)

- Visualização na base de dados do utilizador Alice depois da atualização:

![](https://i.imgur.com/VSgN3RA.png)


### 3.2: Modify other people’ salary

O objetivo desta tarefa é "entrar" nos dados armazenados na base de dados do chefe (Boby) através do perfil da Alice. Para isso, utilizamos outra vez a ferramenta de edição de perfil e colocamos no campo *NickName*: `Urnottheboss', Salary=1 WHERE Name='Boby';#`. 
Este input coloca o Boby com *nickname*: "Urnottheboss" e salário: 1 dólar. Isto resulta pelo mesmo motivo da tarefa 2.1 em que `;#` "fecha" a string e comenta o resto do código e por isso conseguimos controlar a declaração `WHERE`.

- Ataque a ser feito:

![](https://i.imgur.com/xlwl01S.png)

- Perfil do Boby depois da atualização:

![](https://i.imgur.com/MlpN7SX.png)

- Visualização na base de dados do utilizador Boby depois da atualização:

![](https://i.imgur.com/EFlt3Sb.png)


### 3.3: Modify other people’ password

O objetivo desta tarefa é modificar a palavra-passe do Bob através do perfil da Alice. Para tal, voltamos a editar o perfil mas desta vez no campo *nickname* colocamos: `Urnottheboss', Password=sha1('notboss_1dolar') WHERE Name='Boby';#`, este input faz tem um efeito semelhante ao tópico 3.2 mas em vez do salário, modificamos a palavra-passe. Esta é codificada com `sha1` pois é assim que os dados são guardados na base de dados.

Nota: `sha1` -> função que calcula o hash `SHA-1` de uma string.

- Ataque a ser feito:

![](https://i.imgur.com/JUR8ilw.png)

- entrar no perfil do boby com a nova palavra-passe definida:

![](https://i.imgur.com/HRERP1k.png)

