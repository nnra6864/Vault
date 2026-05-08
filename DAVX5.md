---
created: 2026-05-08 21:24
tags:
  - note
  - guide
  - dev/server
  - dev/hosting
aliases:
---
[DAVX5](https://www.davx5.com/) is an excellent tool for syncing contacts, calendars and tasks.
It's extremely easy to integrate with [[Nextcloud]].

## Setup

Install the app from your app store of choice.

### Nextcloud

1. Open the Nextcloud android app.
2. Go to Settings.
3. Select `Sync calendar and contacts`.
4. Follow instructions to connect DAVX5 to your Nextcloud server.

### Contacts

Due to [[Google]] being the biggest fucking pain in the ass ever seen, most Contacts apps don't support exporting to/importing from `.vcf` files.
To work around this, you can use [Fossify Contacts](https://www.fossify.org/apps/), or almost any other [[FOSS]] Contacts app.

#### Exporting the .vcf file

1. Go to your Google Account.
	1. Open the Google app.
	2. Click on your Profile.
	3. Click `Manage your Google Account`.
2. Click on `People & sharing`.
3. Click on `Contacts`.
4. Click on the export button *(looks like an arrow pointing up at the time of writing)*.
5. Select `vCard for Android or iOS`.
6. Click `Export`.

#### Importing the .vcf file

> [!NOTE]
> This assumes you are using the Fossify Contacts app.

1. Open Settings.
2. Click on `Import contacts from a .vcf file`.
3. Select the file your previously exported.
4. Set `Target contact source` to `Contacts (your@mail.com) #6`, or whatever your DAVX5 Contacts option is.
5. Click OK.

Everything should be set now and your contacts should appear in both the app and Nextcloud if you set it up correctly.