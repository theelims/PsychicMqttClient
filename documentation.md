# PsychicMqttClient API Documentation

## Introduction

`PsychicMqttClient` is a fully featured asynchronous MQTT 3.1.1 client for ESP32 with support for SSL/TLS and MQTT over WebSocket. It uses the ESP-IDF MQTT client library under the hood and adds a powerful but easy-to-use API on top of it. This library supports MQTT over TCP, SSL with mbedtls, MQTT over WebSocket, and MQTT over WebSocket Secure.

## Class: `PsychicMqttClient`

A class that wraps the ESP-IDF MQTT client and provides a more user-friendly interface. The API is very similar to `AsyncMqttClient` for the ESP32 by Marvin Roger, so that this library can be used as an almost drop-in replacement.

### Public Methods

#### `PsychicMqttClient()`

Constructs a new instance of the `PsychicMqttClient` class.

**Usage:**

```cpp
PsychicMqttClient mqttClient;
```

#### `~PsychicMqttClient()`

Destroys the instance of the `PsychicMqttClient` class.

#### `setKeepAlive(int keepAlive = 120)`

Sets the keep-alive interval in seconds for the MQTT connection. This is the time the client waits for a message from the server before sending a ping to keep the connection alive.

- **Parameters:**
  - `keepAlive`: The keep-alive interval in seconds. Defaults to `120`.
- **Returns:** A reference to the `PsychicMqttClient` instance for chaining.

**Usage:**

```cpp
mqttClient.setKeepAlive(60); // Set keep-alive to 60 seconds
```

#### `setAutoReconnect(bool reconnect = true)`

Sets the auto-reconnect flag for the MQTT connection. If set to `true`, the client will automatically try to reconnect to the server if the connection is lost.

- **Parameters:**
  - `reconnect`: The auto-reconnect flag. Defaults to `true`.
- **Returns:** A reference to the `PsychicMqttClient` instance for chaining.

**Usage:**

```cpp
mqttClient.setAutoReconnect(true); // Enable auto-reconnect
```

#### `setClientId(const char *clientId)`

Sets the client ID for the MQTT connection. The client ID must be unique for each client connecting to the same broker.

- **Parameters:**
  - `clientId`: The client ID. Defaults to `ESP32_%CHIPID%` where `%CHIPID%` are the last 3 bytes of the MAC address in hex format.
- **Returns:** A reference to the `PsychicMqttClient` instance for chaining.

**Usage:**

```cpp
mqttClient.setClientId("my-esp32-client");
```

#### `setCleanSession(bool cleanSession = true)`

Sets the clean session flag for the MQTT connection. If `true`, the broker will discard any previous session information for the client. If `false`, the broker will restore the previous session.

- **Parameters:**
  - `cleanSession`: The clean session flag. Defaults to `true`.
- **Returns:** A reference to the `PsychicMqttClient` instance for chaining.

**Usage:**

```cpp
mqttClient.setCleanSession(false); // Use a persistent session
```

#### `setBufferSize(int bufferSize = 1024)`

Sets the size for the MQTT send/receive buffer. If messages exceed the buffer size, the message will be split into multiple chunks. Received messages will be assembled into the original message.

- **Parameters:**
  - `bufferSize`: The buffer size in bytes. Defaults to `1024`.
- **Returns:** A reference to the `PsychicMqttClient` instance for chaining.

**Usage:**

```cpp
mqttClient.setBufferSize(2048); // Increase buffer size to 2KB
```

#### `setTaskStackAndPriority(int stackSize, int prio)`

Sets the task stack size and priority for the MQTT client task.

- **Parameters:**
  - `stackSize`: The task stack size in bytes. Defaults to `6144`.
  - `prio`: The task priority. Defaults to `5`.
- **Returns:** A reference to the `PsychicMqttClient` instance for chaining.

**Usage:**

```cpp
mqttClient.setTaskStackAndPriority(8192, 4); // Increase stack size and lower priority
```

#### `setCACert(const char *rootCA, size_t rootCALen = 0)`

Sets the CA root certificate for the MQTT server for secure connections (TLS).

- **Parameters:**
  - `rootCA`: The certificate in PEM or DER format.
  - `rootCALen`: Optional length of the certificate if it's not null-terminated.
- **Returns:** A reference to the `PsychicMqttClient` instance for chaining.

**Usage:**

```cpp
const char* ca_cert = "-----BEGIN CERTIFICATE-----
"
                    "MIID...etc...
"
                    "-----END CERTIFICATE-----
";
mqttClient.setCACert(ca_cert);
```

