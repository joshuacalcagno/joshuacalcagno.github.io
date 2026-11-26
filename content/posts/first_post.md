+++
date = '2026-10-16T16:30:47-03:00'
draft = true
title = 'MISP and Threat Intelligence Research'
+++

A few weeks ago, I had the opportunity to learn how MISP worked. bootcamp etc.

The lab allowed us to develop our own instance of MISP 2.5 in Oracle VirtualBox using Ubuntu 24.04 (I will explain later why this version specifically), with an intelligence event and a few OSINT feeds as to correlate IOCs properly. 
For this you need:
- Hypervisor of your choice
- Ubuntu 24.04 Virtual Machine
- MISP Github Script
- etc

(image)

**bold** and *italic*


## > Hypervisor

We were given the option to choose between VMWare Workstation Pro and Oracle Virtualbox as our hypervisor. Although VMWare Workstation Pro offers better I/O and memory performance for MariaDB, Redis and feed caching (which MISP heavily relies on), I ended up going with Virtualbox since I generally prefer open-source options and knew I only intended to do some lightweight testing; if you're looking to build a large TI lab and run MISP alongside Elasticsearch, or add more elements into the mix like Wazuh, Cortex, TheHive and the like, VMWare will handle a heavy workload more efficiently. Considering this project is a single Ubuntu VM running only MISP, Virtualbox is perfectly adequate (not having to create a new account is an added bonus!)

![Comparison graph made by yours truly](/images/screenshot.png)

After downloading Oracle Virtualbox from [Virtualbox](https://www.virtualbox.org/wiki/Downloads), as well as the `SHA256 CHECKSUMS` file from the same website, I compared the SHA-256 hash of the file with the one published by Oracle Corporation, since an altered .exe file is a classic vector for malware. You can get the file's hash using `PowerShell`:

```
Get-FileHash .\VirtualBox-7.2.20-175154-Win.exe -Algorithm SHA256
```

If the hash it returns is incongruent with the one on the Oracle Corporation website, **DO NOT RUN IT!** Delete the file and re-download it.


## > Virtual Machine

The next step was preparing the Ubuntu 24.04 VM from [osboxes.org].(https://www.osboxes.org/)

#### > Why Ubuntu 24.04?

Ubuntu 24.04 LTS (Noble Numbat, 64-bit) is the only version explicitly supported by the MISP 2.5 installation script. If it detects a different Ubuntu version, the script stops. This is because it relies on a specific set of tested dependencies and configurations to ensure a reliable installation. Even if versions 24.10 or 25.04 appear first on the download website, do not use them.

Go to [osboxes.org/ubuntu](https://www.osboxes.org/ubuntu/) and look for your hypervisor format. Verify the hash of the .7z on `Powershell` and the one published online by osboxes. Once you decompress the file, you will get an Ubuntu 24.04 (64bit).vdi file. 

(image)

By default, the credentials are:

```
user: osboxes
password: osboxes.org
```



#### > Creating the VM in VirtualBox

(image)

Machine -> New. Give it a name and specify the OS, the distribution and the version. You need a minimum of 4096 MB and 2 processors; the recommended RAM space is 8192 MB and 4 processors. Click "Finish" and open Settings -> Network. Adaptor 1: NAT

Copy the .vdi file you decompressed and put it inside your VM file. In Hard Disk, click "Use existing hard drive virtual file" and select it. 


## > Installing MISP

Now you can finally install MISP! Since the 2.4 version is in its security fix only, EOL phase, download 2.5 in its place. The script can be found in Github (read it before running it): (link)

(image: what the script installs)