# Access Groups & Authentication

<table data-view="cards"><thead><tr><th></th><th></th></tr></thead><tbody><tr><td>Time Required</td><td>15 Minutes</td></tr><tr><td>Difficulty</td><td>Low</td></tr></tbody></table>

## Access Groups

Access groups allow you to add authentication policies against all or part of your site. When a user browses to the address, Cloudflare prompts  them to complete an Authentication challenge which they must complete to continue. These challenges are managed via Access Groups

In this example, we're going to create 2 groups. One to only allow people in Australia and another to specific users

### Creation of Groups

1. Log into Cloudflare and click on Zero Trust
2. Navigate to Access > Access Groups
3. Click on 'Add a group'
   1. Group name: User Authentication
   2. Under 'Group configuration'
      1. Change the include 'Selector' to 'Emails' and type your email/s in the right hand field
   3. Click on 'Save'
4. Click on 'Add a group'
   1. Group name: Australia Only
   2. Under 'Group configuration'
      1. Change the include 'Selector' to 'Country' and select 'Australia'
      2. Click on "+Add require"
      3. Change the include 'Selector' to 'Country' and select 'Australia'
   3. Click on Save

### Additional Authentication

The default challenge for the 'User Authentication' is a 2FA code sent via email. We can add additional authentication providers, such as GitHub, Google and Facebook

1. Log into Cloudflare and click on Zero Trust
2. Click on 'Settings', then select 'Authentication'
3. Under login methods, click on 'add new'
4. Follow the steps to configure the relevant auth providers you want to use

#### How does this work?

The 'Emails' selector, used in the 'User Authentication' access group is also compared against the email address assigned to your authentication providers. For example, if we allowed 'test@email.com' in the rule, that would allow the GitHub account with 'test@email.com' as the email address.

