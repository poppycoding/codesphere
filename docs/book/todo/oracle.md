1. Oracle 为了均衡电脑性能和数据库性能, 默认内存大小为物理内存的 1/8, sqlplus 修改参数, 重启服务即可:
```shell
sqlplus system/root@orcl
show parameter sga
alter system set sga_max_size=768m scope=spfile;
alter system set sga_target=768m scope=spfile;
```
不可修改过小 < 636M, 否则出现下面第三条的错误!!!

2. 重启服务 / 重启电脑
```shell
net stop oracleserviceorcl
lsnrctl stop
net start oracleserviceorcl
lsnrctl start
lsnrctl status
```

3. 如果修改的 size 过小, 数据库将启动失败报错
```text
ORA-01034: ORACLE not available
ORA-27101: shared memory realm does not exist
ORA-00823: Specified value of sga_target greater than sga_max_size
ORA-00823: Specified value of sga_target 128M is too small, needs to be at least 636M
```

解决办法: 
```shell
sqlplus /nolog
conn / as sysdba
# 创建临时文件 initbak.ora, 然后修改 sga 属性
create pfile='c:\temp\initbak.ora' from spfile;

# 启动-关闭
startup pfile='C:\Oracle\client\product\admin\orcl\pfile\initbak.ora'
shutdown immediate;

# 重新创建 spfile
create spfile frompfile = 'C:\Oracle\client\product\admin\orcl\pfile\initbak.ora'
startup
```
