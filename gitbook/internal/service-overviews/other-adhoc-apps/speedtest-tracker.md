# Speedtest Tracker

[Link to App](https://stt.xfgn.dev/)

[Link to GitHub or Website](https://github.com/henrywhitaker3/Speedtest-Tracker)

This program runs a speedtest check every hour and graphs the results

This app is hosted on Espresso as a docker container

{% @github-files/github-code-block url="https://github.com/trentnbauer/agg/blob/main/docker-compose/speedtest-tracker.yml" %}

### Instance 1

| Port | Purpose |
| ---- | ------- |
| 1234 | webui   |

| Host Volume | Container Volume | Purpose               |
| ----------- | ---------------- | --------------------- |
| config      | config           | store the config file |

| Integration | Purpose |
| ----------- | ------- |
| N/A         |         |

\
