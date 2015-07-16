---
layout: home
---

# How to Use the Bootstrap Script

You can configure your environment using a YAML (.yml) config file, which will be read by the `dld.py` bootstrap script.
Just create a file named `dld.yml` and the script will look out for this file. The script behaviour can also be adjusted with
some command line switches:

`-c` or `--config`
: specifies to load the [configuraion YAML](#config) from another file path

`-f` or `--file`
: specifies on-the-fly a local dump file to be imported (equivalent to a `datasets` entry with a `file` key)

`-l` or `--location`
: specifies on-the-fly a dump file on the web to be downloaded and imported (equivalent to a `datasets` entry with a `file` key)

`-u` or `--uri`
: specifies on-the-fly the default graph name ((equivalent to the `default_graph` key under `settings`)

`-w` or `--working-directory`
: specifies to where to place the Docker Compose configuration and dataset dumps copies/download for import

`dld.py` will process the [configuraion YAML](#config), taking care of the following steps for you:

  * collecting (by copy or download) specified RDF dump files into a single location provided to the **load** component
  * generate a Docker Compose configuration file defining containers as specified establishing appropirates [links](http://docs.docker.com/userguide/dockerlinks/), [(shared) volumes](http://docs.docker.com/userguide/dockervolumes/) and [exposed ports to the host system](http://docs.docker.com/userguide/usingdocker/#viewing-our-web-application-container) to enable you to interact with front-end components in your browser

The provided [containers](#containers) of the resulting orchestration will co-ordinate themselves so that **load** components will perform bulk-imports to the **store** component. **present** components will know how to use the SPARQL-endpoint exposed by the **store** to allow for navigation (and possibly authoring).

## Understanding the Configuration File

The following list explains all usable keywords in the config file:

<dl class="dl-horizontal">
    <di>
        <dt>datasets</dt>
        <dd>Defines the set of datasources to be preloaded and used.</dd>
    </di>
    <di>
        <dt>site</dt>
        <dd></dd>
    </di>
    <di>
        <dt>graph_name</dt>
        <dd>Specifies the base URI for the data set. Can only be used inside a <code>datasets</code> statement.</dd>
    </di>
    <di>
        <dt>location</dt>
        <dd>Points to a dump file on the web. Can only be used inside a <code>datasets</code> statement.</dd>
    </di>
    <di>
        <dt>location_list</dt>
        <dd>Points to a local file listing dump files on the web to download and import. (One URL per line, empty lines allowed.) Can only be used inside a <code>datasets</code> statement.</dd>
    </di>
    <di>
        <dt>file</dt>
        <dd>Points to a local file dump. Can only be used inside a <code>datasets</code> statement.</dd>
    </di>
    <di>
        <dt>file_list</dt>
        <dd>Points to a local file listing dump files to import. (One file path per line, empty lines allowed.) Can only be used inside a <code>datasets</code> statement.</dd>
    </di>
</dl>

<dl class="dl-horizontal">

    <dt>settings</dt>
    <dd></dd>

    <dt>default_graph</dt>
    <dd></dd>

</dl>

<dl class="dl-horizontal">
    <di>
        <dt>components</dt>
        <dd>Defines the different parts of your linked data environment as key-value mappings. Each value is either just an image name or a key-value mapping for further container configuration.</dd>
    </di>
    <di>
        <dt>store</dt>
        <dd>Sets the triple store container you want to use. Can only be used inside a <code>components</code> statement.
            <br />
            Example: <code>aksw/dld-store-virtuoso7</code>
        </dd>
    </di>
    <di>
        <dt>load</dt>
        <dd>Sets a component to perform bulk import into the store. Can only be used inside a <code>components</code> statement.</dd>
    </di>
    <di>
        <dt>present</dt>
        <dd>Collection of presentation components for your linked datasets. Either just an image name or a key-value mapping for further container configuration. Can only be used inside a <code>components</code> statement.<br />
            Example: <code>aksw/dld-present-ontowiki</code>
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

The keys under `components` and `present` (e.g. `store`, `load`, `ontowiki`) will be part of the created docker containers. The corresponding values can be either just images namges or key-value mappings themselves. These sub-ordinated key-value pairs are copied unaltered as docker-compose settings for the created containers (see the [docker-compose YAML reference](https://docs.docker.com/compose/yml/)).

## Example

The following shows a simple example of a `dld.yml` file.
Keys marked in <mark class="yellow">yellow</mark> are introduced and processed by the dld bootstrap script and keys marked in <mark class="blue">blue</mark> are docker-compose keys.

<div class="col-lg-6">
<h4>Simple dld.yml file</h4>
<pre>

<mark class="yellow">settings</mark>:
    <mark class="yellow">default_graph</mark>: "http://example.org/"

<mark class="yellow">datasets</mark>:
    <mark class="yellow">site</mark>:
        <mark class="yellow">graph_name</mark>: "http://example.org/"
        <mark class="yellow">file</mark>: "hello.ttl"

<mark class="yellow">components</mark>:
    <mark class="yellow">store</mark>:
        <mark class="blue">image</mark>: aksw/dld-store-virtuoso7
        <mark class="blue">ports</mark>: ["8895:8890"]
        <mark class="blue">environment</mark>:
            PWDDBA: "dba"
    <mark class="yellow">load</mark>:
        <mark class="blue">image</mark>: aksw/dld-load-virtuoso

</pre>

</div>


<div class="col-lg-6">
<h4>Output for Docker Compose</h4>
<pre>

load:
  environment: {DEFAULT_GRAPH: 'http://example.org/'}
  image: aksw/dld-load-virtuoso
  links: [store]
  volumes: ['/usr/src/app/examples/simple/wd-dld/models:/import']
  volumes_from: [store]
store:
  environment: {DEFAULT_GRAPH: 'http://example.org/', PWDDBA: dba}
  image: aksw/dld-store-virtuoso7
  ports: ['8895:8890']

</pre>
</div>
