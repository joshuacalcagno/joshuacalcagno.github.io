+++
date = '2026-11-20T20:30:47-03:00'
draft = false
title = 'MISP and Threat Intelligence Research'
+++

A few weeks ago, I had the opportunity to learn how MISP worked. I set up a VM using Oracle VirtualBox and got to work using Ubuntu 24.04

The lab allowed us to develop our own instance of MISP 2.5 in VirtualBox using Ubuntu 24.04 (I will explain later why this version specifically), with an intelligence event and a couple (especificar numero) OSINT feeds as to correlate IOCs properly.
For this you need:
- Hypervisor of your choice
- Ubuntu 24.04 Virtual Machine
- MISP Github Script
- etc

(image)

Here is some `inline code`

A [link to a site](https://example.com).

(Your generated file may use +++ and TOML-style title = '...' lines instead of ---. Either is fine, just keep whatever format Hugo generated and write your text below the closing marker.)

A few Markdown basics you’ll use a lot:


**bold** and *italic*
Triple backticks around code, with the language after the opening ones (e.g. bash, python) for syntax highlighting
![alt text](/images/screenshot.png) for images, with the file stored in static/images/

## > Hypervisor

We were given the option to choose between vmware workstation pro and oracle virtualbox as our hypervisor. Although vmware workstation pro offers better I/O and memory performance for mariadb, redis and feed caching (which MISP heavily relies on), I ended up going with Virtualbox since I generally prefer open-source options and I knew I only intended to do some lightweight testing; if you're looking to build a large TI lab and run MISP alongside Elasticsearch, or add more elements into the mix like Wazuh, Cortex, TheHive and the like, vmware will handle a heavy workload more efficiently. Considering this project is a single Ubuntu VM running only MISP, virtualbox is perfectly adequate (not having to create a new account is an added bonus!)

(image - comparison graph made by yours truly)

After downloading Oracle Virtualbox from virtualbox.org/wiki/Downloads, as well as the sha256 checksums file from the same website, I compared the SHA-256 hash of the file with the one published by Oracle Corporation, since an altered .exe file is a classic vector for malware. You can get the file's hash using PowerShell:

```
Get-FileHash .\VirtualBox-7.2.20-175154-Win.exe -Algorithm SHA256
```

If the hash it returns is incongruent with the one on the Oracle Corporation website, DO NOT RUN IT! Delete the file and re-download it.


## > Virtual Machine

The next step was preparing the Ubuntu 24.04 VM from osboxes.org

(subtitle2)
### > Why Ubuntu 24.04?

Ubuntu 24.04 LTS (Noble Numbat, 64-bit) is the only version explicitly supported by the MISP 2.5 installation script. If it detects a different Ubuntu version, the script stops. This is because it relies on a specific set of tested dependencies and configurations to ensure a reliable installation. Even if versions 24.10 or 25.04 appear first on the download website, do not use them.

Go to osboxes.org/ubuntu and look for your hypervisor format. Verify the hash of the .7z on Powershell and the one published online by osboxes. Once you decompress the file, you will get an Ubuntu 24.04 (64bit).vdi file. 

(image)

By default, the credentials are:

user: osboxes
password: osboxes.org

### > Creating the VM in VirtualBox

(image)

Machine -> New. Give it a name and specify the OS, the distribution and the version. You need a minimum of 4096 MB and 2 processors; the recommended RAM space is 8192 MB and 4 processors. Click "Finish" and open Settings -> Network. Adaptor 1: NAT

Copy the .vdi file you decompressed and put it inside your VM file. In Hard Disk, click "Use existing hard drive virtual file" and select it. 


## > Installing MISP

Now you can finally install MISP! Since the 2.4 version is in its security fix only, EOL phase, download 2.5 in its place. The script can be found in Github (read it before running it): (link)

(image: what the script installs)