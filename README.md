# ansible_ad_hoc_useful

# Рестарт контейнеров
ansible -i <path>/environments/production/hosts <group_host_environment> -m shell -a 'docker stop production_environment.252.master.AG && sleep 2 && docker stop production_environment.252.master.SF && sleep 2 && docker ps'

# Удалить файлы старше 20 дней
ansible -i <path>/environments/production/hosts <group_host_environment> -m shell -a 'hostname && find /opt/logs/ -type f -mtime +20 -delete && sleep 2' --list-hosts
ansible -i <path>/environments/production/hosts <group_host_environment> -m shell -a 'hostname && find /opt/logs/ -type f -mtime +20 -delete && sleep 2'
ansible -i <path>/environments/production/hosts <group_host_environment> -m shell -a 'hostname && find /opt/logs/ -type f -mtime +20 -delete && sleep 2' --limit <hostname>

# Показать место на разделе
ansible -i <path>/environments/production/hosts <group_host_environment> -m shell -a 'hostname && df -Th | grep sda1 | grep ext4 && sleep 2'

# Показать логи компоуз
ansible -i <path>/environments/production/hosts <group_host_environment> -m shell -a '/bin/bash -c "hostname && cd /opt/production/ && sudo docker-compose logs --tail 2000 && sleep 2"' --limit=<hostname>


ansible -i <path>/environments/production/hosts <group_host_environment>  -m shell -a "docker ps --format {{ '{{.Names}}' }} | grep SF"
# OUTPUT
158.160.121.104 | CHANGED | rc=0 >>
production_environment.23.develop.SF


ansible -i <path>/environments/production/hosts <group_host_environment>  -m shell -a 'variableHOST=$(docker ps --format {%raw%}"{{.Names}}"{%endraw%} | grep SF) && echo "The container $variableHOST will be restarted" && echo "The run $variableHOST start restarted" && docker restart "$variableHOST" && echo "Check that the container $variableHOST has been restarted" && docker ps'
# OUTPUT
158.160.121.104 | CHANGED | rc=0 >>
The container production_environment.23.develop.SF will be restarted
The run production_environment.23.develop.SF start restarted
production_environment.23.develop.SF
Check that the container production_environment.23.develop.SF has been restarted
CONTAINER ID   IMAGE                   COMMAND                  CREATED          STATUS                  PORTS                               NAMES
60c1ac547768   nginx:1.27-alpine3.21   "/docker-entrypoint.…"   52 minutes ago   Up Less than a second   0.0.0.0:80->80/tcp, :::80->80/tcp   production_environment.23.develop.SF


ansible -i <path>/environments/production/hosts <group_host_environment>  -m shell -a 'variableHOST=$(docker ps --format {%raw%}"{{.Names}}"{%endraw%} | grep SF) && echo "$variableHOST" && docker ps --filter="name=$variableHOST"'
# OUTPUT
158.160.121.104 | CHANGED | rc=0 >>
production_environment.23.develop.SF
CONTAINER ID   IMAGE                   COMMAND                  CREATED          STATUS          PORTS                               NAMES
60c1ac547768   nginx:1.27-alpine3.21   "/docker-entrypoint.…"   46 minutes ago   Up 46 minutes   0.0.0.0:80->80/tcp, :::80->80/tcp   production_environment.23.develop.SF


ansible -i <path>/environments/production/hosts <group_host_environment>  -m shell -a 'variableHOST=$(docker ps --format {%raw%}"{{.Names}}"{%endraw%} | grep SF) ; echo "$variableHOST"'
# OUTPUT
158.160.121.104 | CHANGED | rc=0 >>
production_environment.23.develop.SF


ansible -i <path>/environments/production/hosts <group_host_environment>  -m shell -a "docker ps -a --format '{{ '{{' }} .Names {{ '}}' }}'"
# OUTPUT
158.160.121.104 | CHANGED | rc=0 >>
production_environment.23.develop.SF


ansible -i <path>/environments/production/hosts <group_host_environment>  -m shell -a 'docker ps -q --filter="name=production_environment.23.develop.SF"'
# OUTPUT
158.160.121.104 | CHANGED | rc=0 >>
60c1ac547768


ansible -i <path>/environments/production/hosts <group_host_environment>  -m shell -a 'docker ps --format {%raw%}"{{.Image}} {{.Names}}"{%endraw%}'
# OUTPUT
158.160.121.104 | CHANGED | rc=0 >>
nginx:1.27-alpine3.21 production_environment.23.develop.SF

ansible -i <path>/environments/production/hosts <group_host_environment>  -m shell -a "docker ps -a --format '{\"ID\": \"{{ '{{' }} .ID {{ '}}' }}\", \"Names\" : \"{{ '{{' }} .Names {{ '}}' }}}\"'"
# OUTPUT
158.160.121.104 | CHANGED | rc=0 >>
{"ID": "60c1ac547768", "Names" : "production_environment.23.develop.SF}"

ansible -i <path>/environments/production/hosts <group_host_environment>  -m shell -a "docker ps -a --format '{\"ID\": \"{{ '{{' }} .ID {{ '}}' }}\", \"Image\": \"{{ '{{' }} .Image {{ '}}' }}\", \"Names\" : \"{{ '{{' }} .Names {{ '}}' }}}\"'"
# OUTPUT
158.160.121.104 | CHANGED | rc=0 >>
{"ID": "60c1ac547768", "Image": "nginx:1.27-alpine3.21", "Names" : "production_environment.23.develop.SF}"
