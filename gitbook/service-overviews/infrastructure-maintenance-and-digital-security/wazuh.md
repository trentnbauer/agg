# Wazuh

[Link to App](https://wazuh.xfgn.dev)

[Link to GitHub or Website](https://wazuh.com/)

Wazuh is an enterprise-ready platform used for security monitoring. It is a free and open-source platform that is used for threat detection, incident response and compliance, and integrity monitoring. Wazuh is capable of protecting workloads across virtualized, on-premises, containerized, and cloud-based environments.

### Wazuh Server

This app is hosted on Lungo as multiple docker containers

This stack is made up of 3 containers,

* Manager
* Indexer
* Dashboard

{% code title="docker-compose.yml" overflow="wrap" %}
```yaml
# Wazuh App Copyright (C) 2017, Wazuh Inc. (License GPLv2)
version: '3.7'

services:
  wazuh.manager:
    image: wazuh/wazuh-manager:4.5.1
    hostname: wazuh.manager
    restart: always
    ports:
      - "1514:1514"
      - "1515:1515"
      - "514:514/udp"
      - "55000:55000"
    environment:
      - INDEXER_URL=https://wazuh.indexer:9200
      - INDEXER_USERNAME=$ADMIN_UN
      - INDEXER_PASSWORD=$ADMIN_PW
      - FILEBEAT_SSL_VERIFICATION_MODE=full
      - SSL_CERTIFICATE_AUTHORITIES=/etc/ssl/root-ca.pem
      - SSL_CERTIFICATE=/etc/ssl/filebeat.pem
      - SSL_KEY=/etc/ssl/filebeat.key
      - API_USERNAME=$API_UN
      - API_PASSWORD=$API_PW
    volumes:
      - wazuh_api_configuration:/var/ossec/api/configuration
      - wazuh_etc:/var/ossec/etc
      - wazuh_logs:/var/ossec/logs
      - wazuh_queue:/var/ossec/queue
      - wazuh_var_multigroups:/var/ossec/var/multigroups
      - wazuh_integrations:/var/ossec/integrations
      - wazuh_active_response:/var/ossec/active-response/bin
      - wazuh_agentless:/var/ossec/agentless
      - wazuh_wodles:/var/ossec/wodles
      - filebeat_etc:/etc/filebeat
      - filebeat_var:/var/lib/filebeat
      - /root/wazuh-docker/single-node/config/wazuh_indexer_ssl_certs/root-ca-manager.pem:/etc/ssl/root-ca.pem
      - /root/wazuh-docker/single-node/config/wazuh_indexer_ssl_certs/wazuh.manager.pem:/etc/ssl/filebeat.pem
      - /root/wazuh-docker/single-node/config/wazuh_indexer_ssl_certs/wazuh.manager-key.pem:/etc/ssl/filebeat.key
      - /root/wazuh-docker/single-node/config/wazuh_cluster/wazuh_manager.conf:/wazuh-config-mount/etc/ossec.conf

  wazuh.indexer:
    image: wazuh/wazuh-indexer:4.5.1
    hostname: wazuh.indexer
    restart: always
    ports:
      - "9200:9200"
    environment:
      - "OPENSEARCH_JAVA_OPTS=-Xms512m -Xmx512m"
    ulimits:
      memlock:
        soft: -1
        hard: -1
      nofile:
        soft: 65536
        hard: 65536
    volumes:
      - wazuh-indexer-data:/var/lib/wazuh-indexer
      - /root/wazuh-docker/single-node/config/wazuh_indexer_ssl_certs/root-ca.pem:/usr/share/wazuh-indexer/certs/root-ca.pem
      - /root/wazuh-docker/single-node/config/wazuh_indexer_ssl_certs/wazuh.indexer-key.pem:/usr/share/wazuh-indexer/certs/wazuh.indexer.key
      - /root/wazuh-docker/single-node/config/wazuh_indexer_ssl_certs/wazuh.indexer.pem:/usr/share/wazuh-indexer/certs/wazuh.indexer.pem
      - /root/wazuh-docker/single-node/config/wazuh_indexer_ssl_certs/admin.pem:/usr/share/wazuh-indexer/certs/admin.pem
      - /root/wazuh-docker/single-node/config/wazuh_indexer_ssl_certs/admin-key.pem:/usr/share/wazuh-indexer/certs/admin-key.pem
      - /root/wazuh-docker/single-node/config/wazuh_indexer/wazuh.indexer.yml:/usr/share/wazuh-indexer/opensearch.yml
      - /root/wazuh-docker/single-node/config/wazuh_indexer/internal_users.yml:/usr/share/wazuh-indexer/opensearch-security/internal_users.yml

  wazuh.dashboard:
    image: wazuh/wazuh-dashboard:4.5.1
    hostname: wazuh.dashboard
    restart: always
    ports:
      - 443:5601
    environment:
      - INDEXER_USERNAME=$ADMIN_UN
      - INDEXER_PASSWORD=$ADMIN_PW
      - DASHBOARD_USERNAME=kibanaserver
      - DASHBOARD_PASSWORD=kibanaserver
      - API_USERNAME=$API_UN
      - API_PASSWORD=$API_PW
    volumes:
      - /root/wazuh-docker/single-node/config/wazuh_indexer_ssl_certs/wazuh.dashboard.pem:/usr/share/wazuh-dashboard/certs/wazuh-dashboard.pem
      - /root/wazuh-docker/single-node/config/wazuh_indexer_ssl_certs/wazuh.dashboard-key.pem:/usr/share/wazuh-dashboard/certs/wazuh-dashboard-key.pem
      - /root/wazuh-docker/single-node/config/wazuh_indexer_ssl_certs/root-ca.pem:/usr/share/wazuh-dashboard/certs/root-ca.pem
      - /root/wazuh-docker/single-node/config/wazuh_dashboard/opensearch_dashboards.yml:/usr/share/wazuh-dashboard/config/opensearch_dashboards.yml
      - /root/wazuh-docker/single-node/config/wazuh_dashboard/wazuh.yml:/usr/share/wazuh-dashboard/data/wazuh/config/wazuh.yml
    depends_on:
      - wazuh.indexer
    links:
      - wazuh.indexer:wazuh.indexer
      - wazuh.manager:wazuh.manager

volumes:
  wazuh_api_configuration:
  wazuh_etc:
  wazuh_logs:
  wazuh_queue:
  wazuh_var_multigroups:
  wazuh_integrations:
  wazuh_active_response:
  wazuh_agentless:
  wazuh_wodles:
  filebeat_etc:
  filebeat_var:
  wazuh-indexer-data:
  
```
{% endcode %}

| Port  | Purpose             |
| ----- | ------------------- |
| 1514  | Agent connection    |
| 1515  | Agent enrollment    |
| 514   | Syslog Collector    |
| 55000 | Server RESTful API  |
| 9200  | Indexer RESTful API |
| 5601  | Web UI              |

### Wazuh Agent

The agent is pushed via the 'All' Portainer edge stack

<pre class="language-yaml" data-title="docker-compose.yml" data-overflow="wrap"><code class="lang-yaml"><strong>version: '3'
</strong>services:
  wazuh-agent:
    image: kennyopennix/wazuh-agent:latest
    volumes:
      - /etc/os-release:/etc/os-release
      - /var/run/docker.sock:/var/run/docker.sock
      - /:/rootfs:ro
      - ossec:/var/ossec
    network_mode: host
    environment:
      - JOIN_MANAGER_MASTER_HOST=lungo.agg.local
      - JOIN_MANAGER_WORKER_HOST=lungo.agg.local
      - JOIN_MANAGER_PASSWORD=MyS3cr37P450r.*-
      - JOIN_MANAGER_USER=wazuh-wui
    restart: always
    labels:
      - com.centurylinklabs.watchtower.enable=true
volumes:
  ossec:
</code></pre>

## Volumes

### Manager

| Host Volume                                                                            | Container Volume                   | Purpose          |
| -------------------------------------------------------------------------------------- | ---------------------------------- | ---------------- |
| /root/wazuh-docker/single-node/config/wazuh\_indexer\_ssl\_certs/wazuh.manager.pem     | /etc/ssl/filebeat.pem              | Self Signed Cert |
| wazuh\_wazuh\_agentless                                                                | /var/ossec/agentless               |                  |
| /root/wazuh-docker/single-node/config/wazuh\_indexer\_ssl\_certs/wazuh.manager-key.pem | /etc/ssl/filebeat.key              | Self Signed Cert |
| /root/wazuh-docker/single-node/config/wazuh\_indexer\_ssl\_certs/root-ca-manager.pem   | /etc/ssl/root-ca.pem               | Self Signed Cert |
| wuzah\_wazuh\_api\_configuration                                                       | /var/ossec/api/configuration       | API config       |
| wuzah\_filebeat\_var                                                                   | /var/lib/filebeat                  |                  |
| wuzah\_wazuh\_logs                                                                     | /var/ossec/logs                    | Logs             |
| wuzah\_wazuh\_queue                                                                    | /var/ossec/queue                   |                  |
| wuzah\_wazuh\_var\_multigroups                                                         | /var/ossec/var/multigroups         |                  |
| /root/wazuh-docker/single-node/config/wazuh\_cluster/wazuh\_manager.conf               | /wazuh-config-mount/etc/ossec.conf | Server Config    |
| wuzah\_wazuh\_active\_response                                                         | /var/ossec/active-response/bin     |                  |
| wuzah\_wazuh\_etc                                                                      | /var/ossec/etc                     |                  |
| wuzah\_wazuh\_integrations                                                             | /var/ossec/integrations            |                  |

### Indexer

| Host Volume                                                                            | Container Volume                                                 | Purpose          |
| -------------------------------------------------------------------------------------- | ---------------------------------------------------------------- | ---------------- |
| /root/wazuh-docker/single-node/config/wazuh\_indexer/internal\_users.yml               | /usr/share/wazuh-indexer/opensearch-security/internal\_users.yml |                  |
| /root/wazuh-docker/single-node/config/wazuh\_indexer/wazuh.indexer.yml                 | /usr/share/wazuh-indexer/opensearch.yml                          |                  |
| wuzah\_wazuh-indexer-data                                                              | /var/lib/wazuh-indexer                                           | Indexer Data     |
| /root/wazuh-docker/single-node/config/wazuh\_indexer\_ssl\_certs/admin-key.pem         | /usr/share/wazuh-indexer/certs/admin-key.pem                     | Self Signed Cert |
| /root/wazuh-docker/single-node/config/wazuh\_indexer\_ssl\_certs/admin.pem             | /usr/share/wazuh-indexer/certs/admin.pem                         | Self Signed Cert |
| /root/wazuh-docker/single-node/config/wazuh\_indexer\_ssl\_certs/root-ca.pem           | /usr/share/wazuh-indexer/certs/root-ca.pem                       | Self Signed Cert |
| /root/wazuh-docker/single-node/config/wazuh\_indexer\_ssl\_certs/wazuh.indexer-key.pem | /usr/share/wazuh-indexer/certs/wazuh.indexer.key                 | Self Signed Cert |
| /root/wazuh-docker/single-node/config/wazuh\_indexer\_ssl\_certs/wazuh.indexer.pem     | /usr/share/wazuh-indexer/certs/wazuh.indexer.pem                 | Self Signed Cert |

### Dashboard

| Host Volume                                                                              | Container Volume                                             | Purpose          |
| ---------------------------------------------------------------------------------------- | ------------------------------------------------------------ | ---------------- |
| /root/wazuh-docker/single-node/config/wazuh\_dashboard/opensearch\_dashboards.yml        | /usr/share/wazuh-dashboard/config/opensearch\_dashboards.yml |                  |
| /root/wazuh-docker/single-node/config/wazuh\_dashboard/wazuh.yml                         | /usr/share/wazuh-dashboard/data/wazuh/config/wazuh.yml       |                  |
| /root/wazuh-docker/single-node/config/wazuh\_indexer\_ssl\_certs/root-ca.pem             | /usr/share/wazuh-dashboard/certs/root-ca.pem                 | Self Signed Cert |
| /root/wazuh-docker/single-node/config/wazuh\_indexer\_ssl\_certs/wazuh.dashboard-key.pem | /usr/share/wazuh-dashboard/certs/wazuh-dashboard-key.pem     | Self Signed Cert |
| /root/wazuh-docker/single-node/config/wazuh\_indexer\_ssl\_certs/wazuh.dashboard.pem     | /usr/share/wazuh-dashboard/certs/wazuh-dashboard.pem         | Self Signed Cert |

### Agent

| Host Volume          | Container Volume     | Purpose        |
| -------------------- | -------------------- | -------------- |
| edge\_all\_ossec     | /var/ossec           | Config         |
| /                    | /rootfs              | File system    |
| /etc/os-release      | /etc/os-release      | OS information |
| /var/run/docker.sock | /var/run/docker.sock | Monitor Docker |