#### `setCACertBundle(const uint8_t *bundle, size_t bundleLen = 0)`

Sets a CA root certificate bundle for the MQTT server. Use this method if you have multiple CA root certificates and this is the only place using SSL/TLS. Otherwise, use `attachArduinoCACertBundle()` to attach an existing certificate bundle.

- **Parameters:**
  - `bundle`: The certificate bundle in PEM or DER format.
  - `bundleLen`: Optional length of the bundle.
- **Returns:** A reference to the `PsychicMqttClient` instance for chaining.

**Usage:**

```cpp
// Assuming 'cert_bundle' is a byte array containing the certificate bundle
mqttClient.setCACertBundle(cert_bundle, sizeof(cert_bundle));
```

#### `attachArduinoCACertBundle(bool attach = true)`

Attaches an existing CA root certificate bundle for the MQTT server. For example, if you already use `WiFiClientSecure` and want to use the same CA root certificate bundle for MQTT.

- **Parameters:**
  - `attach`: Whether to attach or detach the CA root certificate bundle. Defaults to `true`.
- **Returns:** A reference to the `PsychicMqttClient` instance for chaining.

**Usage:**

```cpp
mqttClient.attachArduinoCACertBundle();
```

#### `setCredentials(const char *username, const char *password = nullptr)`

Sets the credentials for the MQTT connection.

- **Parameters:**
  - `username`: The username for the MQTT connection. Defaults to `nullptr`.
  - `password`: The password for the MQTT connection. Defaults to `nullptr`.
- **Returns:** A reference to the `PsychicMqttClient` instance for chaining.

**Usage:**

```cpp
mqttClient.setCredentials("myuser", "mypassword");
```

#### `setClientCertificate(const char *clientCert, const char *clientKey, size_t clientCertLen = 0, size_t clientKeyLen = 0)`

Authenticates via mutual X.509 certificate. This is used for client-side authentication.

- **Parameters:**
  - `clientCert`: The X.509 certificate issued from the same CA as the server cert.
  - `clientKey`: The private key for the X.509 certificate.
  - `clientCertLen`: Optional length of the client certificate if it's not null-terminated.
  - `clientKeyLen`: Optional length of the client key if it's not null-terminated.
- **Returns:** A reference to the `PsychicMqttClient` instance for chaining.

**Usage:**

```cpp
const char* client_cert = "-----BEGIN CERTIFICATE-----
"
                        "MIID...etc...
"
                        "-----END CERTIFICATE-----
";
const char* client_key = "-----BEGIN PRIVATE KEY-----
"
                       "MIIE...etc...
"
                       "-----END PRIVATE KEY-----
";
mqttClient.setClientCertificate(client_cert, client_key);
```

#### `setWill(const char *topic, uint8_t qos, bool retain, const char *payload = nullptr, int length = 0)`

Sets the last will and testament (LWT) for the MQTT connection. The LWT is a message that the broker will publish on behalf of the client if the client disconnects ungracefully.

- **Parameters:**
  - `topic`: The topic for the last will and testament.
  - `qos`: The QoS level for the last will and testament.
  - `retain`: The retain flag for the last will and testament.
  - `payload`: The payload for the last will and testament. Defaults to `nullptr`.
  - `length`: The length of the payload for the last will and testament. Defaults to `0`.
- **Returns:** A reference to the `PsychicMqttClient` instance for chaining.

**Usage:**

```cpp
mqttClient.setWill("esp32/status", 1, true, "offline");
```

#### `setServer(const char *uri)`

Sets the MQTT server URI. Supports `mqtt://`, `mqtts://`, `ws://`, and `wss://` as transport protocols. Fully supports SSL/TLS.

- **Examples:**
  - `mqtt://162.168.10.1`
  - `mqtt://mqtt.eclipseprojects.io`
  - `mqtt://mqtt.eclipseprojects.io:1884`
  - `mqtt://username:password@mqtt.eclipseprojects.io:1884`
  - `mqtts://mqtt.eclipseprojects.io`
  - `mqtts://mqtt.eclipseprojects.io:8884`
  - `ws://mqtt.eclipseprojects.io:80/mqtt`
  - `wss://mqtt.eclipseprojects.io:443/mqtt`
- **Parameters:**
  - `uri`: The MQTT server URI.
- **Returns:** A reference to the `PsychicMqttClient` instance for chaining.

**Usage:**

```cpp
mqttClient.setServer("mqtts://mqtt.eclipseprojects.io:8883");
```

