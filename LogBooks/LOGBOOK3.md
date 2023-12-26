
# Trabalho realizado na Semana #3

## Identificação

- O Raccoon Attack explora uma falha na especificação TLS  que pode levar um invasor a conseguir calcular o segredo pré-master em conexões.
- Vulnerabilidade que afeta HTTPS e outros serviços que dependem de SSL (Secure Sockets Layer) e TLS (Transport Layer Security).
- Esta vulnerabilidade afeta especificamente TLS 1.2 e as suas versões anteriores de protocolo de encriptação.
- Estas versões recorrem ao uso do método Diffie-Hellman (DH) para troca de chave. 

## Catalogação

- Esta vulnerabilidade foi descoberta por investigadores de universidades alemãs e israelitas em 2020.
- O problema foi resolvido para a versão TLS 1.3 por David Benjamin.
- Afeta OpenSSL 1.0.2 (que já não é suportado e não recebe updates), versão 1.1.1 já não é afetada.
- Nível baixo de gravidade - impacto parcial à confidencialidade e nenhum impacto a nível de disponibilidade ou de integridade.

## Exploit

- Exploit de elevada dificuldade. 
- Apenas ocorre se o servidor atingido reutilizar a chave pública do tipo DH.
- Tem como base medições do tempo de encriptação e a criação de equações para computar a chave original.
- Sendo uma vulnerabilidade proveniente do servidor, os utilizadores apenas conseguem prevenir estes ataques assegurando-se que os seus web browsers não recorrem ao uso de DH.

## Ataques

- Capacidade de desencriptar seções de TLS e ler comunicações sensíveis entre o utilizador e o servidor.
- Incluindo acesso a usernames, passwords, números de cartões de crédito, emails, mensagens e documentos dos utilizador.
- Um dos únicos ataques conhecidos relacionados a esta vulnerabilidade foi o Lucky 13.
- Muitos dos ataques realizados são considerados impotentes, não tendo  impacto significativo na integridade dos softwares.
