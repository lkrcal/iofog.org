# Setup Your Connectors (optional)

If the microservices running on your nodes need to communicate with other microservices in your network, ioFog includes an _*optional*_ daemon called a **Connector**. The Connector assists in providing automatic discovery and NAT traversal, brokering direct peer-to-peer (P2P) communication when possible.

### Minimum Requirements

- Processor: x86-64 or ARM Dual Core or better
- RAM: 1 GB minimum
- Hard Disk: 5 GB minimum
- Linux kernel v3.10+ (Ubuntu, CentOS, Raspbian, etc)
- Java Runtime v8.0.0 or higher

## Setup

### Install Java v8.0.0+

You can find official Java SE Runtime downloads on [Oracle's website](https://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html) or alternatively install the [OpenJDK Runtime](http://openjdk.java.net/install/).

You can also use apt-get:

```sh
sudo add-apt-repository ppa:webupd8team/java
sudo apt-get update
sudo apt-get install oracle-java8-installer
```

### Install Connector Daemon

#### Ubuntu/Debian

```sh
curl -s https://packagecloud.io/install/repositories/iofog/iofog-connector/script.deb.sh | sudo bash
sudo apt-get install iofog-connector
```

#### CentOS/Red Hat/Fedora

```sh
curl -s https://packagecloud.io/install/repositories/iofog/iofog-connector/script.rpm.sh | sudo bash
sudo yum install iofog-connector
```

## Connector Configuration

The Connector has configuration file that is located at `/etc/iofog-connector/iofog-connector.properties`.

This configuration file has several configuration properties listed below:

| Field                             | Description                                               |
| --------------------------------- | --------------------------------------------------------- |
| `"connector.isDevModeEnabled"`    | develop mode (should be false if ssl is used)             |
| `"server.port"`                   | connector REST API port (8080 for HTTP and 443 for HTTPS) |
| `"server.ssl.key-store-type"`     | keystore format                                           |
| `"server.ssl.key-store"`          | keystore location                                         |
| `"server.ssl.key-store-password"` | keystore password                                         |
| `"server.ssl.key-alias"`          | key alias in the keystore                                 |
| `"artemis.agent-user"`            | user to connect to ActiveMQ                               |
| `"artemis.agent-password"`        | user password                                             |
| `"artemis.agent-role"`            | user role                                                 |
| `"artemis.address"`               | ActiveMQ address to send iofog messages to                |
| `"artemis.host"`                  | ActiveMQ host                                             |
| `"artemis.port"`                  | ActiveMQ port                                             |

Here's an example:

```
# This is only an example. Make sure to change
# these values for your unique configuration.

spring.application.name=iofog-connector

connector.isDevModeEnabled=true

########### ssl settings ###########
# Comment out all ssl configs in order to switch to http

#server.port=443
#server.ssl.key-store-type=PKCS12
#server.ssl.key-store=classpath:server-side-keystore.p12
#server.ssl.key-store-password=secureexample
#server.ssl.key-alias=iofog
########### ssl settings ###########

########### ActiveMQ settings ###########
artemis.agent-user=agent
artemis.agent-password=agent123
artemis.agent-role=agent
artemis.address=pubsub.iofog
artemis.host=connector.iofog.org
artemis.port=5500
########### ActiveMQ settings ###########
```

## Dev Mode

The Connector has a Dev Mode that allows you to get up and running more quickly without needing to deal with SSL certificates.

When you're ready for production, make sure you disable Dev Mode on Agent, Controller and Connector, and have valid SSL certificates installed.

To enable Dev Mode on the Connector you'll need to first edit your configuration file `/etc/iofog-connector/iofog-connector.properties`,
changing the property connector.isDevModeEnabled to true (it defaults to true).

If your Connector is already registered with your Controller, you'll need to update it to use Dev Mode as well:

```sh
iofog-controller connector update --name <connector_name> --dev-mode-on
```

Otherwise, when you do register it with the Controller, make sure you include the `--dev-mode-on` flag.

With these enabled, the Connector will send and receive communications using `http://`, not `https://`, bypassing any need for SSL certificates.

## SSL Certificates

When not running in developer mode, the Connector requires keystore that contains a valid server and CA certificates.

Ideally certificates would be signed by a Certificate Authority, but because that would require a public domain name, self-signed certificates are accepted as well.

Here are properties in `/etc/iofog-connector/iofog-connector.properties` file required for ssl configuration:

```
connector.isDevModeEnabled=false
server.port=443
server.ssl.key-store-type=PKCS12
server.ssl.key-store=classpath:server-side-keystore.p12
server.ssl.key-store-password=secureexample
server.ssl.key-alias=iofog

```

If you are using a self-signed certificate for your Connector, you'll need to make sure that you have enabled that feature from the Controller, wherever it is running:

```sh
# NOTE: this is the *Controller* not your Connector!

iofog-controller connector add \
  --name "my-connector" \
  --domain "example.com" \
  --self-signed-on \
  --dev-mode-off \
  --ca-cert "$(< path/to/ca.cert) \
  --server-cert "$(< path/to/server.cert) \
  --keystore-password changeit \
  --port 5500 \
  --user agent \
  --user-password agent123 \
  --token 01826c283d7e4ae4bd8055232fff2aaf2ff7ad5aaf1f4163ba701bf2806a200d

# If you are using Dev Mode, you need to also include --dev-mode-on
```

The `--ca-cert "$(< path/to/ca.cert)"` argument is used to provide a copy of our self-signed ca certificate.

The `--server-cert "$(< path/to/server.cert)"` argument is used to provide a copy of our self-signed server certificate.

<aside class="notifications tip">
  <h3><img src="/images/icos/ico-tip.svg" alt=""> Use the same certificate for Controllers, Connectors, and Agents</h3>
  <p>If you use self-signed certificates your Controllers, Connectors, and Agents likely need to be configured to use the same certificate/key pair so their communication is trusted.</p>
</aside>

## Create authorization token

Connector REST API requires token to be passed in Authorization header. Run following command to generate it:

```sh
sudo iofog-connector --add-token

Authorization Token: 01826c283d7e4ae4bd8055232fff2aaf2ff7ad5aaf1f4163ba701bf2806a200d
```

## Delete authorization token

```sh
sudo iofog-connector --del-token

Deleting Token: 01826c283d7e4ae4bd8055232fff2aaf2ff7ad5aaf1f4163ba701bf2806a200d
```

## Start the Connector

Now that our Connector is setup, let's go ahead and start it up using the Linux [service](https://linux.die.net/man/8/service) command.

```sh
sudo service iofog-connector start
```

If you need to stop or restart it, you can use `stop`

```sh
sudo service iofog-connector stop
```

Also you can check connector status with following:

```sh
sudo service iofog-connector status
```

## Add To Your Controller

The last step is to register your Connector with your Controller. This will require providing a domain and and static IP address to reach your Connector as arguments to the Controller's `connector add` command:

```sh
iofog-controller connector add \
  --name "my-connector" \
  --domain "example.com" \
  --dev-mode-on \
  --port 5500 \
  --user agent \
  --user-password agent123 \
  --token 01826c283d7e4ae4bd8055232fff2aaf2ff7ad5aaf1f4163ba701bf2806a200d
```

## Conclusion

We now have a running Connector! If you haven't already, you'll want to next setup your ioFog [node Agents](setup-your-agents.html), and [your Controller](setup-your-controllers.html).

You can also [learn more about Connectors](../connectors/overview.html) or change additional [configuration options](../connectors/cli-usage.html).
