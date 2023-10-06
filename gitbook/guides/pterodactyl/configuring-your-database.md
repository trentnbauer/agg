# ❔ Configuring your Database



<table data-view="cards"><thead><tr><th></th><th></th></tr></thead><tbody><tr><td>Time Required</td><td>5 minutes</td></tr><tr><td>Difficulty</td><td>Low</td></tr></tbody></table>

## Create the Wings user

Firstly, we need to create the Wings user. This is unfortunately a manual step that needs to be done on the DB but its pretty straight forward

1. Log into Portainer and navigate to Stacks
2. Open the Pterodactyl Panel stack and take note of the MYSQL\_PASS\_ROOT variable
3. Scroll down and click on the database container
4. Click on Console, then Connect
5. Type the below command and input the Root password from step 2

<pre><code><strong>mysql -u root -p
</strong></code></pre>

6. Generate (or come up with) a new, secure, random password
7. Copy and paste the below text into Notepad, and change update the password field

```
CREATE USER 'test'@'127.0.0.1' IDENTIFIED BY 'PASSWORD';
GRANT ALL PRIVILEGES ON *.* TO 'test'@'%' WITH GRANT OPTION;
exit

```

8. Copy the edited text
9. Right click in the DB console window and select 'paste'
