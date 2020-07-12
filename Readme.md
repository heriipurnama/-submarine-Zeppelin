#### apache_submarine-apache_zeppelin

1. Pull images from docker<br>
    ``` docker pull heripurnama/apache_submarine-apache_zeppelin:tagname ```
2. Running images <br>
    ``` docker run -it -h submarine-dev --name mini-submarine-heriipurnama --net=bridge --privileged -P --expose 9009 heripurnama/apache_submarine-apache_zeppelin:tagname /bin/bash ```
3. Goto io container <br>
    ``` docker container ls -a ``` <br>
    ```docker container start <id-container> ```<br>
    ```docker container attach <id-container> ```
3. Run zeplin <br>
    ``` cd /home/yarn/zeppelin-hp/bin ```<br>
    ```./zeppelin-daemon.sh start ```
3. Run mini-submarine image <br>
     In the container, use root user to bootstrap hdfs and yarn
    ``` /tmp/hadoop-config/bootstrap.sh ``` <br>
    Two commands to check if yarn and hdfs is running as expected <br>
    ``` yarn node -list -showDetails ``` <br>
        You should see info like this:
            Total Nodes:1
            Node-Id      Node-State	Node-Http-Address	Number-of-Running-Containers
    submarine-dev:35949         RUNNING	submarine-dev:8042                            0
    Detailed Node Information :
    Configured Resources : <memory:8192, vCores:16, nvidia.com/gpu: 1>
    Allocated Resources : <memory:0, vCores:0>
    Resource Utilization by Node : PMem:4144 MB, VMem:4189 MB, VCores:0.25308025
    Resource Utilization by Containers : PMem:0 MB, VMem:0 MB, VCores:0.0
    Node-Labels : <br>
    ``` hdfs dfs -ls /user ``` <br>
    drwxr-xr-x - yarn supergroup 0 2019-07-22 07:59 /user/yarn <br>
4. Run workbench server <br>
    ``` /tmp/hadoop-config/setup-mysql.sh ```
5. Start submarine server <br>
    ``` cd /opt/submarine-current/bin ``` <br>
    ``` ./submarine-daemon.sh start getMysqlJar ```
6. Check port on docker container <br>
   ``` netstat -nlp ``` <br> Like This<br>
   `` Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
tcp        0      0 127.0.0.1:3306          0.0.0.0:*               LISTEN      1120/mysqld     
tcp        0      0 0.0.0.0:42827           0.0.0.0:*               LISTEN      62/java         
tcp        0      0 0.0.0.0:8080            0.0.0.0:*               LISTEN      147/java        
tcp        0      0 0.0.0.0:9009            0.0.0.0:*               LISTEN      62/java   ``<br>
7. Open new terminal for check port to acces on Guest OS <br>
   ``` docker ls -a ``` <br>
   READ : <br>
   
   0.0.0.0:32860->9000/tcp <br>

   IP OS GUEST:port OS Guest -> port forwarding docker container <br>

   exp. 0.0.0.0:32860->9000/tcp <br>
   port 9000 on docker container can u acces on IP os GUEST on port 32860.



WEB UI
1. submarine 
    u : admin
    p : admin
2. zeppelin <br>
    u  = p       = Role <br>
admin = password1, admin <br>
user1 = password2, role1, role2 <br>
user2 = password3, role3 <br>
user3 = password4, role2 