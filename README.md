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

Docker Engine >= 17.05 (Docker-ce 18.03+ is required to be able to configure docker logging plugin via 'daemon.json')
Splunk >= 6.6

Heading 1: Installation

Step 1: Configure HEC
Docker logging plugin requires an HEC token.  HEC token can be configured on both Splunk Enterprise (either single instance or distributed environment) and Splunk Cloud.
Details can be found in the document below:
http://docs.splunk.com/Documentation/Splunk/7.0.3/Data/UsetheHTTPEventCollector
http://docs.splunk.com/Documentation/Splunk/7.0.3/Data/ScaleHTTPEventCollector

Note that Docker logging plugin doesn't work with HEC ack. Make sure to uncheck the box when creating the HEC token.

Step 2: Installthe plugin

There are two ways you can install the Docker Logging plugin.

Option 1: Get Plugin from Docker Store (NEED TO VERIFY AFTER PLUGIN IS PUBLISED)

# install plugin and answer security question prompt if needed
$ docker plugin install splunk-log-plugin
 
 
# enable the plugin
$ docker plugin enable splunk-log-plugin:latest
Build from Source
Clone the repo
$ git clone https://github.com/splunk/docker-logging-plugin.git
Create plugin package
$ cd docker-logging-plugin
$ make package # this creates a splunk-log-plugin.tar.gz



Option 2: Create plugin from the package

# unzip package
$ tar -xzf splunk-log-plugin.tar.gz
  
# create plugin
$ docker plugin create splunk-log-plugin:latest splunk-log-plugin/
 
 
# verify plugin is installed by running command
$ docker plugin ls
 
# enable plugin
$ docker plugin enable splunk-log-plugin:latest

Step 3: Start Containers with the plugin installed

There are two ways to configure docker containers to run with splunk-log-plugin. 

Details: https://docs.docker.com/config/containers/logging/configure/

Here is a sample daemon.json that configures splunk log plugin for all containers on the docker engine

{
  "log-driver": "splunk-log-plugin",
  "log-opts": {
    "splunk-url": "<splunk_hec_endpoint>",
    "splunk-token": "<splunk-hec-token>",
    "splunk-insecureskipverify": "true" # only if you don't want to use TLS
  }
}


Here's a sample command that configures splunk log plugin for a container

$ docker run --log-driver=splunk-log-plugin --log-opt splunk-url=<splunk_hec_endpoint> --log-opt splunk-token=<splunk-hec_token> --log-opt splunk-insecureskipverify=true -d <docker_image>

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


Heading 1: Configuration options

SAMPLE USAGE:
MyImage/MyContainer env1=val1 label1=label1 my message
MyImage/MtContainer env1=val1 lavel1-label1 {"foo": "bar"}

Docker Logging Plugin Log Options

Heading 2: Required Options

splunk-token:	Splunk HTTP Event Collector token.
splunk-url:	Path to your Splunk Enterprise, self-service Splunk Cloud instance, or Splunk Cloud managed cluster (including port and scheme used by HTTP Event Collector) in one of the following formats:  https://your_splunk_instance:8088  or  https://input-prd-p-XXXXXXX.cloud.splunk.com:8088  or  https://http-inputs-XXXXXXXX.splunkcloud.com

Heading 2: Optional Options

splunk-source: Event source.
splunk-sourcetype: Event source type.
splunk-index:	Event index. (Note that HEC token must be configured to accept the specified index)
splunk-capath: Path to root certificate. (Must be specified if splunk-insecureskipverify is false)
splunk-caname: Name to use for validating server certificate; by default the hostname of the splunk-url is used.
splunk-insecureskipverify: Ignore server certificate validation. Default to false.
splunk-format: Message format. Can be inline, json or raw. Defaults to inline. (For more infomation: Messageformats )
splunk-verify-connection: Verify on start, that docker can connect to Splunk HEC endpoint. Defaults to false.
splunk-gzip: Enable/disable gzip compression to send events to Splunk Enterprise or Splunk Cloud instance. Defaults to false.
splunk-gzip-level: Set compression level for gzip. Valid values are -1 (default), 0 (no compression), 1 (best speed) … 9 (best compression). Defaults to DefaultCompression.
tag: Specify tag for message, which interpret some markup. Default value is {{.ID}} (12 characters of the container ID). Refer to the log tag option documentation for customizing the log tag format.
labels: Comma-separated list of keys of labels, which should be included in message, if these labels are specified for container.
env: Comma-separated list of keys of environment variables, which should be included in message, if these variables are specified for container.
env-regex: Similar to and compatible with env. A regular expression to match logging-related environment variables. Used for advanced log tag options.If there is collision between the label and env keys, the value of the env takes precedence. Both options add additional fields to the attributes of a logging message.

