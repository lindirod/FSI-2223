# CTF : Semana 4 - Web

> **Objetivo:** Fazer login como administrador num servidor wordpress utilizando uma CVE com exploit conhecido.

## Desafio 1

Seguindo as recomendações do enúnciado, começou-se por recolher algumas informações a partir da navegação pela aplicação web. Nomeadamente, a versão do wordpress, plugins instalados e versões dos mesmos e por último, possíveis utilizadores e nomes de utilizadores.

Destacou-se uma review do utilizador Orval Sanford, na qual se queixava da falta de segurança, referindo ainda que os plugins do wordpress não eram regularmente atualizados. Através da resposta a este comentário, concluímos qua havia ainda uma conta denominada por admin.

![](https://i.imgur.com/T0LAlI9.png)

Tendo em conta a review, decidimos pesquisar por CVE's associados a WooCommerce plugin da versão 5.7.1 e a Booster for WooCommerce plugin da versão 5.4.3. Após alguma procura, achamos a CVE-2021-34646
que afetava WooCommerce Booster Plugin 5.4.3. Após introduzir este CVE na flag, este desafio foi completado.

## Desafio 2

Para completar o segundo desafio, foi necessário ir em busca do exploit da vulnerabilidade que tínhamos encontrado no primeiro desafio.

Acedendo à plataforma Exploit Database, conseguimos obter o seu exploit e o método de aceder a uma conta já registada no site. 

Ao correr o código com o ID=1, foi dado o acesso à conta do admin através de um dos links devolvidos pela execução do exploit. 

![](https://i.imgur.com/CepP0IJ.png)



Já dentro da conta, a flag foi encontrada numa mensagem dirigida aos empregados que se localizava na secção dos Posts.

![](https://i.imgur.com/bHfNDpA.png)



