# Ansible

[Link to App](https://ansible.xfgn.dev)

[Link to GitHub or Website](https://www.ansible-semaphore.com/)

Ansible is a suite of software tools that enables infrastructure as code. It is open-source and the suite includes software provisioning, configuration management, and application deployment functionality

Ansible Semaphore is a modern UI for Ansible. It lets you easily run Ansible playbooks, get notifications about fails, control access to deployment system.

Ansible Semaphore pulls its playbooks from GitHub, linked to the [private AGG repo](https://github.com/trentnbauer/agg.local/tree/main/ansible)

This app is hosted on Lungo as a docker stack

This stack is made up of 2 containers,

* MySQL Database
* Ansible Semaphore app

{% code title="docker-compose.yml" lineNumbers="true" %}
```yaml
---
volumes:
  inventory:
  authorized-keys:
  config:
  mysql:
  
services:
  mysql:
    image: mysql:8.0
    hostname: mysql
    volumes:
      - mysql:/var/lib/mysql
      - /etc/localtime:/etc/localtime:ro
      - /etc/timezone:/etc/timezone:ro
    environment:
      - MYSQL_RANDOM_ROOT_PASSWORD=yes
      - MYSQL_DATABASE=semaphore
      - MYSQL_USER=semaphore
      - MYSQL_PASSWORD=$DB_PW  # change!
    restart: unless-stopped
  semaphore:
    container_name: ansiblesemaphore
    image: semaphoreui/semaphore:v2.8.92
    ports:
      - $PORT:3000
    environment:
      - SEMAPHORE_DB_USER=semaphore
      - SEMAPHORE_DB_PASS=$DB_PW  # change!
      - SEMAPHORE_DB_HOST=mysql
      - SEMAPHORE_DB_PORT=3306
      - SEMAPHORE_DB_DIALECT=mysql
      - SEMAPHORE_DB=semaphore
      - SEMAPHORE_PLAYBOOK_PATH=/tmp/semaphore/
      - SEMAPHORE_ADMIN_PASSWORD=$ADMIN_UN  # change!
      - SEMAPHORE_ADMIN_NAME=$ADMIN_UN
      - SEMAPHORE_ADMIN_EMAIL=ADMIN_EMAIL
      - SEMAPHORE_ADMIN=$ADMIN_UN
      - SEMAPHORE_ACCESS_KEY_ENCRYPTION=$ENCRYPTIONKEY  # add to your access key encryption !
      - ANSIBLE_HOST_KEY_CHECKING=false  # (optional) change to true if you want to enable host key checking
    volumes:
      - inventory:/inventory:ro
      - authorized-keys:/authorized-keys:ro
      - config:/etc/semaphore:rw
      - /etc/localtime:/etc/localtime:ro
      - /etc/timezone:/etc/timezone:ro
    restart: unless-stopped
    depends_on:
      - mysql
```
{% endcode %}

### Instance 1

| Port | Purpose |
| ---- | ------- |
| 1234 | WebUI   |

| Host Volume              | Container Volume | Purpose             |
| ------------------------ | ---------------- | ------------------- |
| ansible\_mysql           | /var/lib/mysql   | database data       |
| ansible\_config          | /etc/semaphore   | configuration files |
| ansible\_inventory       | /inventory       |                     |
| ansible\_authorized-keys | /authorized-keys |                     |

| Integration | Purpose |
| ----------- | ------- |
|             |         |

\
