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
1. dir /var/lib/redis
2. changed port from default to 10001
3. Enable redis in the start -- <sudo systemctl enable redis>
4. To start/stop redis service <sudo systemctl status/start/stop redis>
Seen warning like :
WARNING overcommit_memory is set to 0! Background save may fail under low memory condition. To fix this issue add 'vm.overcommit_memory = 1' to /etc/sysctl.conf and then reboot or run the command 'sysctl vm.overcommit_memory=1' for this to take effect.
WARNING you have Transparent Huge Pages (THP) support enabled in your kernel. This will create latency and memory usage issues with Redis. To fix this issue run the command 'echo never > /sys/kernel/mm/transparent_hugepage/enabled' as root, and add it to your /etc/rc.local in order to retain the setting after a reboot. Redis must be restarted after THP is disabled.
1 Like the warning suggests, just add the line vm.overcommit_memory=1 to the bottom of /etc/sysctl.conf, with something like sudo vi /etc/sysctl.conf.
2. But permissions don't allow you to edit THP as the warning suggests, so instead do
sudo apt install hugepages
and add the command sudo hugeadm --thp-never to the bottom of your .bashrc, with something like sudo vi ~/.bashrc.

# [2] Installation of Memtier BenchMark and Loading Redis
-----------------------------------------------------------
Cloned memtier benchmark from https://github.com/RedisLabs/memtier_benchmark.git and installed it using instruction. 
Missing dependency libssl was fixed using <sudo apt install --reinstall libssl1.1=1.1.0g-2ubuntu4> <sudo apt install libssl-dev>


