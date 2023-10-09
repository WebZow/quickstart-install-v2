#  Installation Guide - iGaming v2.4.2-final

Olá, **Dev**!
 Você possui um script para instalação da WebZOW. Isso significa que você escolheu efetuar a instalação por conta própria, aqui está um **passo-a-passo** de como executar nosso script.

> ***Evite o uso não autorizado de scripts WebZOW. 
> O uso de script pirateado/nulled não está de acordo á suporte da empresa!***

# Requisitos

* Servidor Linux Ubuntu 20.04 (LTS) x64 (de preferência na DigitalOcean.com)
Configuração suficiente - 2 GB Memória  / 1 AMD vCPU  / 50 GB  Disk
 * Experiência básica com comandos Linux (Criar, Autenticar, Executar comandos)

## 1. - Instalação e Configuração

> Certifique-se de executar os comandos um a um e por ordem!

#### Liberar portas do Firewall (WEB, SSH, Redis e Socket)

```bash
sudo ufw allow https
sudo ufw allow http
sudo ufw allow 22
sudo ufw allow 6379
sudo ufw allow 7777
sudo ufw status
sudo ufw enable
```
---
#### Certificar que Apache não está instalado.

```bash
sudo apt remove apache2  
sudo apt autoremove
```
---
#### Instalar todas as ferramentas necessárias.

> Execute cada comando separado e aguarde a finalização.
> 
```bash
sudo apt update && sudo apt upgrade -y
apt-get install software-properties-common python-software-properties
apt install software-properties-common
add-apt-repository -y ppa:ondrej/php
apt-get update
```

```bash
sudo add-apt-repository -y ppa:ondrej/php
sudo apt-get install software-properties-common
sudo apt install nginx
apt-get -y install mysql-client mysql-server
```
---
> **Este em exceção, execute todos de uma vez, copie o comando inteiro e execute-o!**

```bash
sudo apt-get install software-properties-common && 
sudo add-apt-repository -y ppa:ondrej/php && 
sudo apt update && sudo apt upgrade && 
sudo apt install php7.3-common php7.3-zip php7.3-curl php7.3-xml php7.3-xmlrpc php7.3-mysql php7.3-pdo php7.3-gd php7.3-imagick php7.3-ldap php7.3-imap php7.3-mbstring php7.3-intl php7.3-cli php7.3-tidy php7.3-bcmath php7.3-opcache
```

```bash
apt-get -y install curl php7.3 php7.3-mysql  php7.3-fpm php7.3-mbstring php7.3-xml php7.3-curl
```

> **Neste de baixo, todos os comando juntos de uma só vez!**

```bash
sudo apt install python3 python3-venv libaugeas0 &&  
sudo python3 -m venv /opt/certbot/ &&  
sudo /opt/certbot/bin/pip install --upgrade pip &&  
sudo /opt/certbot/bin/pip install certbot certbot-nginx &&  
sudo ln -s /opt/certbot/bin/certbot /usr/bin/certbot  
```
#### Gerar certificado SSL

> Se pedir e-mail, preencha, irá pedir o domínio do seu site. Digite (www.seusite.com)

```bash
sudo certbot --nginx
```
---
#### Instalar Redis
> Copie tudo e execute de uma só vez!

```bash
sudo apt update && sudo apt upgrade &&  
sudo apt install redis-server &&  
sudo systemctl status redis &&  
sudo systemctl enable redis &&  
redis-cli
```


## 2. - Configurar Servidor MySQL

> Uma observação. Caso deseje acessar seu banco de dados remotamente, apenas siga as etapas abaixo da etapa 1 até 3. Se não, apenas ignore-o!

##### Configurar Acesso Remoto do MySQL

```bash
mysql -u root
```
##### Digite cada comando e pressione Enteder para ser enviado e processado.
> Altere os textos para os que deseja. Neste caso Nome do Usuário e Senha.
* YOUR_USER_NAME -> Será o nome do usuário para efetuar acesso ao seu banco de dados.
* YOUR_PASSWORD_HERE -> Sua senha para acessar o usuário.
* YOUR_DATABASE_NAME -> Nome do Banco de Dados que deseja.
  
```bash
CREATE USER 'YOUR_USER_NAME'@'%' IDENTIFIED BY 'YOUR_PASSWORD_HERE';  
GRANT ALL PRIVILEGES ON *.* TO 'YOUR_USER_NAME'@'%';  
ALTER USER 'YOUR_USER_NAME'@'%' IDENTIFIED WITH mysql_native_password BY 'YOUR_PASSWORD_HERE';  
FLUSH PRIVILEGES;  
create database YOUR_DATABASE_NAME;
```

---
##### Instalação de Pacotes
> Execute cada comando separado!

```bash
cd ~
sudo apt-get install -y nodejs
npm install socket.io@2.0.4 qrcode express fs https http crypto request mysql sha256 mathjs randomstring body-parser nodemailer multer

curl -sL https://deb.nodesource.com/setup_16.x | sudo -E bash -  
sudo apt-get install -y nodejs  
apt-get update  
npm i -g pm2

sudo service nginx restart
```


## 3. Pastas & Arquivos

**Você precisará editar alguns arquivos e criar outros. Aqui estão os passos.**
> Recomendo usar algum software de FTP, neste caso Filezilla Client ou WinSCP para Windows.
#### Acesse a pasta

```
/run/redis
```
#### Crie um arquivo sem extensão, chamado de

```
redis.sock
```

---
#### Vá até a pasta

```
/etc/nginx/sites-available
```

#### Faça um backup do arquivo 'default' no seu computador local e então edite ele para.
> **Observação:**  Edite as variáveis cujo nome seja respectivo ao seu uso.
> IP_DO_SEU_SERVIDOR -> Digite o IP do seu servidor.
> www.SEU_DOMINIO.COM -> Digite o domínio do seu site.

Acesso: [Script nginx Default para copiar e editar](https://pastebin.com/raw/Q08QCp5V)


## Subir o projeto

#### Instalar pacote do unzip

```bash
sudo apt-get install unzip
```

***No seu servidor de arquivo, vá para a pasta****

```file
/var/www/html
```

***Faça upload do arquivo ZIP do projeto e execute o comando***
> NOME_DO_ARQUIVO -> Digite exatamente o nome do arquivo que irá descompactar.

```bash
unzip NOME_DO_ARQUIVO.zip
```

#### Adicionar poermissão nas pastas
```bash
chmod -R 777 storage
chmod -R 777 bootstrap
```

### Agora, edite o arquivo .env de acordo com login do seu banco de dados e URL configurada no nginx.

---
### Configurar Redis

Vá a pasta e coloque o arquivo deste repositório no lugar

```
/etc/redis/redis.conf
```

```bash
sudo service redis restart
```

---
#### Ligar o Servidor & BOT
**Vá até a pasta**

```bash
/var/www/html/storage/bot
```
**Executa o comando**

```bash
pm2 start app.js
```

### Finalizado!

![image](https://github.com/WebZow/quickstart-install-v2/assets/6683056/c32a5b10-4f20-4536-8cf4-5f66133a10ce)

### Comandos PM2 caso de erro

```base
pm2 log app.js
pm2 stop app.js
pm2 restart app.js
pm2 flsush
```

## WebZOW - Zero Obstacles on Web
> ***Contato:*** contato@webzow.com
> ***Note:*** This quickstart is for scripts V2 of WebZOW, so you need consult documentation guide for more details.
