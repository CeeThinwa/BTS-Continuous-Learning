(db-setup)=
# Database Setup Notes

## NoSQL Database

The database of choice that I decided to adopt in this category was *ElasticSearch*.

Based on [the page where one could get started](https://www.elastic.co/webinars/getting-started-elasticsearch?baymax=default&elektra=docs&storm=top-video),
I went to their [download page](https://www.elastic.co/downloads/elasticsearch).

### Local *ElasticSearch* setup
I decided to first set it up locally on my Windows.

Elasticsearch runs within a Docker container, so I went [here](https://www.docker.com/products/docker-desktop/)
to download Docker Desktop.

If you directly attempt to install Docker Desktop, you get the following error:

![Docker error](../images/docker-error.jpg)

As per the guidance in [this article](https://stackoverflow.com/questions/71095210/installing-docker-desktop-4-5-0-failed-componet-communityinstaller-enablefeatur)
I managed the Windows Management Instrumentation (WMI) by entering my Command Prompt and running the commands below:

![WMI commands](../images/WMI-commands.jpg)

