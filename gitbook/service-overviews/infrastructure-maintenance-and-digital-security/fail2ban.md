# Fail2Ban

[Link to GitHub or Website](https://github.com/fail2ban/fail2ban)

Fail2ban is an intrusion prevention software framework. Written in the Python programming language, it is designed to prevent against brute-force attacks. It is able to run on POSIX systems that have an interface to a packet-control system or firewall installed locally, such as iptables or TCP Wrapper.

This stack is part of the 'all' edge stack and is deployed to all machines

<pre class="language-yaml" data-title="docker-compose.yml" data-overflow="wrap"><code class="lang-yaml">version: '3'
services:
<strong>    fail2ban:
</strong>    image: ghcr.io/crazy-max/fail2ban:latest
    network_mode: "host"
    cap_add:
      - NET_ADMIN
      - NET_RAW
    volumes:
      - fail2ban:/data
      - "/var/log:/var/log:ro"
    environment:
      - TZ=Australia/Melbourne
      - F2B_LOG_TARGET=STDOUT
      - F2B_LOG_LEVEL=INFO
      - F2B_DB_PURGE_AGE=1d
      - SSMTP_HOST=smtp.gmail.com
      - SSMTP_PORT=587
      - SSMTP_HOSTNAME=gmail.com
      - SSMTP_USER=noreply@xfgn.dev
      - SSMTP_PASSWORD=sdgshvyrbhtqnkly
      - SSMTP_TLS=YES
    restart: always
    labels:
      - com.centurylinklabs.watchtower.enable=true
volumes:
  fail2ban:
</code></pre>

| Host Volume | Container Volume | Purpose                    |
| ----------- | ---------------- | -------------------------- |
| fail2ban    | /data            | data, f2b logs and configs |
| /var/log    | /var/log         | server logs                |

| Integration | Purpose |
| ----------- | ------- |
|             |         |

\
