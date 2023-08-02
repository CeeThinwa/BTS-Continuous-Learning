(db-setup)=
# Database Setup Notes

## NoSQL Database

The database of choice that I decided to adopt in this category was *ElasticSearch*.

Based on [the page where one could get started](https://www.elastic.co/webinars/getting-started-elasticsearch),
I went to their [download page](https://www.elastic.co/downloads/elasticsearch).

### Local *ElasticSearch* setup

I decided to first set it up locally on my Windows.

#### <u>Docker setup on Windows: my experience</u>

Elasticsearch runs within a Docker container, so I went [here](https://www.docker.com/products/docker-desktop/)
to download Docker Desktop.

If you directly attempt to install Docker Desktop, you get the following error:

![Docker error](../images/docker-error.jpg)

As per the guidance in [this article](https://stackoverflow.com/questions/71095210/installing-docker-desktop-4-5-0-failed-componet-communityinstaller-enablefeatur)
I managed the Windows Management Instrumentation (WMI) by entering my Command Prompt and running the commands below:

![WMI commands 1](../images/WMI-commands1.jpg)

![WMI commands 2](../images/WMI-commands2.jpg)

I then rebooted and voila!

![Docker success](../images/docker-success.jpg)

However when I started Docker, I got the following notice:

![Docker error 2](../images/docker-home.jpg)

So as per the advice in these 2 articles ([here](https://stackoverflow.com/questions/39684974/docker-for-windows-error-hardware-assisted-virtualization-and-data-execution-p) and
[here](https://stackoverflow.com/questions/56141254/enabling-hyper-v-in-bios-is-required-for-docker-to-work)),
I ran the following commands in Windows Powershell as an admin:

![Admin Powershell Output 1](../images/Hyper-V-1.jpg)

However, due to the error shown, I had to navigate to control panel "Turn Windows Features on or off"
and select the checkbox for the feature pointed at (as you can see, it was off):

![Turn Windows Features On 1](../images/Hyper-V-2.jpg)

![Turn Windows Features On 2](../images/Hyper-V-3.jpg)

Once I checked it and the update finished installation, I got the following prompt to restart my
machine:

![Turn Windows Features On 3](../images/Hyper-V-4.jpg)

To turn on Virtualization in the BIOS menu, as per [this article](https://www.thewindowsclub.com/disable-hardware-virtualization-in-windows-10),
when restarting the computer, press `shift` and `F10` until you access the Windows blue screen where you:
1. click the `Troubleshoot` tile, then
2. select `Advanced Options`,
3. and select `UEFI Firmware Settings`;
4. click the `Restart` option afterward and
5. once the black screen appears click `F10` to enter the BIOS menu.

Once in the BIOS menu, navigate to the `Virtualization` option and select `Enable`.

When opening Docker again, I got a new error:

![Docker error 2](../images/docker-home-2.jpg)

When I went to [this link](https://learn.microsoft.com/en-us/windows/wsl/install-manual#step-4---download-the-linux-kernel-update-package), I ran the
`wsl.exe --update` command in Powershell as an admin like so:

![Docker error 2](../images/wsl-update.jpg)

Voila! The docker engine can now run.

![Docker success 2](../images/docker-success-2.jpg)

To configure memory to 4 GB as per the ElasticSearch README file in the installation package,
I followed [this resource](https://learn.microsoft.com/en-us/windows/wsl/wsl-config#configure-global-options-with-wslconfig),
[this resource](https://superuser.com/questions/1765370/cannot-locate-wslconfig-in-user-profile-on-windows-11) and
[this resource](https://github.com/MicrosoftDocs/wsl/blob/main/WSL/wsl-config.md)
to create a `.wslconfig` file.

I then ran the following commands:

![WSL configuration 1](../images/wsl-configuration-1.jpg)

This caused the following prompt to appear:

![WSL configuration 2](../images/wsl-configuration-2.jpg)

Once I clicked `Restart`, the WSL debugger and Docker opened and ran the updates from the `.wslconfig` file like so: 

![WSL configuration 3](../images/wsl-configuration-3.jpg)

:::{admonition} Warning:
:class: warning
Update: When I attempted to open docker in a new session after restarting and effecting the
configuration settings, I was unable to start Docker (both normally and as an admin). I was
forced to do a Factory Reset to get Docker up and running once more.
:::


#### <u>ElasticSearch Setup</u>

In the virtual environment of my project, as per the ElasticSearch README file in the installation package I ran
the following commands:

```
docker network create elastic
docker pull docker.elastic.co/elasticsearch/elasticsearch:8.8.2
```

This enabled me to install ElasticSearch successfully, as per the status below:

![Elasticsearch success](../images/elasticsearch-success.jpg)

To run the Elasticsearch database, the following command was run:

```
docker run --name elasticsearch --net elastic -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" -t docker.elastic.co/elasticsearch/elasticsearch:8.8.2
```


