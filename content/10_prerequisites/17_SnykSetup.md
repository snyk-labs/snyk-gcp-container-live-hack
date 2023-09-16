---
title: "Snyk CLI"
chapter: false
weight: 17
---

# Snyk Setup Instructions
You will need a Snyk account to run scans.  Snyk is available for free and all you need is a valid email address to register.  Once you register, you can perform scans and view results locally or on the website.

## Setting up your Snyk Account

### Create or Login to Snyk account
[Login or Create a free account here.](https://snyk.co/GKE-Live-Hack-Sept20)

### Create Snyk Access Token
- Visit your Snyk account (Account Settings > API Token section) (https://app.snyk.io/account)
- In the KEY field, select click to show, then select and copy your API token from the field
- Paste the token that appears on the screen in a safe location for use in future modules

{{% notice warning %}}
<p style='text-align: left;'>
Your Snyk access token must be protected and not shared with unauthorized parties to prevent exposure and unauthorized access.
</p>
{{% /notice %}}

You can read more about Snyk Access Token from their docs here.

## Setting up the Snyk CLI

The Snyk Command-Line-Interface (CLI) is highly portable and very popular with end users.  We’ll use the Snyk CLI in this workshop to collect and send results about your vulnerabilities.

Start by downloading the Snyk CLI to your environment.  In this workshop, we’ll prescribe steps to save time and you can find more details on the Snyk documentation site at:
https://docs.snyk.io/snyk-cli/install-the-snyk-cli


You will need to authenticate on the CLI with your API token.

### Browser based authentication
If you are working on your local machine, simply run `snyk auth` and a browser should automatically open prompting you to authenticate. Accept that (logging in, if needed) and your CLI should show the following:
```
Your account has been authenticated. Snyk is now ready to be used.
```

### Manual (non-browser based) authentication
If you are running remotely and the automatic authentication is not available you can manually authenticate by navigating to your Snyk Account (https://app.snyk.io/account), and get your API_TOKEN by clicking into your Account Settings -> API Token section.

In the KEY field, click your “click to show” box to copy your API token.

You can then run this command where API_TOKEN is the value you copied.

```
snyk auth API_TOKEN.
```

That should be it!  Your response should look like the following:

    snyk auth 12345678-abcd-efgh-1234head5678bead

    Your account has been authenticated. Snyk is now ready to be used.
