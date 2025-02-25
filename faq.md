---
title: FAQ
nav_order: 9
layout: page
description: "Common errors and frequently asked questions"
---
# Frequently Asked Questions

## Account Limits
To ensure system stability, every account is subject to limits on the amount of CPU cores, memory and disk storage it can use. You can view these limits in the control panel under *Account > Account Limits*. To avoid abuse, new accounts start with lower limits. After you make the first top-up to your account, your limits will increase to 10 pods and 2 TB of storage. If you need more than that, please just send an email to [hello@pikapods.com](mailto:hello@pikapods.com).


## Sending emails
Some apps need to send emails to work properly. Currently PikaPods doesn't offer email sending services. Some popular and proven options are [Mailjet](https://www.mailjet.com/), [Mailgun](https://www.mailgun.com/) and [Postmark](https://postmarkapp.com/). If you already use Amazon AWS, there is also [SES](https://aws.amazon.com/ses/).

No matter which provider you use for emails, be sure to use a *submission port*, like port 465 or 587. To prevent spam from our system port 25 is blocked.


## Using custom domains
If you already pay for your own domain, you can map a sub- or root domain to your pod. So your pod will be reachable as e.g. `status.my-domain.com` or `my-domain.com`. This is done by adding a `CNAME` or `ALIAS` record and setting your pod's pikapod.net domain as target. We will also apply for a SSL certificate for this domain. So it can take a few minutes before the pod is reachable from the new domain. Steps to use a custom domain:

