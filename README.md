SynologyCloudFlareDDNS
========================

Purpose & Pros
---------------
* A script for Cloudflare DDNS on Synology DSM.
* A Minimum Settings required.
* Uses Cloudflare API v4.

Changelog
---------------
2022.02.14. Supports both "API Tokens" and "Global API Key"

Prerequisites
---------------
* Have a active Zone in Cloudflare. (Your own domain, too)
* Have a A Record.

Installation - Simple way (requires DSM 7.0+ or Python3 installed)
----------------
1. Open **Task Scheduler** (Control Panel - [Services] Task Scheduler)

2. Create a user-defined script item.
    Create - Triggered Task - User-defined script
```
[General Tab]
Task: Cloudflare DDNS (not important)
User: root
Event: Boot-up
Pre-task: none
Enabled: Checked
```
```
[Task Settings Tab]
[Run Command] User-defined script
    curl https://raw.githubusercontent.com/michaelochs/SynologyCloudflareDDNS/master/setddns.py | python3 -
```

3. Press OK

4. Right-Click on the task you've just created.

5. Click Run

6. You can see Cloudflare DDNS has been added to your DDNS list.

7. Setup DDNS in Synology DSM (You can use "API Tokens" or "Global API Key")
> a. Using API Tokens (Recommended)
>    > (1) Single Domain and single permission can granted with a Token -> more secure<br />
>    > (2) How to create: Cloudflare - My Profile - API Tokens - Create Token (Use "Edit zone DNS" template, required permission: Zone - DNS - Edit)<br />
>    > (3) Synology DDNS Settings
>    > ```
>    > Username: Anything you want(not using when authorize the token)
>    > Password: API Token (40 byte)
>    > ```
>    >    >![Use "Edit zone DNS" Template](https://user-images.githubusercontent.com/51356408/153817729-1b3663a8-b585-4ad6-b785-ba969f054fdc.png)
>    >    >![Check the Permission and Domain name](https://user-images.githubusercontent.com/51356408/153818045-5099b08d-413f-4e56-85b3-9e1540bd5699.png)
>
> b. Using Global API Key
>    > (1) All permission with a single API Key - less secure<br />
>    > (2) How to view: Cloudflare - My Profile - API Tokens - Global API Key - Click "View"<br />
>    > (3) Synology DDNS Settings
>    > ```
>    > Username: Cloudflare Username
>    > Password: Global API Key (37 byte)
>    > ```

Installation - Another way (DSM 7.0- or  Python3 NOT installed)
----------------
1. Connect via SSH. (can be activated in DSM)
2. Execute 
```
sudo curl https://raw.githubusercontent.com/michaelochs/SynologyCloudflareDDNS/master/cloudflare.php -o /usr/syno/bin/ddns/cloudflare.php && sudo chmod 755 /usr/syno/bin/ddns/cloudflare.php
```

3. Add some notes to end of DDNS config file. You can use your preferred text-editor. *(sudo vi /etc.defaults/ddns_provider.conf)*
```
[Cloudflare]
  modulepath=/usr/syno/bin/ddns/cloudflare.php
  queryurl=https://www.cloudflare.com/
```
4. Set up DDNS in DSM (Use your Cloudflare __Global API Key__(can be found in My Profile) as a password)
