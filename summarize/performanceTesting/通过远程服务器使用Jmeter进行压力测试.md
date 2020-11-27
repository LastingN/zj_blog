# 通过远程服务器使用Apache JMeter进行压力测试

​	通过连接远程服务器，调用jmeter进行noGUI压力测试

### Windows连接远程服务器

1.	下载XShell
  2.	生成秘钥
  3.	将公钥提交给服务器管理者
  4.	连接远程服务器主机,如39.96.206.141

###  linux连接远程服务器

 1. 通过终端生成秘钥

    `ssh-keygen -t rsa `

 2. 根据不同用户，查找“.ssh”文件，如root用户秘钥位于/root/.ssh/目录下

 3. id_rsa 私钥

    id_rsa.pub公钥

 4. 将公钥提交给服务器管理者

 5. 通过命令访问远程服务器

    `ssh root@39.96.206.141`

    

    

### 操纵远程服务器进行压力测试

1. linux将.jmx和.csv文件传输到远程服务器上

   `scp stress_testing.jmx root@39.96.206.141:~/stress_testing/`

   `scp user.csv root@39.96.206.141:~/`

   Windows将文件传输到远程服务器上

   `rz`

2. 通过`jmeter -v` 查看服务器是否设置了环境变量，

   + 如显示版本号，则表示设置了环境变量，可直接执行jmeter命令，

   + 如果没有显示版本号，则需要进到`~/stress_testing/apache-jmeter-5.1.1/bin/`下执行jmeter命令

   + 或者配置jmeter环境变量

     ```
     root@testing:~# vi /etc/profile
     
     JMETER=/root/stress_testing/apache-jmeter-5.1.1
     CLASSPATH=$JMETER/lib/ext/ApacheJMeter_core.jar:$JMETER/lib/jorphan.jar:$CLASSPATH
     PATH=$PATH:$JMETER/bin
     export JMETER PATH
     
     root@testing:~# source /etc/profile
     ```

3. 远程服务器执行

   ` jmeter -Jthreads=100 -Jloops=10 -Juser="/root/user.csv" -n -t stress_testing.jmx -l stress_result.csv -e -o stress_result`

   `-Jthreads`：线程数

   `-Jloops`：每个线程运行数

   `-Juser`：csv数据文件的全路径

   `-n`：不启动GUI图形界面

   `-t stress_testing.jmx  `：后面跟测试方案的文件名

   `-l stress_result.csv`：测试过程中产生的数据

   `-e -o stress_result ` ：对最终的结果进行分析，并且出具报告，生成文件夹

   ```
   root@testing:~/stress_testing# jmeter -Jthreads=100 -Jloops=10 -Juser="/root/user.csv" -n -t stress_testing.jmx -l stress_result.csv -e -o stress_result
   Creating summariser <summary>
   Created the tree successfully using stress_testing.jmx
   Starting the test @ Tue May 14 14:44:59 CST 2019 (1557816299021)
   Waiting for possible Shutdown/StopTestNow/HeapDump/ThreadDump message on port 4445
   summary +    190 in 00:00:01 =  214.2/s Avg:   111 Min:    18 Max:   194 Err:     0 (0.00%) Active: 85 Started: 85 Finished: 0
   summary +   2916 in 00:00:30 =   97.2/s Avg:   516 Min:     7 Max: 26674 Err:     0 (0.00%) Active: 100 Started: 100 Finished: 0
   summary =   3106 in 00:00:31 =  100.5/s Avg:   492 Min:     7 Max: 26674 Err:     0 (0.00%)
   summary +   1042 in 00:00:30 =   34.4/s Avg:  1063 Min:     7 Max: 30656 Err:     0 (0.00%) Active: 100 Started: 100 Finished: 0
   summary =   4148 in 00:01:01 =   67.8/s Avg:   635 Min:     7 Max: 30656 Err:     0 (0.00%)
   summary +   1666 in 00:00:30 =   56.0/s Avg:  2482 Min:     7 Max: 61004 Err:    58 (3.48%) Active: 100 Started: 100 Finished: 0
   summary =   5814 in 00:01:31 =   64.0/s Avg:  1164 Min:     7 Max: 61004 Err:    58 (1.00%)
   summary +    741 in 00:00:30 =   24.7/s Avg:  4418 Min:    39 Max: 60005 Err:    28 (3.78%) Active: 100 Started: 100 Finished: 0
   summary =   6555 in 00:02:01 =   54.2/s Avg:  1532 Min:     7 Max: 61004 Err:    86 (1.31%)
   summary +    563 in 00:00:30 =   18.7/s Avg:  6488 Min:   640 Max: 60005 Err:    30 (5.33%) Active: 100 Started: 100 Finished: 0
   summary =   7118 in 00:02:31 =   47.2/s Avg:  1924 Min:     7 Max: 61004 Err:   116 (1.63%)
   summary +    786 in 00:00:30 =   26.1/s Avg:  4472 Min:   690 Max: 60007 Err:    17 (2.16%) Active: 100 Started: 100 Finished: 0
   Tidying up ...    @ Tue May 14 14:37:47 CST 2019 (1557815867937)
   ... end of run
   root@testing:~/stress_testing#
   ```

