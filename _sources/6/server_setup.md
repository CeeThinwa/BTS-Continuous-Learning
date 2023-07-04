(VM-setup)=
# Server Setup Notes

My first task was to identify a suitable server in terms of compute and storage space that worked for the project that I had in mind
* For my first project, a tiny server was enough
* For an ElasticSearch database, I worked with (as per this [recommendation](https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-elasticsearch-on-ubuntu-20-04)) a slightly larger Ubuntu 22.10 server with 4GB RAM, 2 CPUs and regular disk type

During setup, I learned the hard way after getting the `PuTTY Fatal Error` error `No supported authentication methods available (server sent: publickey)` (in spite of following [this](https://www.bingehacking.net/2022/01/putty-no-supported-authentication.html)
and [this](https://docs.digitalocean.com/products/droplets/how-to/connect-with-ssh/putty/)
and [this](https://docs.digitalocean.com/products/droplets/how-to/add-ssh-keys/create-with-putty/))
 **setup the server using a password for root user**!

In as much as a SSH key is safer, it is simpler to
1. setup using a password,
2. generate the pair of SSH keys and passphrase within the server itself, then
3. block password authentication to only rely on SSH authentication.
