
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

#### Arquivo de auto instalação
> Responda o comando **y** para todos.

```bash
sudo apt-get install dos2unix && dos2unix webzow_v2_autoinstaller.sh && wget https://webzow.com/v2/webzow_v2_autoinstaller.sh && chmod +x webzow_v2_autoinstaller.sh && ./webzow_v2_autoinstaller.sh
```

## 2. - Configurar Servidor MySQL

> Uma observação. Caso deseje acessar seu banco de dados remotamente, apenas siga as etapas abaixo da etapa 1. Se não, apenas ignore-o!

1. Habilitar acesso MySQL Remoto (opcional)
2. 
```bash
nano /etc/mysql/mysql.conf.d/mysqld.cnf
bind-address = 0.0.0.0
systemctl restart mysql
ufw allow 3306/tcp
ufw reload
```

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


***No seu servidor de arquivo, vá para a pasta****

```file
/var/www/html
```

***Faça upload do arquivo ZIP do projeto e execute o comando***
> NOME_DO_ARQUIVO -> Digite exatamente o nome do arquivo que irá descompactar.

```bash
unzip NOME_DO_ARQUIVO.zip
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
> ***Contato:*** contato@webzow.com.
> 
> ***Note:*** This quickstart is for scripts V2 of WebZOW, so you need consult documentation guide for more details.
>
> Credit for mmatech by tutorial.