4. 打包文件

   `zip -r aa.zip stress_result*`

5. windows 通过XShell连接，可直接使用`sz aa.zip` 下载文件

   linux 在本机终端 `scp root@39.96.206.141:/root/stress_testing/aa.zip /home/Jmeter/aa.zip`

6. 解压文件，进入文件夹，打开index.html文件，查看结果



### 对服务器进行分布式压力测试

| 名称      | 公网IP       | 内网IP      |
| --------- | ------------ | ----------- |
| jmeter001 | xxx.xx.x.xxx | 172.16.6.70 |
| jmeter002 |              | 172.16.6.71 |
| jmeter003 |              | 172.16.6.72 |
| jmeter004 |              | 172.16.6.73 |



1. 进入控制服务器

   `ssh root@xxx.xx.x.xxx`

2. 将本地的压力测试脚本.jmx文件与csv数据文件发送到控制服务器

   `scp  stress_testing.jmx    root@123.56.5.254:~/stress_testing/`

   `scp    pro_user.csv   root@123.56.5.254:~/  `

3. 修改jmeter.properties

   `server.rmi.ssl.disable=true`

4. 在控制服务器进入子控制服务器修改jmeter-server

   `ssh   root@172.16.6.71`

   `cd   stress_testing/apache-jmeter-5.1.1/bin/`

   `vim   jmeter-server`

   修改第30行 `RMI_HOST_DEF=-Djava.rmi.server.hostname=172.16.6.45` 后面的内网IP为主控制服务器的内网IP

   返回后保存，运行jmeter-Server

   如果子控制服务器已运行jmeter-server，但是没有显示的话，执行

   `jps -l`

   然后使用kill 杀掉已执行的jmeter-server即可

5. 修改完所有的子控制服务器并且全部打开jmeter-server后，对主控制服务器修改hosts文件

   `vim   /etc/hosts`

   `172.16.6.71     jmeter002       jmeter002
    172.16.6.72     jmeter003       jmeter003
    172.16.6.73     jmeter004       jmeter004`

6. 执行测试计划

   `jmeter -Gthreads=100 -Gloops=10 -Gdemo_accounts="/root/stress_testing/pro_user.csv" -R jmeter001，jmeter002,jmeter003,jmeter004 -n -t jmeter-test-demo.jmx -l stress_result.csv -e -o stress_result`

   

   如果报错：`The program 'jmeter' is currently not installed. You can install it by typing:
   apt install jmeter` 请参考**操纵远程服务器进行压力测试的第二步**

   如果签名不对，将`mosoink-jmeter-functions-1.0.jar`包传到主控制服务器上，然后再由主控制服务器分发到子控制服务器

   ```
   root@jmeter001:~/stress_testing/apache-jmeter-5.1.1/bin# ./jmeter -Gthreads=10,-Gloops=1 -Guser=/root/stress_testing/pro_user.csv -R jmeter002,jmeter003,jmeter004 -n -t /root/stress_testing/stress_testing.jmx -l /root/stress_result.csv -e -o /root/stress_result
   Creating summariser <summary>
   Created the tree successfully using /root/stress_testing/stress_testing.jmx
   Configuring remote engine: jmeter002
   Configuring remote engine: jmeter003
   Configuring remote engine: jmeter004
   Starting remote engines
   Starting the test @ Tue Jun 25 16:59:42 CST 2019 (1561453182601)
   Error in rconfigure() method java.rmi.ConnectException: Connection refused to host: 172.16.6.70; nested exception is: 
   	java.net.ConnectException: Connection refused (Connection refused)
   summary =      0 in 00:00:00 = ******/s Avg:     0 Min: 9223372036854775807 Max: -9223372036854775808 Err:     0 (0.00%)
   Tidying up remote @ Tue Jun 25 16:59:43 CST 2019 (1561453183672)
   Remote engines have been started
   Waiting for possible Shutdown/StopTestNow/HeapDump/ThreadDump message on port 4445
   summary =      0 in 00:00:00 = ******/s Avg:     0 Min: 9223372036854775807 Max: -9223372036854775808 Err:     0 (0.00%)
   Tidying up remote @ Tue Jun 25 16:59:44 CST 2019 (1561453184014)
   Error generating the report: java.lang.NullPointerException
   ... end of run
   Error generating the report: java.lang.NullPointerException
   ... end of run
   root@jmeter001:~/stress_testing/apache-jmeter-5.1.1/bin# 
   ```

7. 打包文件

   `zip -r aa.zip stress_result*`

8. windows 通过XShell连接，可直接使用`sz aa.zip` 下载文件

   linux 在本机终端 `scp root@39.96.206.141:/root/stress_testing/aa.zip /home/Jmeter/aa.zip`

9. 解压文件，进入文件夹，打开index.html文件，查看结果