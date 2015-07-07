---
layout: default
---

<a id="introduction"></a>

# Dockerizing Linked Data <small>a tutorial page</small>

Popular knowledge bases that provide SPARQL endpoints for the web are usually experiencing a high number of requests, which often results in low availability of their interfaces.
A common approach to counter the availability issue is to run a local mirror of the knowledge base. Running a SPARQL endpoint is currently a complex task which requires a lot of effort and technical support for domain experts who just want to use the SPARQL interface.

With our approach of containerised knowledge base shipping we are introducing a simple to setup methodology for running a local mirror of an RDF knowledge base and SPARQL endpoint with interchangeable exploration components.
The flexibility of the presented approach further helps maintaining the publication infrastructure for dataset projects.
We are demonstrating and evaluating the presented methodology at the example of the dataset projects DBpedia, Catalogus Professorum Lipsiensium and Sächsisches Pfarrerbuch.


## Requirements

To run the presented environment with full functionality you need to have the following programs installed:

<dl class="dl-horizontal">
    <di>
        <dt>Docker</dt>
        <dd>An open platform for distributed applications for developers and sysadmins (<a href="https://docs.docker.com">Documentation</a>)</dd>
    </di>
    <di>
        <dt>Docker-Compose</dt>
        <dd>A tool for defining and running multi-container applications with Docker (<a href="https://docs.docker.com/compose/install/">Documentation</a>)</dd>
    </di>
    <di>
        <dt>docker-py</dt>
        <dd>An API client for docker written in Python (<a href="https://docker-py.readthedocs.org/en/latest/">Documentation</a>)</dd>
    </di>
    <di>
        <dt>git</dt>
        <dd>A free and open source distributed version control system (<a href="https://git-scm.com/doc">Documentation</a>)</dd>
    </di>
</dl>

<a id="bootstrap"></a>

## How To Use Bootstrap Script

To run your environment you need to download or clone the bootstrap git repository from [dockerizing-bootstrap@github](https://github.com/Dockerizing/dockerizing-bootstrap).

    git clone https://github.com/Dockerizing/dockerizing-bootstrap.git

You can then setup your environment through a YAML (.yml) config file, which will be read by the `dld.py` bootstrap script.
Just create a file named `dld.yml` and the script will look out for this file.

The following list explains all usable keywords in the config file:

<dl class="dl-horizontal">
    <di>
        <dt>datasets</dt>
        <dd>Defines the set of datasources to be preloaded and used.</dd>
    </di>
    <di>
        <dt>graphUri</dt>
        <dd>Specifies the base URI for the data set. Can only be used inside a <code>datasets</code> statement.</dd>
    </di>
    <di>
        <dt>location</dt>
        <dd>Points to file dump in the web. Can only be used inside a <code>datasets</code> statement.</dd>
    </di>
    <di>
        <dt>file</dt>
        <dd>Points to a local file dump. Can only be used inside a <code>datasets</code> statement.</dd>
    </di>
</dl>

<dl class="dl-horizontal">
    <di>
        <dt>setup</dt>
        <dd>Defines the different parts of your linked data environment.</dd>
    </di>
    <di>
        <dt>store</dt>
        <dd>Sets the triple store container you want to use. Can only be used inside a <code>setup</code> statement.
            <br />
            Example: <br />
            <code>aksw/dld-store-virtuoso7</code>
        </dd>
    </di>
    <di>
        <dt>backup</dt>
        <dd>Sets a backup container which backups your data regular. Can only be used inside a <code>setup</code> statement.</dd>
    </di>
    <di>
        <dt>present</dt>
        <dd>Sets the presentation container for your linked datasets. Can only be used inside a <code>setup</code> statement.<br />
            Example:<br />
            <code>aksw/dld-present-ontowiki</code>
        </dd>
    </di>
    <di>
        <dt>ontowiki</dt>
        <dd>A visual presentation layer of your knowledge base. Can only be used inside a <code>present</code> statement.</dd>
    </di>
    <di>
        <dt>pubby</dt>
        <dd>A visual presentation layer of your knowledge base. Can only be used inside a <code>present</code> statement.</dd>
    </di>
</dl>

The keys under `components` and `present` (e.g. `store`, `load`, `ontowiki`) will be part of the of the created docker containers. The corresponding values can be either just images namges or key-value mappings themselves. These sub-ordinated key-value pairs are copied unaltered as docker-compose settings for the created containers (see the [docker-compose YAML reference](https://docs.docker.com/compose/yml/).

The following shows an example of a config file. Keys marked in <mark class="yellow">Yellow</mark> are introduced and processed by the dld bootstrap architecture and keys marked in <mark class="blue">blue</mark> are docker-compose keys.</p>

<pre>
<mark class="yellow">datasets</mark>:
    <mark class="yellow">dbpedia-abstracts-39</mark>:
        <mark class="yellow">graphUri</mark>: "http://v39.dbpedia.org"
        <mark class="yellow">location</mark>: "http://downloads.dbpedia.org/3.9/en/long_abstracts_en.ttl.bz2"
    <mark class="yellow">dbpedia-abstracts-2014</mark>:
        <mark class="yellow">graphUri</mark>: "http://v2014.dbpedia.org"
        <mark class="yellow">location</mark>: "http://downloads.dbpedia.org/2014/en/long_abstracts_en.ttl.bz2"
    <mark class="yellow">dbpedia-2015</mark>:
        <mark class="yellow">location_list</mark>: "dbpedia-2015-selection.list"

<mark class="yellow">components</mark>:
    <mark class="yellow">store</mark>:
        <mark class="blue">image</mark>: aksw/dld-store-virtuoso7
        <mark class="blue">ports</mark>: ["8891:8890"]
        <mark class="blue">environment</mark>:
            PWDDBA: super-secret
    <mark class="yellow">load</mark>:
        <mark class="blue">image</mark>: aksw/dld-load-virtuosoload
    <mark class="yellow">present</mark>:
        <mark class="yellow">ontowiki</mark>:
            <mark class="blue">image</mark>: aksw/dld-present-ontowiki
            <mark class="blue">ports</mark>: ["8081:80"]

<mark class="yellow">settings</mark>:
    <mark class="yellow">default_graph</mark>: "http://dbpedia.org"
</pre>

<a id="container-list"></a>

## List of Containers usable for the linked data environment

<ul>
    <li>Virtuoso 7 Container <a href="https://github.com/Dockerizing/triplestore-virtuoso7">@github</a></li>
    <li>OntoWiki Container <a href="https://github.com/Dockerizing/OntoWiki">@github</a></li>
    <li>Pubby Container <a href="https://github.com/Dockerizing/present-pubby">@github</a></li>
    <li>Fuseki Container <a href="https://github.com/Dockerizing/fuseki-docker">@github</a></li>
    <li>Virtuoso Import Container <a href="https://github.com/Dockerizing/virtuoso-import-docker">@github</a></li>
    <li>Virtuoso Backup Container <a href="https://github.com/Dockerizing/virtuoso-backup-docker">@github</a></li>
    <li>SNORQL Container <a href="https://github.com/Dockerizing/snorql-docker">@github</a></li>
</ul>

<p>For all repositories see the <a href="https://github.com/dockerizing">Dockerizing</a> repository page.</p>
