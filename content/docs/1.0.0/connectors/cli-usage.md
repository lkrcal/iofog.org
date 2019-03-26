# Connector CLI Usage

```sh
service iofog-connector <command>
```

## Commands

|                        |                                                        |
| ---------------------- | ------------------------------------------------------ |
| [start](#start)        | Start connector service.                               |
| [stop](#stop)          | Stop connector service.                                |
| [status](#status)      | Display current status information about the software. |
| [create token](#token) | Create token to access Connector REST API.             |
| [delete token](#token) | Delete token.                                          |

---

## start

Start the iofog-connector daemon

```sh
service iofog-connector start
```

---

## stop

Stop the iofog-connector daemon

```sh
service iofog-connector stop
```

---

## status

Display current status information about the software.

```sh
service iofog-connector status
```

## token

Create token to access Connector REST API

```sh
iofog-connector --add-token
```

Delete token

```sh
iofog-connector --del-token=01826c283d7e4ae4bd8055232fff2aaf2ff7ad5aaf1f4163ba701bf2806a200d
```

# Configuration

Configuration is located in `/etc/iofog-connector/iofog-connector.properties`

```
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

### main settings

“connector.isDevModeEnabled” - develop mode (should be false if ssl is used)

“server.port” - connector REST API port (8080 for HTTP and 443 for HTTPS)

### ssl settings

“server.ssl.key-store-type” - keystore format

“server.ssl.key-store” - keystore location

“server.ssl.key-store-password” - keystore password

“server.ssl.key-alias” - key alias in the keystore

### ActiveMQ settings

“artemis.agent-user” - user to connect to ActiveMQ

“artemis.agent-password” - user password

“artemis.agent-role” - user role

“artemis.address” - ActiveMQ address to send iofog messages to

“artemis.host” - ActiveMQ host.

“artemis.port” - ActiveMQ port.

# Certificates

Here is how to generate server side keystore:

```sh
 cat ssl_certificate.crt > bundle.crt
 cat IntermediateCA.crt >> bundle.crt
 openssl pkcs12 -export -in bundle.crt -inkey server-key.pem -out keystore/server-side-keystore.p12 -name iofog
```

“ssl_certificate.crt” - server certificate

“IntermediateCA.crt” - CA certificate

# Logs

The log files are located inside `/var/log/iofog-connector`