#### `onConnect(OnConnectUserCallback callback)`

Registers a callback function to be called when the MQTT client is connected.

- **Callback Signature:** `void onConnectCallback(bool sessionPresent)`
  - `sessionPresent`: `true` if a previous session was resumed.
- **Parameters:**
  - `callback`: The callback function to be registered.
- **Returns:** A reference to the `PsychicMqttClient` instance for chaining.

**Usage:**

```cpp
void onMqttConnect(bool sessionPresent) {
  Serial.println("Connected to MQTT.");
  Serial.print("Session present: ");
  Serial.println(sessionPresent);
}

mqttClient.onConnect(onMqttConnect);
```

#### `onDisconnect(OnDisconnectUserCallback callback)`

Registers a callback function to be called when the MQTT client is disconnected.

- **Callback Signature:** `void onDisconnectCallback(bool sessionPresent)`
  - `sessionPresent`: `true` if a previous session was resumed.
- **Parameters:**
  - `callback`: The callback function to be registered.
- **Returns:** A reference to the `PsychicMqttClient` instance for chaining.

**Usage:**

```cpp
void onMqttDisconnect(bool sessionPresent) {
  Serial.println("Disconnected from MQTT.");
}

mqttClient.onDisconnect(onMqttDisconnect);
```

#### `onSubscribe(OnSubscribeUserCallback callback)`

Registers a callback function to be called when a topic is subscribed.

- **Callback Signature:** `void onSubscribeCallback(int msgId)`
  - `msgId`: The message ID of the subscribe acknowledgment.
- **Parameters:**
  - `callback`: The callback function to be registered.
- **Returns:** A reference to the `PsychicMqttClient` instance for chaining.

**Usage:**

```cpp
void onMqttSubscribe(int msgId) {
  Serial.print("Subscribe acknowledged for msgId: ");
  Serial.println(msgId);
}

mqttClient.onSubscribe(onMqttSubscribe);
```

#### `onUnsubscribe(OnUnsubscribeUserCallback callback)`

Registers a callback function to be called when a topic is unsubscribed.

- **Callback Signature:** `void onUnsubscribeCallback(int msgId)`
  - `msgId`: The message ID of the unsubscribe acknowledgment.
- **Parameters:**
  - `callback`: The callback function to be registered.
- **Returns:** A reference to the `PsychicMqttClient` instance for chaining.

**Usage:**

```cpp
void onMqttUnsubscribe(int msgId) {
  Serial.print("Unsubscribe acknowledged for msgId: ");
  Serial.println(msgId);
}

mqttClient.onUnsubscribe(onMqttUnsubscribe);
```

#### `onMessage(OnMessageUserCallback callback)`

Registers a callback function to be called when a message is received. Multipart messages will be reassembled into the original message.

- **Callback Signature:** `void onMessageCallback(char *topic, char *payload, int retain, int qos, bool dup)`
  - `topic`: The topic of the received message.
  - `payload`: The payload of the received message.
  - `retain`: The retain flag of the message.
  - `qos`: The QoS level of the message.
  - `dup`: The duplicate flag of the message.
- **Parameters:**
  - `callback`: The callback function to be registered.
- **Returns:** A reference to the `PsychicMqttClient` instance for chaining.

**Usage:**

```cpp
void onMqttMessage(char* topic, char* payload, int retain, int qos, bool dup) {
  Serial.print("Message arrived [");
  Serial.print(topic);
  Serial.print("] ");
  Serial.println(payload);
}

mqttClient.onMessage(onMqttMessage);
```

#### `onTopic(const char *topic, int qos, OnMessageUserCallback callback)`

Registers a callback function to be called when a message is received on a specific topic. Multipart messages will be reassembled into the original message. Fully supports MQTT Wildcards. Will automatically subscribe to all topics once the client is connected.

- **Callback Signature:** `void onTopicCallback(char *topic, char *payload, int retain, int qos, bool dup)`
  - `topic`: The topic of the received message.
  - `payload`: The payload of the received message.
  - `retain`: The retain flag of the message.
  - `qos`: The QoS level of the message.
  - `dup`: The duplicate flag of the message.
- **Parameters:**
  - `topic`: The topic to listen for. MQTT Wildcards are fully supported.
  - `qos`: The QoS level to listen for.
  - `callback`: The callback function to be registered.
- **Returns:** A reference to the `PsychicMqttClient` instance for chaining.

**Usage:**

