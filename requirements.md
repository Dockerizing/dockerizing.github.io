---
layout: doc
---

# Requirements

To run the presented infrastructure with full functionality you need to have the following programs installed:

<dl class="dl-horizontal">
<dt><a href="http://docs.docker.com/installation/">Docker Engine</a></dt>
<dd><a href="https://docs.docker.com">Docker</a> is an open platform for easy application deployment in well-defined environments for developers and operators.
http://docs.docker.com/linux/step_one/</dd>
<dt><a href="https://wiki.python.org/moin/BeginnersGuide/Download">Python 2.7</a></dt>
<dd>Some key Python libraries/tools utilized are:

<ul>
<li><a href="https://docs.docker.com/compose/">Docker Compose</a> (implying <a href="https://github.com/docker/docker-py/">docker-py</a>)</li>
<li><a href="http://pyyaml.org/wiki/PyYAML">PyYAML</a></li>
</ul>

<p><a href="https://pip.pypa.io/en/latest/installing.html">pip</a> allows you to get required Python dependencies installed and/or updated with the <code>pip install -Ur requirements.txt</code> one-liner.</p>
</dd>
<dt><a href="https://git-scm.com/book/en/v2/Getting-Started-Installing-Git">git</a></dt>
<dd>A free and open source distributed version control system.</dd>
</dl>

## Complete requirements installation
If you are lazy and just want to get things going, you can simply copy and paste the following commands to a ubuntu or debian Linux system.
If you'd like to know more about, what the things do, better keep reading.

Creat a new directory and change into it.
Just copy the following commands and paste them into the terminal:

    sudo apt-get update && \
    apt-get -y install docker.io && \
    curl -L https://github.com/docker/compose/releases/download/1.4.2/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose && \
    chmod +x /usr/local/bin/docker-compose && \
    apt-get -y install python-pip && \
    git clone https://github.com/Dockerizing/dockerizing-bootstrap && \
    cd dockerizing-bootstrap && \
    sudo pip install -Ur requirements.txt


## Install Docker

Docker supports an installation on many systems.
Exemplarily you can find the installation instructions for Debian Jessie 8.0 (64-bit) on this point.
If you use another system, please see the [Docker documentation](https://docs.docker.com/installation/).

The docker.io package can be found in the jessie-backports repository.
Make sure you enabled this Repository. To enable jessie-backports please see the [Debian Backports Instructions](http://backports.debian.org/Instructions/).

Open a terminal and execute the following commands (you don't need to copy the commends following the `#`):

    $ sudo apt-get update               # Updates the package list
    $ sudo apt-get install docker.io    # Install Docker
    $ sudo docker run --rm hello-world  # Verify docker works using a 'hello-world' Docker image and running it


## Install Docker Compose

To install Docker Compose, an installation of Docker (see the point above) is required. Make sure that your `/usr/local/bin` directory is writable.

Get Docker Compose according to the instructions at: https://github.com/docker/compose/releases and verify the Compose installation

    $ docker-compose --version

## Install Python

Install the latest Python package

    $ apt-get install python

The Python package repository pip is already included by default in the following Python versions:

* Python 2.7.9 and later versions on the python 2 series
* Python 3.4 and later versions

### Install dependencies using pip

On other Versions you have to install pip through the following terminal commands:

    $ sudo python get-pip.py

You can also use the OS Package Manager (for example on Debian) to install pip

    $ sudo apt-get install python-pip       # for python 2
    $ sudo apt-get install python3-pip      # for python 3

Now you should go to the [first page](/) again and try out our example.
