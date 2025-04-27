# Troubleshoting steps

1/ SSH into the server or using cloud console if you have problem with connection 

2/ Check available partitions and volume using below command:
df -h
lsblk
You can find which consume most space

3/ Clean oldest logs or unesed files in /tmp and /var/log to have enough storage to restore NGINX, avoiding product interruption.

4/ Check below possible root cause and solution to fix it


**Possible root case 1**
- NGINX access/error logs or syslogs overgrowth
Impact: 
- NGINX service could stop and interrupt your production
- Other services may not start properly or stop
Recovery and Preventative Steps:
- Remove the old log files
- Check logrotate config and force run after update
- Expand disk space 
- If your NGINX process too much transaction per day, you custom log directory and mount it to external File Storage

**Possible root case 2**
- Log rotate failed to run
Impact:
- NGINX log can not be rotated which cause log overgrowth
Recovery and Preventative Steps:
- Force run logrotate manunally
- Check and fix logrotate issue

**Possible root case 3**
- Could be attacked from outside
Impact:
- Attacker may scan you route or credentials which generates a lot of error log and transaction log
Recovery and Preventative Steps: 
- Setup fail2ban on your NGINX server to block malicious requests
- Setup security group to allow specific IP range
- Setup WAF (assume VM is running in AWS) 

**Possible root case 4**
- Temporary or cache files accumulate: /var/cache/, NGINX cache (if caching is enabled), System updates left temporary files
Impact:
Slow service, risk of full disk = total failure.
Recovery and Preventative Steps: 
- Clean cache data
- Setup cron job to auto clean cache


**In general, We need to setup monitor for all above issue**