```cpp
void onSensorData(char* topic, char* payload, int retain, int qos, bool dup) {
  Serial.print("Sensor data received on topic ");
  Serial.print(topic);
  Serial.print(": ");
  Serial.println(payload);
}

mqttClient.onTopic("sensors/+/data", 1, onSensorData);
```

#### `onPublish(OnPublishUserCallback callback)`

Registers a callback function to be called when a message is published.

- **Callback Signature:** `void onPublishCallback(int msgId)`
  - `msgId`: The message ID of the published message.
- **Parameters:**
  - `callback`: The callback function to be registered.
- **Returns:** A reference to the `PsychicMqttClient` instance for chaining.

**Usage:**

```cpp
void onMqttPublish(int msgId) {
  Serial.print("Publish acknowledged for msgId: ");
  Serial.println(msgId);
}

mqttClient.onPublish(onMqttPublish);
```

#### `onError(OnErrorUserCallback callback)`

Registers a callback function to be called when an error occurs.

- **Callback Signature:** `void onErrorCallback(esp_mqtt_error_codes_t error)`
  - `error`: The error code.
- **Parameters:**
  - `callback`: The callback function to be registered.
- **Returns:** A reference to the `PsychicMqttClient` instance for chaining.

**Usage:**

```cpp
void onMqttError(esp_mqtt_error_codes_t error) {
  Serial.print("MQTT Error: ");
  Serial.println(error);
}

mqttClient.onError(onMqttError);
```

#### `connected()`

Checks if the MQTT client is connected.

- **Returns:** `true` if the client is connected, `false` otherwise.

**Usage:**

```cpp
if (mqttClient.connected()) {
  Serial.println("MQTT client is connected.");
} else {
  Serial.println("MQTT client is not connected.");
}
```

#### `connect()`

Connects the MQTT client to the server.

- **Note:** All parameters must be set before calling this method.

**Usage:**

```cpp
mqttClient.connect();
```

#### `disconnect()`

Disconnects the MQTT client from the server. This call might be blocking until the client is stopped cleanly.

**Usage:**

```cpp
mqttClient.disconnect();
```

#### `forceStop()`

Forcefully stops the MQTT client and disconnects from the server. This does not trigger the `onDisconnect` callbacks.

**Usage:**

```cpp
mqttClient.forceStop();
```

#### `subscribe(const char *topic, int qos)`

Subscribes to a topic. The server must be connected for a subscription to succeed.

- **Parameters:**
  - `topic`: The topic to subscribe to.
  - `qos`: The QoS level for the subscription.
- **Returns:** Message ID on success, `-1` on failure.

**Usage:**

```cpp
int msgId = mqttClient.subscribe("my/topic", 1);
if (msgId != -1) {
  Serial.print("Subscribed with msgId: ");
  Serial.println(msgId);
}
```

#### `unsubscribe(const char *topic)`

Unsubscribes from a topic. The server must be connected for an unsubscription to succeed.

- **Parameters:**
  - `topic`: The topic to unsubscribe from.
- **Returns:** Message ID on success, `-1` on failure.

**Usage:**

```cpp
int msgId = mqttClient.unsubscribe("my/topic");
if (msgId != -1) {
  Serial.print("Unsubscribed with msgId: ");
  Serial.println(msgId);
}
```

#### `publish(const char *topic, int qos, bool retain, const char *payload = nullptr, int length = 0, bool async = true)`

Publishes a message to a topic.

- **Parameters:**
  - `topic`: The topic to publish to.
  - `qos`: The QoS level (0-2) for the message.
  - `retain`: The retain flag for the message.
  - `payload`: The payload for the message. Defaults to `nullptr`.
  - `length`: The length of the payload. Defaults to `0`.
  - `async`: Whether to enqueue the message for asynchronous publishing. Defaults to `true`. `false` means blocking until the message is published.
- **Returns:** Message ID on success, `-1` on failure.

**Usage:**

```cpp
int msgId = mqttClient.publish("my/topic", 1, false, "hello world");
if (msgId != -1) {
  Serial.print("Published with msgId: ");
  Serial.println(msgId);
}
```

#### `getClientId()`

Gets the client ID of the MQTT client.

- **Returns:** The client ID.

**Usage:**

```cpp
const char* clientId = mqttClient.getClientId();
Serial.print("Client ID: ");
Serial.println(clientId);
```

#### `getMqttConfig()`

Returns the ESP-IDF MQTT client config object of the MQTT client in case lower-level access is needed.

- **Returns:** The config object.

**Usage:**

```cpp
esp_mqtt_client_config_t* mqttConfig = mqttClient.getMqttConfig();
// Access lower-level configuration if needed
```
