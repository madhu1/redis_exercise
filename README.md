Exercise details:
1.    Install standalone Open Source Redis server version 4.x or 5.x on Server A and configure it to run on port 10001
2.    Use memtier-benchmark tool created by RedisLabs, to load data into the standalone Redis.
3.    Install Redis Enterprise GA version on Server B.
4.    Setup Redis Enterprise using no DNS option.
5.    Create Redis DB on Redis Enterprise.
6.    Enable Replica Of between Redis Enterprise and the Open Source Redis. (Source: OpenSource Redis, Target: Redis Enterprise)
7.    Write a small script/routine in either Java, Python, Ruby, Go, Scala, C#, or JavaScript  to complete the following:
Insert values 1-100 into the Redis OSS server, and read and print them in a reverse order from the Redis Enterprise database.

# [1] Installation and Configuration of Single Instance
-------------------------------------------------------
Main step is to download redis installation tar file and extract it. THen set the following configuration. Refer: https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-redis-on-ubuntu-16-04
1. dir /var/lib/redis
2. changed port from default to 10001
3. Enable redis in the start <sudo systemctl enable redis>
4. To start/stop redis service <sudo systemctl status/start/stop redis>
5. simple test in the link to check commands on redis-cli  

Seen warning like :
WARNING overcommit_memory is set to 0! Background save may fail under low memory condition. To fix this issue add 'vm.overcommit_memory = 1' to /etc/sysctl.conf and then reboot or run the command 'sysctl vm.overcommit_memory=1' for this to take effect.
WARNING you have Transparent Huge Pages (THP) support enabled in your kernel. This will create latency and memory usage issues with Redis. To fix this issue run the command 'echo never > /sys/kernel/mm/transparent_hugepage/enabled' as root, and add it to your /etc/rc.local in order to retain the setting after a reboot. Redis must be restarted after THP is disabled.
1 Like the warning suggests, just add the line vm.overcommit_memory=1 to the bottom of /etc/sysctl.conf, with something like sudo vi /etc/sysctl.conf.
2. But permissions don't allow you to edit THP as the warning suggests, so instead do
sudo apt install hugepages and add the command sudo hugeadm --thp-never to the bottom of your .bashrc, with something like sudo vi ~/.bashrc.
  
 # [3] & [4] Installing Redis Enterprise GA On Machine 2, Create replica of Machine 1 redis DB on Machine 2
 Used the instruction from Redis Enterprise and follow them in the URL below to set up redis cluster
 https://docs.redislabs.com/latest/rs/getting-started/quick-setup/
1. Disabling Swap 
To disable the swap in the OS of an existing server/VM/instance, you must have sudo access or be root to run the following command:
$ sudo swapoff -a
$ sudo sed -i.bak '/ swap / s/^(.*)$/#1/g' /etc/fstab
2. Easily set up redis cluster through URL <https://Machine2_publicIP:8443/ https://54.209.240.172:8443/>, set 3 shards, no DNS  
3. In the set up, enabled Replica of  <redis://privateIP_source:PORT> <<redis://172.31.19.17:10001>>  
4. To access redis-cli -h 172.31.26.19 -p 19797

# [5] Insert into redis numbers 1-100 and return them in reverse order

1. Python code to connect and insert numbers into redis
2. Used list and used rpush and rpop to insert and retrieve the number 
3. After use, delete the redis list so that next time new list is created.




