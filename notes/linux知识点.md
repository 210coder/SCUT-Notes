**在linux操作时，经常要查看运行的项目的进程和端口号，在这里总结了以下常用到的相关命令**

查看系统运行的java项目，并查看进程号

- ps -ef|grep java

**lsof命令**

根据进程pid查看端口号

- lsof -i | grep pid

根据用户查看进程和端口号

- lsof -i | grep user

根据端口号查看进程

- lsof -i:port

**netstat命令**

根据进程查看端口号

- netstat -nap | grep pid

根据端口号查看进程

- netstat -nap | grep port

6
