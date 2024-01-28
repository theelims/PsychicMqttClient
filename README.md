# PsychicMqttClient

Fully featured async MQTT client for ESP32 with support for SSL/TLS and MQTT over WS. Uses the ESP-IDF MQTT client library under the hood and adds a powerful and easy to use API on top of it. Supports MQTT over TCP, SSL with mbedtls, MQTT over Websocket, MQTT over Websocket Secure.

The API is very similar to AsyncMqttClient for the ESP32 by Marvin Roger, so that this library can be used as a almost drop-in replacement.

## Features

- Supports MQTT 3.1.1 with QoS 0, QoS 1 and QoS 3
- Supports MQTT over TCP and MQTT over websocket
- Full support for SSL/TSL encryption - for both MQTT over TCP and MQTT over WS
- No limitation in buffer size for transmit and receive messages. Multipart messages are reassembled.
- Fully asynchronous and non-blocking
- No dependencies
- Flexible and powerful event-based API
  - `onTopic()` event which calls a callback every time a message on a specific subscribed topic is received
