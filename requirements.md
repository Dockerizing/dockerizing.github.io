---
layout: home
---

# Requirements

To run the presented infrastructure with full functionality you need to have the following programs installed:

{:.dl-horizontal}
[Docker Engine](http://docs.docker.com/installation/)
: [Docker](https://docs.docker.com) is an open platform for easy application deployment in well-defined environments for developers and operators.
http://docs.docker.com/linux/step_one/

[Python 2.7](https://wiki.python.org/moin/BeginnersGuide/Download)
: Some key Python libraries/tools utilized are:

    * [Docker Compose](https://docs.docker.com/compose/) (implying [docker-py](https://github.com/docker/docker-py/))
    * [PyYAML](http://pyyaml.org/wiki/PyYAML)

    [pip](https://pip.pypa.io/en/latest/installing.html) allows you to get required Python dependencies installed and/or updated with the `pip install -Ur requirements.txt` one-liner.

[git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
: A free and open source distributed version control system.

##Install Docker

<p>Docker supports an installation on many systems. Exemplarily you can find the installation instructions on Debian Jessie 8.0 (64-bit). If you use another system, please see the <a href="https://docs.docker.com/installation/" target="_blank">Docker documentation</a>.</p>
<p>The docker.io package can be found in the jessie-backports repository. Make shure you enabled this Repository. To enable jessie-backports please see the <a href="http://backports.debian.org/Instructions/" target="_blank">Debian Backports Instructions</a>.</p>
<p>Open a terminal and execute the following commands:</p>
Update the package list
<pre>
$ sudo apt-get update
</pre>
Install Docker
<pre>
$ sudo apt-get install docker.io
</pre>
Verify your installation by downloading a 'hello-world' Docker image and runnnig it in a 
Docker container
<pre>
$ sudo docker run --rm hello-world
</pre> 



##Install Docker Compose

To install Docker Compose, an installation of Docker (see the point above) is required. Make shure that your `/usr/local/bin` directory is writable.

<p>Open a terminal and execute the following commands:</p>
Get Docker Compose
<pre>
curl -L https://github.com/docker/compose/releases/download/1.3.2/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
</pre>
Make Docker Compose executable
<pre>
chmod +x /usr/local/bin/docker-compose
</pre>
Verify the Compose installation
<pre>
docker-compose --version
</pre>

##Install pip

Text
