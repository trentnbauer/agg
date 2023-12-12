# CrowdSec

[Link to App](https://app.crowdsec.net/)

CrowdSec Security Engine, the open-source intrusion prevention system written in Go, protects against attacks on any server by parsing real-time service logs (servers, SSH, WordPress etc. logs) by detecting malicious behaviors.

This app is hosted externally, with a client installed on each VM which is pushed out via [Ansible](ansible.md)

<table><thead><tr><th width="223.33333333333331">Integration</th><th width="152">Host</th><th>Purpose</th></tr></thead><tbody><tr><td>NFTables</td><td>Any server with a port  forward</td><td>Add malicious IPs to firewall blocklists</td></tr><tr><td>Cloudflare</td><td>Lungo, Cola, Mocha, Latte</td><td>Provide malicious IP's with Captcha requests before accessing agamersgrind.com, agamersgrind.dev, xfgn.dev and lattemedia.tv</td></tr></tbody></table>

\
