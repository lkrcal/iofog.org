# Connector API

Connector exposes API and it’s API where you have a set of identities. Fog Controller has the proper identity and it’s able to tell Connector “I want you to open up some connections”. Fog Controller uses Connector API to tell it to do and Connector simply replies whether it is successful or not successful.

ioFog Agent connects to Connectors and through connecting Connectors traffic is able to move between fog nodes. In addition Connector has the capability to open traffic to the outside world so the outside users can get route into fog node.

### Connector offers two connectivity types:

**1) The first type, called a public pipe, provides a way to securely access Fog software and data from anywhere on in the world. Connector punches through firewalls and NATed networks to perform automatic internetworking of the Fog.**

**The Endpoint to create a public pipe connection is displayed below:**

**Request**

```json
Endpoint: /route
Method: POST
Header Content-Type: application/json
Header Authorization: <token>
Body:
{
	"publisherId": "2HKHh4xvxRydfQfTHqkXHBJ86Tq7RDhD",
	"routeType": "public"
}
```

**Response:**

```json
{
  "routeType": "PUBLIC",
  "publisherId": "2HKHh4xvxRydfQfTHqkXHBJ86Tq7RDhD",
  "passKey": "d836053a-4430-4b16-996e-ba022f5aa560",
  "privatePort": 12349,
  "publicPort": 12350,
  "maxConnections": 60
}
```

“routeType” - connectivity type created.

“publisherId” - uuid of publishing microservice

“passKey” is used by the ioFog agent to establish a secure connection to the Connector. The Fog agent will receive the information through the Fog controller and tell you that you need to connect.

“privatePort” - port that will be used by the ioFog agent.

“publicPort” - port that will be used by the Connector for public URL access.

“maxConnections” means how many connection threads the ioFog agent will make with the Connector. You can have many users at the same time.

**The Endpoint to remove public pipe connection is displayed below:**

**Request**

```json
Endpoint: /route/public/2HKHh4xvxRydfQfTHqkXHBJ86Tq7RDhD
Method: DELETE
Header Authorization: <token>
```

“2HKHh4xvxRydfQfTHqkXHBJ86Tq7RDhD” - uuid of publishing microservice.

The request responds with status code 204 if successful.

**2) The second type, called a private pipe, consumes bandwidth on the Connector but stabilizes connectivity between Fog nodes that can’t normally see each other.**

Connector is available for 2 different ioFog agents talking to each other.

**The Endpoint to create private pipe connection is displayed below:**

**Request**

```json
Endpoint: /route
Method: POST
Header Content-Type: application/json
Header Authorization: <token>
Body:
{
	"publisherId": "2HKHh4xvxRydfQfTHqkXHBJ86Tq7RDhD",
	"routeType": "private"
}
```

**Response:**

```json
{
  "routeType": "PRIVATE",
  "publisherId": "2HKHh4xvxRydfQfTHqkXHBJ86Tq7RDhD",
  "passKey": "48715faa-00d0-4141-80ba-7e5542a29b3c"
}
```

“routeType” - connectivity type created.

“publisherId” - uuid of publishing microservice

“passKey” is used by the ioFog agent to establish a secure connection to the Connector. The Fog agent will receive the information through the Fog controller and tell you that you need to connect.

**The Endpoint to remove private pipe connection is displayed below:**

**Request**

```json
Endpoint: /route/private/2HKHh4xvxRydfQfTHqkXHBJ86Tq7RDhD
Method: DELETE
Header Authorization: <token>
```

“2HKHh4xvxRydfQfTHqkXHBJ86Tq7RDhD” - uuid of publishing microservice.

The request responds with status code 204 if successful.

---

**In Public mode the URL is generated as follows:**

Example: ${protocol}://${address}\${publicPort}

where

{protocol} is either http:// or https://

{address} is either IP address or domain name

---

In iofog-connector.properties file

When connector.isDevModeEnabled=true, it's http connection.

When connector.isDevModeEnabled=false, it's https connection.
