# NetData

[Link to App](https://netdata.xfgn.dev)

[Link to GitHub or Website](https://github.com/netdata/netdata)

View real-time metrics from your favorite operating systems, servers, containers, hardware, and applications running in a variety of environments, including on-premises, cloud and hybrid, with a continuously expanding selection of collectors, and send automated alarms about anomalous behavior or performance degradation to your favorite notification apps.

This container is deployed to all hosts via the 'all' Portainer Edge Stack, or installed manually on each host (such as Macaroni)

| Port | Purpose |
| ---- | ------- |
|      |         |

| Host Volume          | Container Volume     | Purpose |
| -------------------- | -------------------- | ------- |
| /etc/passwd          | /etc/passwd          |         |
| /etc/group           | /etc/group           |         |
| /var/run/docker.sock | /var/run/docker.sock |         |
| /etc/hostname        | /etc/hostname        |         |
| /proc                | /proc                |         |
| /etc/os-release      | /etc/os-release      |         |
| /sys                 | /sys                 |         |
| netdatalib           | /var/lib/netdata     |         |
| netdatacache         | /var/cache/netdata   |         |
| netdataconfig        | /etc/netdata         |         |

| Integration   | Purpose                                                               |
| ------------- | --------------------------------------------------------------------- |
| Netdata Cloud | Sends utlization, logs etc to Netdata cloud for monitoring & alerting |

\
