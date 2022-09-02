# sllab3-assign-1-3rd-question
In this task it has been asked to do mysql  master slave replication .
for this we require two ubuntu system with all permissions granted to the user
first we have fix a machine for master and another for slave.
first we have to open file /etc/mysql/mysql.conf.d/mysqld.cnf of master  using sudo command in your preffered editor.
here in that file we have to change the bind-address to server_ip of master,then we have to give server-id to 1.
then restart the mysql using command : sudo systemctl restart mysql.
Now we have to create a replica user : In master we have to login to mysql then we have
to execute the command :CREATE USER 'replica_user'@'replica_server_ip' IDENTIFIED WITH mysql_native_password BY 'password';
Here replica_server_ip must be the ip address of slave .and password is the preffered one that you want to keep.
Then run a command :FLUSH PRIVILEGES;
Now you can check master status with command :SHOW MASTER STATUS;
which will give the bin file name and the position of that file.
Now for configuring replica similar to the previous we have to open a file /etc/mysql/mysql.conf.d/mysqld.cnf in slave and change the bind adress to the its ip and give server-id different from the master.
Now once again restart the mysql using the command :sudo systemctl restart mysql.
Now we are ready to replicate the data .
In slave we have to run a command:
CHANGE REPLICATION SOURCE TO
SOURCE_HOST='source_server_ip',
SOURCE_USER='replica_user',
SOURCE_PASSWORD='password',
SOURCE_LOG_FILE='mysql-bin.000001',
SOURCE_LOG_POS=899;
here the SOURCE_LOG_FILE and SOURCE_LOG_POS should be the name of the bin file and position we got from the command show master status respectively;
NOW you can type the commands:START REPLICA;
this command will start the replica 
next give command :SHOW REPLICA STATUS\G;
this will give certain output in which if we see replica_io :running and replica_sql:running and replica_status as waiting for source to send event then we can say that our replication has been start.

