# hello-world
tutorial practice
helloo my name is Aldo and i am a Hedgehog
Outline Splunk Docker Plugin

Heading 1: What does the Splunk Docker Plugin do?

We currently have heavily downloaded Splunk Enterprise and Forwarder images on 
Docker Hub, Docker Store, and GitHub. The new plugin leverages Docker internally so 
that customers can use Splunk within containerized environments. 

Works with Docker to collaboratively define a joint support agreement.

Heading 1: Prerequisites

-What to install first


Heading 2: Architecture and developers
-Infor for developersArchitecture
-A description of the new architecture that is relevant to users and developers

Heading 2: Installation

dInstall plugin
There are two ways you can get splunk-log-plugin
Get Plugin from Docker Store (NEED TO VERIFY AFTER PLUGIN IS PUBLISED)
# install plugin and answer security question prompt if needed
$ docker plugin install splunk-log-plugin
 
 
# enable the plugin
$ docker plugin enable splunk-log-plugin:latest
Configuration options:

Build from Source
Clone the repo
$ git clone https://github.com/splunk/docker-logging-plugin.git
Create plugin package
$ cd docker-logging-plugin
$ make package # this creates a splunk-log-plugin.tar.gz
Create plugin from the package
# unzip package
$ tar -xzf splunk-log-plugin.tar.gz
 
 
# create plugin
$ docker plugin create splunk-log-plugin:latest splunk-log-plugin/
 
 
# verify plugin is installed by running command
$ docker plugin ls
 
 
# enable plugin
$ docker plugin enable splunk-log-plugin:latest


Heading: Start docker

Start Containers with plugin
There are two ways to configure docker containers to run with splunk-log-plugin. Details: https://docs.docker.com/config/containers/logging/configure/
Sample daemon.json to configure splunk log plugin for all containers on the docker engine
# substitute the values in <>
{
  "log-driver": "splunk-log-plugin",
  "log-opts": {
    "splunk-url": "<splunk_hec_endpoint>",
    "splunk-token": "<splunk-hec-token>",
    "splunk-insecureskipverify": "true" # only if you don't want to use TLS


Using the Docker Plugin

Configuration options

SAMPLE:
MyImage/MyContainer env1=val1 label1=label1 my message
MyImage/MtContainer env1=val1 lavel1-label1 {"foo": "bar"}

splunk_token
splunk-url
splunk-source
splunk-sourcetype
splunk-index
splunk-capathsplunk-caname
splunk-insecureskipverify
splunk-format
splunk-verify-connection
splunk-gzip
splunk-gzop-level
tag
labels
env
env-regex

Advanced options

SPLUNK_LOGGING_POST_MESSAGES_FREQUENCY

SPLUNK_LOGGING_POST_MESSAGES_BATCH_SIZE

SPLUNK_LOGGING_BUFFER_MAX

SPLUNK_LOGGING_CHANNEL_SIZE

Heading 1; Message formats
\Need to collect iformation for this








Splunk enterprise example:




$ docker run --log-driver=splunk =
			 --log-opt splunk-url=https://splunkhost:8088 \
			 --log-opt splunk-token=
			 --log-opt splunk-capath=/path/to/cert/cacert.pem \		
			 --log-opt splunk-caname=SplunkServerDefaultCert \
			 --log-opt tag="{{.Name}} / {{.FullID}}" \
			 --log-opt lbels=location \
			 --log-opt env=location \
			 --log-opt env=TEST \
			 --env "TEST=false" \
			 --label location=west \
			 -it ubuntu bash
			 
			 

			 
			 
