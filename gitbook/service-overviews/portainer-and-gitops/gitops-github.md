# GitOps / GitHub

[Public GitHub Repo](https://github.com/trentnbauer/agg)

[Private GitHub Repo](https://github.com/trentnbauer/agg.local)

GitOps gives you tools and a framework to take DevOps practices, like collaboration, CI/CD, and version control, and apply them to infrastructure automation and application deployment. Developers can work in the code repositories they already know, while operations can put the other necessary pieces into place.

This app is hosted externally

<table><thead><tr><th width="232">Integration</th><th width="134">Repo</th><th>Purpose</th></tr></thead><tbody><tr><td>Portainer</td><td>N/A</td><td>Portainer reads data in GitHub, pulling compose files and containers</td></tr><tr><td>Renovate Bot</td><td>Private</td><td>A bot that watches for container updates in the compose files and creates a merge request to update them</td></tr><tr><td><a href="https://mergify.com/">Mergify bot</a></td><td>Public</td><td>Merges pull requests in the 'approved' state</td></tr><tr><td>Auto Approve action</td><td>Public</td><td>Auto approves pull requests created by me</td></tr><tr><td>Sync Files action</td><td>Private</td><td>Sync's files from the private repo to the public repo</td></tr></tbody></table>

\
