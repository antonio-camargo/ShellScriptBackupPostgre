# ShellScriptBackupPostgre

Este Script tem a finalidade de otomizar o backup, e antes de finalizar o script criaremos outro banco de dados e restauramos o backup para teste, quando for fazer o backup novamente antes excluirá o banco criado, assim dará tempo para verificação do banco teste, antes de encerrar o script irá compactar o arquivo de backup e excluirá o outro arquivo não compactado, ainda criará uma pasta de log para armazenar a data de todos os backups realizados.


<pre>#!/bin/bash</pre>

Declarando variaveis

<pre>data=`date "+%Y%m%d-%H:%M"`
usuario=postgres
host=localhost
banco=postgre
senha=123456
restaurar=postgre
teste= bancodeTeste</pre>


#Conectando ao banco de dados

<pre>su - postgres</pre>

#Excluindo o bancoTeste criado para teste no primeiro backup

<pre>psql -c dropdb $teste</pre>

#Realizando Backup do banco de dados.

<pre>pg_dump -h localhost -p 5432 -U postgres -v -f "/backup/$data.dbserver.sql"</pre>

#Criando um banco teste e restaurando Backup do banco de dados.

<pre>psql -c createdb $teste

pg_restore -U $usuario -h $host -d  $teste /backup/$data.dbserver.sql</pre>

#Compactando o Backup do banco de dados 

<pre>tar -zcf /backup/compactado/$data.dbserversql.tar /backup/$data.dbserver.sql</pre>

#Apagando arquivo sem compactacao do Backup

<pre>rm -rf /backup/$data.dbserver.sql</pre>

#Criando uma pasta de log para os backups feitos e acrescentando a data toda vez que o backup for feito

<pre>$data >>/var/log/backup.log</pre>
