## Synopsis

If you’re like me, staying on top of server logs is near impossible when you’re administrating more than one website. Even if I had the time, I don’t have the screen real estate to tail all of my server logs.

But I always have time for Slack. It’s on my phone, my computer, and my mind half the day. It makes it easy to communicate when and where you want to.

Using Fail2Ban, we can receive Slack notifications when a jail executes a ban or unban action. When the action is trigger, a notification will be sent to the slack channel of your choice with the corresponding jail name and offending IP.

## Requirements

Slack
Fail2Ban
CURL


## Installation

## **Step 1.&nbsp;**

**[Generate an Incoming WebHook API Token for Slack:](https://my.slack.com/services/new/incoming-webhook/)**

The first thing you will need is an [API token](https://my.slack.com/services/new/incoming-webhook/) that will allow us to issue commands to the Slack REST API. Using an Incoming Webhook, we can send message to the channel of your choice.

## **Step 2.&nbsp;**

**Create a new ban action for Fail2Ban**

With root, use your favorite editor to create the following file:

**/etc/fail2ban/action.d/slack-notify.conf**


Replace&nbsp;**YOUR_SLACK_API_TOKEN_GOES_HERE**&nbsp;with the API token you created with the Incoming hook. And where it says&nbsp;“**general**,” that’s the channel name (without the pound sign).

Save the file. Now it’s time to add this action to one of our jails.

## **Step 3.&nbsp;**

**Apply the action to your jail(s)**

For this demonstration we are going to be using the SSH jail. If you haven’t already, create a jail.local file for Fail2Ban in case a package update overwrite the default configuration:

`sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local`

Now let’s open&nbsp;**/etc/fail2ban/jail.local **and add the Slack notification action.

```
[ssh]

enabled  = true
port     = ssh
filter   = sshd
logpath  = /var/log/auth.log
maxretry = 6
banaction = iptables-multiport
            **slack-notify**
```


The&nbsp;“ssh” configuration block will most likely use the default banaction, which means the property won’t be listed. Add the banaction line, using&nbsp;“slack-notify” as the second command. Save and close the file.

**Now restart the Fail2Ban service and you should see your jails starting up:**

_Fail2Ban (ssh) jail has started_


## License

Use it and abuse it, just don't lose it.