The following is an example of the logging options specified for the Splunk Enterprise instance. The instance is installed locally on the same machine on which the Docker daemon is running. The path to the root certificate and Common Name is specified using an HTTPS scheme. This is used for verification. The SplunkServerDefaultCert is automatically generated by Splunk certificates.

$ docker run --log-driver=splunk \
             --log-opt splunk-token=176FCEBF-4CF5-4EDF-91BC-703796522D20 \
             --log-opt splunk-url=https://splunkhost:8088 \
             --log-opt splunk-capath=/path/to/cert/cacert.pem \
             --log-opt splunk-caname=SplunkServerDefaultCert \
             --log-opt tag="{{.Name}}/{{.FullID}}" \
             --log-opt labels=location \
             --log-opt env=TEST \
             --env "TEST=false" \
             --label location=west \
             <docker_image>


Heading 3: Advanced options - Environment Variables

SPLUNK_LOGGING_DRIVER_POST_MESSAGES_FREQUENCY	5s	How often driver posts messages when there is nothing to batch, i.e., the maximum time to wait for more messages to batch.

SPLUNK_LOGGING_DRIVER_POST_MESSAGES_BATCH_SIZE	1000	The number of messages the driver should collect before sending them in one batch.

SPLUNK_LOGGING_DRIVER_BUFFER_MAX	10 * 1000	The maximum amount of messages to hold in buffer and retry when the driver cannot connect to remote server.

SPLUNK_LOGGING_DRIVER_CHANNEL_SIZE	4 * 1000	How many pending messages can be in the channel used to send messages to background logger worker, which batches them. The value should be bigger than the value for SPLUNK_LOGGING_DRIVER_POST_MESSAGES_BATCH_SIZE

SPLUNK_LOGGING_DRIVER_TEMP_MESSAGES_HOLD_DURATION	100ms	Appends logs that are chunked by docker with 16kb limit. It specifies how long the system can wait for the next message to come.

SPLUNK_LOGGING_DRIVER_TEMP_MESSAGES_BUFFER_SIZE	1mb	Appends logs that are chunked by docker with 16kb limit. It specifies the biggest message that the system can reassemble.



Heading 1: Message formats

There are three logging driver messaging formats: inline (default), json, and raw.
The default format is inline where each log message is embedded as a string and is assigned to "line" field. For example:
{
    "attrs": {
        "env1": "val1",
        "label1": "label1"
    },
    "tag": "MyImage/MyContainer",
    "source":  "stdout",
    "line": "my message"
}
{
    "attrs": {
        "env1": "val1",
        "label1": "label1"
    },
    "tag": "MyImage/MyContainer",
    "source":  "stdout",
    "line": "{\"foo\": \"bar\"}"  //though this is a string that can be marshaled to json, it is still treated as a string
}

If your messages are JSON objects, you may want to embed them in the message we send to Splunk.
To format messages as json objects, set --log-opt splunk-format=json. The driver trys to parse every line as a JSON object and embed the json object to "line" field. If it cannot parse the message, it is sent inline. For example:
{
    "attrs": {
        "env1": "val1",
        "label1": "label1"
    },
    "tag": "MyImage/MyContainer",
    "source":  "stdout",
    "line": "my message"  //fall back to a string
}
{
    "attrs": {
        "env1": "val1",
        "label1": "label1"
    },
    "tag": "MyImage/MyContainer",
    "source":  "stdout",
    "line": {
        "foo": "bar"
    }
}

If --log-opt splunk-format=raw, each message together with attributes (environment variables and labels) and tags are combined in a raw string.  Attributes and tags are prefixed to the message. For example:
#<tag> <env=value> <label=value> <log_messaage>
MyImage/MyContainer env1=val1 label1=label1 my message
MyImage/MyContainer env1=val1 label1=label1 {"foo": "bar"}













			 

			 
			 