1. Open *Pod Settings* and then the *Domain* tab.
2. Enable *Custom Domain*. Take note of your pod's `xxx-yyy.pikapod.net` subdomain
3. Ensure no CAA records are set that may prevent us from applying for a SSL certificate. If you already use CAA, please also add `sectigo.com`, as mentioned [here](https://zerossl.com/help/troubleshoot/caa-records/) **and** `letsencrypt.org` as mentioned [here](https://letsencrypt.org/docs/caa/).
4. Then, depending on if you want to map a subdomain (e.g. `status.my-domain.com`) or root (apex) domain (e.g. `my-domain.com`):
   
   `CNAME` record for subdomains: `status.my-domain.com. 3600 IN CNAME xxx-yyy.pikapod.net`

   `ALIAS` record for root domains: `@ 3600 IN ALIAS xxx-yyy.pikapod.net`
5. Use a tool like [DNS Checker](https://dnschecker.org/) to ensure the record on your domains shows your `pikapod.net` domain or the same IP.
6. Finally, add the new domain on PikaPod's control panel. If the setup is correct, it will be verified and saved.

After adding a domain, we will periodically check if DNS is still set up correctly. If there is any issue you will receive a notification.

Only **one custom domain** can be mapped per pod. If you need additional alias domains, you will need to set up redirects with your DNS provider or a service like [Freedirector](https://freedirector.io/lite) (free, no signup needed) or [redirect.pizza](https://redirect.pizza/) (free tier).

## Accessing pod files using SFTP
SFTP is a way to securely access remote files. It's similar to FTP, but uses encryption. To access your pod's files, first enable SFTP in the pod's settings on the **Pod Settings > SFTP** tab. This will display the hostname, username and password to use.

Then use any SFTP client to connect to the displayed hostname. Some popular SFTP clients:

- [WinSCP](https://winscp.net/eng/index.php): free, Windows
- [Cyberduck](https://cyberduck.io/): free, macOS
- [Rclone](https://rclone.org/sftp/): free, Unix command line, good for larger transfers
- [OpenSSH SFTP](https://man.openbsd.org/sftp): free, Unix command line, included with OpenSSH

To connect to a pod, you need the following details, which will be displayed in the *SFTP* tab of your pod:

- Hostname: This is the `.pikapod.net` domain of your pod. E.g. `eager-beaver.pikapod.net`
- Port: This is always 22, which is the default port for SFTP
- Username: Usually something like `p9999`
- Password: A randomly generated password

The first time you may be prompted to verify the server signature. So you can be certain to connect to the correct server in the future.

Also note that you can only *write* to subfolders, like `data` or `music` that you may see. You can **not** add files in the top folder, since those wouldn't be accessible in your pod.

Files starting with a dot (like `.config`) may be hidden in your SFTP client. If you don't see any files, try changing the setting to show hidden files.

## Accessing the pod's database
Advanced users can directly access the database to make small edits or import/export data. To access the database, first *Enable Database Access* in the pod's settings. This will display a login link and password.

**For improved security, please be sure to keep this feature disabled when not using it.**


## Adding new apps
New apps are regularly being added. If your favorite app isn't available yet, you can suggest or vote for it on our [feedback](https://feedback.pikapods.com/) page. For an app to be suitable for PikaPods, it should roughly match those criteria:

- Be a web application and use one HTTPS port only
- Not use large amounts of CPU or bandwith by design (e.g. video encoding)
- Not have a potential for abuse or impact on other pods (e.g. proxies or VPNs)
- Have a license that allows self-hosting
- Not compete with the author's own paid hosting service, unless alternative hosting options are actively promoted.
- Have an official (or semi-official) Docker image available
- Be actively developed and security issues are adressed


## Updating apps
Providing you with a stable experience is our main priority. As such we don't just automatically apply updates as they are released, but test them in a staging environment before making them available to all pods. Updates should also be for a stable release (in the context of the project) and released for a minimum of 3 days. Generally updates will reach your pod within 1-2 weeks of their release.

For security-related updates, we may do ad-hoc updates of single apps.


## Missing environment variables
When adding new apps, we will add the most common and useful env vars from the start. Sometimes this doesn't include very specific or newly-added env vars. If you find that an env var you want to use is missing, just send us an email to to [hello@pikapods.com](mailto:hello@pikapods.com) and we will add it right away.


## Backup of pod data
Keeping your data safe is very important to us. In addition to redundant drives, there are also daily backups of all databases and mounted files (everything you see over SFTP) pods use. These backups are meant to recover from server-wide failures. While it's also possible to restore individual pod files in emergencies, we currently can't offer this service regularly.

All persistent pod data is available over [SFTP](#accessing-pod-files-using-sftp). Some pods also use an external database, usually Postgresql or MySQL/MariaDB.

You can make a full pod backup by following the steps:

1. Stop the pod to make sure all data from memory is written to disk (e.g. for SQLite or Mongo databases)
2. Enable [SFTP](#accessing-pod-files-using-sftp) in the pod's settings.
3. Log into the pod via SFTP and copy all files.
4. Enable [Database access](#accessing-the-pods-database) in the pod's settings.
5. Log into the pod's database and *Export* the whole database.
6. Start your pod again

We plan on adding more granular backup- and restore options in the future. See [here](https://feedback.pikapods.com/posts/14/offer-backup-option) for the latest progress.


## Debugging a Pod
If a pod shows "Pod is loading..." for more than a few minutes, there is usually an issue with starting it. This could be an issue with settings or permissions. In most cases the detailed error can be found in the pod logs. To view them, just click the *Show Logs* button and look for obvious errors. If the error is unclear or log is empty, you can also use *Report Issue*, include the logs you got and we'll investigate further.

<img src="/img/pod-show-logs.jpg" width="60%" />


## IPv6 Support
We fully support IPv6 as of May 2023. Before this, IPv6 worked over TCP, but possibly not ICMP. If your pod was created before May 2023, you may need to recreate it. This will happen automatically during updates or when you change resources or env vars.


## Support for individual apps
If you have any concerns with your PikaPods account, billing or with running apps, email to [hello@pikapods.com](mailto:hello@pikapods.com) any time. You will generally receive a response within a few hours on working days.

For well-defined, one-off setup tasks (like setting up email), we can sometimes help, if your account has seen a minimum of $50 in top-ups.

Unfortunately we don't have the resources to help deal with specific app issues. You may find help in a community forum or similar for that app. Please don't file a Github issue, unless it is a genuine bug report. This is to avoid overloading the app author with support requests.


### Have any other questions? [Email Us!](mailto:hello@pikapods.com)
{: .no_toc }