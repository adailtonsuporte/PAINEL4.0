# PAINEL4.0
PAINEL SSH 4.0 ,,

Instalação do Painel WEB de Gerenciamento SSH 4.0  Debian 8 32x / Debian 8 64x 

 ATENÇÃO IMPORTANTE 
 
• Sempre que solicitado [Y/N] escolha a opção Y (Sim para Todos).  
• Sempre que solicitado digite sempre a mesma senha de sua preferencia (letras e numeros, não use caracteres especiais ou espaço)  
• Para a seleção do servidor, escolha apache2. 
• Selecione Sim quando perguntado se deseja usar dbconfig-common para configurar o banco de dados.  

1 - Vamos instalar o Apache  

wget http://hostgrid.com.br/downloads/install

bash install


• Continue somente apos aparecer a mensagem COMPLETADO COM SUCESSO.  

service apache2 restart 

2 - Criar o Banco de Dados SSH 
• Altere o comando abaixo trocando SUA_SENHA pela senha que voce esta utilizando, o -p deve ficar junto a sua senha sem espaço 

• Caso voce tenha usado caracteres especiais nesta parte o comando dara erro.  


mysql -h localhost -u root -pSUA_SENHA -e "CREATE DATABASE ssh"  


service apache2 restart 


3 - Configurar arquivo de acesso ao Banco de Dados  


nano /var/www/html/pages/system/pass.php 


• O comando acima cria o arquivo pass.php e abre a edição do mesmo 

• Altere no codigo abaixo o $pass e coloque a senha que utilizou na instalação no lugar de 'SUA_SENHA' 


<?php $pass = 'SUA_SENHA';?> 


• Copie o codigo acima e cole na edição do arquivo pass.php 

• Apos colar o comando aperte Ctrl+X e escola opção Y depois aperte Enter 

4 - Configurar Tabelas do Banco de Dados 

• Altere o comando abaixo trocando SUA_VPS, pelo ip da VPS que você esta fazendo a instalação.  


curl IP_DA_SUA_VPS/create.php  

cd /var/www/html

rm create.php  

service apache2 restart 

cd

5 - Configurarando comando de Agendamento Crontab 

• Os comandos a seguir serão responsaveis por fazer a atualização dos usuarios online no painel.  


crontab -e 


• Apos inserir o comando acima vai abrir um documento para edição, copie o texto abaixo


* * * * * /usr/bin/php /var/www/html/pages/system/cron.php
* * * * * /usr/bin/php /var/www/html/pages/system/cron.ssh.php 
* * * * * /usr/bin/php /var/www/html/pages/system/cron.sms.php 
* * * * * /usr/bin/php /var/www/html/pages/system/cron.online.ssh.php 
10 * * * * /usr/bin/php /var/www/html/pages/system/cron.servidor.php 

• Cole no documento na ultima linha. 

• Apos colar o comando aperte Ctrl+X e escola opção Y depois aperte Enter   

6 - Alterando a Data e a Hora  


dpkg-reconfigure tzdata   


• Escolher opção America e depois São Paulo.  


service apache2 restart 


FINAL 

• Pronto agora seu painel ja esta pronto, voce pode logar da seguinte forma:  IP_DOMINIO-VPS/admin/  Usuario: admin  Senha: admin 

• Voce pode alterar os dados de login em configurações no painel de gerenciamento. 

• Lembrando que ainda falta configurar a VPS que sera o servidor ssh. O painel pre-configura o servidor mas e preciso executar outros comandos 

CONTINUE ABAIXO NA VPS QUE SERA O SERVIDOR SSH



Instalação Servidor SSH  Ubuntu 16.4 / Ubuntu 14.4 

1 - Baixe os scripts para a vps.  


crontab -e 


• Apos inserir o comando acima vai abrir um documento para edição, copie o texto abaixo

* * * * * /usr/bin/php /var/www/html/pages/system/cron.php
* * * * * /usr/bin/php /var/www/html/pages/system/cron.ssh.php 
* * * * * /usr/bin/php /var/www/html/pages/system/cron.sms.php 
* * * * * /usr/bin/php /var/www/html/pages/system/cron.online.ssh.php 
10 * * * * /usr/bin/php /var/www/html/pages/system/cron.servidor.php 


wget http://hostgrid.com.br/downloads/vpsmanagersetup.sh 
  

apt-get update && apt-get upgrade 

3 - Executar comando de permissão.  

chmod 777 vpsmanagersetup.sh 

4 - Feito isso agora vamos instalar o vpsmanager 2.0
  
./vpsmanagersetup.sh 


• Sigua as orientações que a instalação vai pedir, tenha certeza que o IP apresentado na instalação eo mesmo da sua VPS. 

5 - Alterando a Data e a Hora  


dpkg-reconfigure tzdata   


• Escolher opção America e depois São Paulo. FINAL 

• Pronto agora seu servidor ja está configurado e pronto para ser utilizado no painel de gerenciamento, basta adicionar atraves do login de Administrador.
