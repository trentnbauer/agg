# Add Health Check to Container

The

```
healthcheck:
  test: curl --connect-timeout 15 --silent --show-error --fail http://localhost:$PORT
  interval: 1m
  timeout: 30s
  retries: 3
  start_period: 1m
```
