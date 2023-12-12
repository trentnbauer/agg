# Monitoring and Alerting

We are currently using Pushover and UptimeKuma for server & service monitoring

## Pushover

| **Alert Type** | **Notification**                      |
| -------------- | ------------------------------------- |
| -2             | No notification, displays in app      |
| -1             | No notification, displays in app      |
| 0              | Notification during active hours only |
| 1              | Always notify                         |
| 2              | Always notify                         |

If the service can integrate with Pushover;

* Configure 2 notifiers if possible
  * Low priority
  * High priority (if only 1 possible, configure high priority)
* Set up alerts for notifiers
  * Low priority can be used for
    * Services that will cause minimal impact
    * Low urgency alerts
    * Alerts that are good to have, but may be common
  * High priority can be used for
    * Services with public impact
    * Services that will cause an outage (eg DNS)
    * Outages

_Some services can integrate with Pushover via other methods, such as the text message API or SMTP. These should only be used for priority 0 and above._

## UptimeKuma

All UptimeKuma monitors should be set as below. This reduces spam to Pushover as well as resource load

| **Setting**              | **Value**                |
| ------------------------ | ------------------------ |
| Time in Seconds          | 120                      |
| Retry count before error | 3                        |
| Pushover                 | Priority based on impact |

* All servers should be pinged
* Services with public interfaces (such as MovieMatch) should have their internal and external access monitored

### Status Pages

We currently have 3 status pages,

* LatteMedia
* A Gamers Grind
* XFGN

### Maintenance Periods

Maintenance periods should be set before the potential service outage, with the relevant monitors selected

Consistent maintenance periods, like the 4:30PM daily update schedule, can be scheduled and automated in Kuma

## NetData

Why?

This is to allow us to keep ahead of issues in the network and resolve them before they affect any public facing services
