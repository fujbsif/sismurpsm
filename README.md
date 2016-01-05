# smurpsm
Sistema de Monitoramento do Usuário da Rede de Proteção Social Municipal

Requisitos:
	- Sistema Operacional Ubuntu Linux 12.04.2 LTS (ou superior)
	- Apache 2.2
	- PHP 5.4
	- PostgreSQL 8.4
	- GD 5.3
	- Acesso Ã  internet, com a porta 80 tcp de entrada liberada

1. Abrir o terminal e se logar como administrador (root)
	
	sudo su
	
2. Instalar os pacotes apache2, php5, postgresql, php5-pgsq, php5-gd e ttf-mstcorefonts-installer

	apt-get install apache2 postgresql php5-pgsql php5-gd php5 ttf-mstcorefonts-installer

3.  Reiniciar o apache

	/etc/init.d/apache2 restart

4. Descompactar o arquivo pcni_htdocs.tar.bz2 no diretÃ³rio htdocs do apache
   (usualmente /var/www).

	cd /var/www
	tar -xvf <diretorio>/pcni_htdocs.tar.bz2

5. Logar-se como usuÃ¡rio 'postgres'

	su - postgres

6. Criar um usuÃ¡rio de banco de dados chamado 'semge', com senha 'semge', e
   permissÃ£o de superusuÃ¡rio

	createuser -sPE semge

7. Criar os seguintes usuÃ¡rios de banco de dados, com permissÃ£o de superusuÃ¡rio
   e sem senha: labbiousr, prematuser e rsi1

	createuser -s labbiousr
	createuser -s prematuser
	createuser -s rsi1

8. Criar um banco de dados chamado 'pcni', cujo dono Ã© o usuÃ¡rio 'semge' criado
   anteriormente

	createdb -O semge pcni
	

9. Carregar o script 'pcni_v0.sql' no banco de dados criado

	psql pcni < /var/www/pcni_v0.sql

10. Sair do login do usuÃ¡rio 'postgres' voltar para o login de administrador

        exit

11. Iniciar a configuraÃ§Ã£o do crontab para adiÃ§Ã£o de dois cronjobs

	crontab -e

12. Adicionar as seguintes linhas, salvar o arquivo e sair do editor

   	00 02 01 * * wget -q http://localhost/painel/dedurador.php
   	00 03 *  * * wget -q http://localhost/painel/cobrador.php

13. Mudar a permissÃ£o dos diretÃ³rios <htdocs>/educacao/images/alunos e
    <htdocs>/educacao/images/professores para dar permissao de escrita
    ao Apache

    	chmod 775 /var/www/educacao/images/{alunos,professores}
    	chgrp www-data /var/www/educacao/images/{alunos,professores}
    
14. Abrir o browser e colocar como endereÃ§o o ip do computador no qual o
    sistema estÃ¡ sendo instalado. AparecerÃ¡ uma tela com links para os diversos
    sistemas.  Cada um apresenta uma tela de autenticaÃ§Ã£o.

15. Para os sistemas da educacao, saude, CREAS, CRAS e painel Ã© necessÃ¡rio se
    autenticar como administrador e efetuar a configuraÃ§Ã£o de cada sistema
    (cadastro de usuÃ¡rio, etc).  Em sistemas recÃ©m-instalados, foi configurado o
    usuario 'admin', com senha '!qwe123123*' (sem aspas). Use-o para entrar 
    em cada sistema, lembrando de alterar esta senha para uma mais segura.

16. Mover os arquivos pcni_v0.sql, manual_instalacao.txt que estÃ£o no diretÃ³rio
    htdocs do apache para outro lugar (por exemplo, /root)

    mv /var/www/{pcni_v0.sql,manual_instalacao.txt} ~

17. O sistema estÃ¡ agora pronto para ser utilizado
