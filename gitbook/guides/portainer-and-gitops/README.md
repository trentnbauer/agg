---
coverY: 0
---

# Portainer and GitOps



<table data-view="cards"><thead><tr><th></th><th></th></tr></thead><tbody><tr><td>Time Required</td><td>1 - 2 Hours</td></tr><tr><td>Difficulty</td><td>Low</td></tr></tbody></table>

_Come across an issue with this documentation? You can make an edit request yourself, using the 'Submit Change' button on the left, or report it on our_ [_Discord_](https://discord.agamersgrind.com)

## The Scenario

Our goal is to make use of GitOps (a variation of DevOps, using GitHub) to have a infrastructure-as-code set up, allowing us to easily manage and deploy changes with minimal touch and 1 source of truth.

Whilst Portainer is a GUI based application, it can read code from GitHub which allows it to automatically push changes based on our code.

The lifecycle of the container (starting, stopping and deleting) is still managed via the Portainer GUI.

### Prerequisites

* A server
* A GitHub account

### Recommended

* Multiple Servers
* Ubuntu Server for the OS
