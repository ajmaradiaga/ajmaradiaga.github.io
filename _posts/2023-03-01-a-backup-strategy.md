---
layout: post
title: A backup strategy
---

*In this short blog post, I'll share what I've been doing to backup my data and hopefully keep it safe in case of any disasters.*

![Clear and Black Cassette Tape on Brown Wooden Surface Photo by Anthony : ) from Pexels: https://www.pexels.com/photo/clear-and-black-cassette-tape-on-brown-wooden-surface-170290/](../images/pexels-anthony-170290.jpg)
<p align = "center">
<i>Clear and Black Cassette Tape on Brown Wooden Surface Photo by <a href="https://www.pexels.com/photo/clear-and-black-cassette-tape-on-brown-wooden-surface-170290/">Anthony : ) from Pexels</a></i>
</p>

I've been self-hosting many services that I use daily for quite some time now. I use [Unraid](https://unraid.com) to manage my services. Some of these services have become quite critical to my "workflows". Now, one thing is running some services on a server and occasionally using them; another is that your workflows depend on them. If your day-to-day depends on them, that data has a particular value. Meaning losing the data can have huge implications.

Sometimes, you don't even own the data that forms part of your workflows, e.g. content included in web pages - bookmarks. For far too long, I've been relying on external services to maintain the web page/content available online for as long as I needed it. However, we all know how often content will become unavailable without notice. Therefore, I also keep a backup of this type of online data.... in simple terms, it is an archive of my bookmarks (not just the link but an offline copy of the contents).


My "keeping backups" journey hasn't been straightforward. I've not had a fantastic strategy for longer than I would like to admit. Generally, I did manual backups, some scripts here and there, and some data didn't even have backups. All those backups lived in a separate hard drive from my server, meaning I would have a copy of that data if my server died. As all the process was manual, I would lose data as the backup wasn't running as often as it should :-). Apart from that, I didn't have an offsite backup of the data.... not good.

In my case, I have to backup configurations, different files, and databases used by the services. For this, I leverage various tools/applications available in Unraid.
- [Appdata backup/restore v3](https://forums.unraid.net/topic/132721-plugin-ca-appdata-backup-restore-v3/): This is a backup plugin available which I use to backup the USB flash drive used for Unraid, as well as some of the files within the appdata folder in my server (think of it as a *Program Files* folder in the Windows environment).
- [tiredofit/db-backup](https://hub.docker.com/r/tiredofit/db-backup/): Various services I run, store their data in a SQL or NoSQL database. This [application](https://unraid.net/community/apps/c/backup?q=db+backup#r) helps me backup, on a scheduled basis, the databases I run.
- [Duplicacy]([Duplicacy](https://duplicacy.com/)): *Duplicacy backs up your files to many cloud storages with client-side encryption and the highest level of deduplication - Source: duplicacy.com*. Given that there is a very active [Unraid community](https://unraid.net/community), someone had already created a [community app for Duplicacy](https://unraid.net/community/apps/c/backup?q=duplicacy#r). I installed it on my Unraid server, configured a Cloud storage service ([Wasabi](https://wasabi.com/)), and set up a schedule to backup some of the local data to the cloud storage service for different backups. It is also possible to configure how often you want to prune/delete old backups, e.g. delete backup older than one month. It is incredible how simple it was to set up my backups to a cloud service properly.

I'm happy with how the backups are running at the moment. It is something that has been worrying me for quite some time, given the criticality of the data, but I now know that it is safely encrypted and replicated to a different location. To test my backups, I restored some data from a snapshot created by Duplicacy, and it all magically works. Happy times!
