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
2. setup a superuser
3. generate the pair of SSH keys and passphrase within the server itself, then
4. block root user access and
5. block password authentication to only rely on SSH authentication

## Server Access

The first step if using a Windows device is to install PuTTY, a SSH and telnet client that allows you to connect
securely to a particular physical or virtual server. The latest version can be downloaded
from [here](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html). Once it is installed, navigate to it as
shown below:

![accessing PuTTY](../images/putty-first-time1.jpg)

Once you open it, paste the IP address into the correct field; also ensure the port is set to `22` and
the connection type selected is `SSH` like so:

![PuTTY homepage](../images/putty-first-time3.jpg)

For the first login, you will get the following disclaimer:

![PuTTY homepage](../images/putty-first-time2.jpg)

The best thing to do is so choose the option `No` as selected above, so that you can do the entire configuration without
it being cached.

## Superuser creation

You will then be prompted for your username, which in this case is `root` and your password.
For security purposes, you will not see it even as you type it out, but once you press the `enter`
button, you will be successfully logged in as shown below:

![PuTTY successfull access](../images/putty-first-time4.jpg)

Once logged in, add the new superuser like so:

![new superuser](../images/putty-first-time5.jpg)

Then make them a superuser by changing their usergroup from the group they are currently
in (same as the username itself) to `sudo` group like so:

![new superuser](../images/putty-first-time6.jpg)

(For a deeper dive into configuring sudo users, you can read [this article](https://jumpcloud.com/blog/how-to-create-sudo-user-manage-sudo-access-ubuntu-22-04).)

We then test if this new user actually does have administrative rights like so:

![superuser sudo 1](../images/putty-first-time7.jpg)

As shown above, we can see that the user does have admin rights once sudo is put in front of the command and
they key in their password; this user can control administrative privileges of subsequent users.

The next step is to generate a pair of SSH keys for this user like so:

![superuser ssh 1](../images/putty-first-time8.jpg)

Also authenticate your newly generated key, then test ssh login like so:

![superuser ssh 2](../images/putty-first-time9.jpg)

## Restrict root access

To prevent any localized logins as root using a password, (as per [this article](https://www.howtogeek.com/828538/how-and-why-to-disable-root-login-over-ssh-on-linux/))
you use both the `-l` (lock) and `-d` (delete password) options, combined as `-ld` like so:

![root disabled 1](../images/putty-first-time12.jpg)

As you can now see below, users are no longer able to log into the server under `root`.

![root disabled 2](../images/putty-first-time13.jpg)

To prevent root login via ssh, as the superuser, navigate to the `sshd_config` file and set 
* `PasswordAuthentication` to `no`
* `PermitRootLogin` to `no`

![ssh config 1](../images/putty-first-time10.jpg)

![ssh config 2](../images/putty-first-time11.jpg)

![ssh config 3](../images/putty-first-time14.jpg)

Save your changes, then restart the server for the changes to take effect. If we test login,
after 3 attempts `root` will be locked out, as shown below:

![ssh reboot and test for root](../images/putty-first-time15.jpg)
