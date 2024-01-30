# PsychicMqttClient

Fully featured async MQTT 3.1.1 client for ESP32 with support for SSL/TLS and MQTT over WS. Uses the ESP-IDF MQTT client library under the hood and adds a powerful but easy to use API on top of it. Supports MQTT over TCP, SSL with mbedtls, MQTT over Websocket and MQTT over Websocket Secure.

There are countless popular MQTT client libraries available for Arduino and ESP32. Like [AsyncMqttClient](https://github.com/marvinroger/async-mqtt-client) by Marvin Roger, [pubsubclient](https://github.com/knolleary/pubsubclient) by knolleary and [arduino-mqtt](https://github.com/256dpi/arduino-mqtt) by 256dpi. They are widely used, but all have their unique limitations. Like not supporting all QoS levels, limited message size to the buffer size and none of them has a practical support for SSL/TLS. Also MQTT over websocket is missing in all of them.

The API is very similar to [AsyncMqttClient](https://github.com/marvinroger/async-mqtt-client) for the ESP32 by Marvin Roger, so that this library can be used almost as a drop-in replacement. Only minor adjustments are necessary.

## Features

- Supports MQTT 3.1.1 with QoS 0, QoS 1 and QoS 3
- Compatible with all ESP32 variants (ESP32, ESP32-S2, ESP32-S3, ESP32-C3, ESP32-C6, ...)
- Supports MQTT over TCP and MQTT over websocket
- Full support for SSL/TSL encryption - for both MQTT over TCP and MQTT over WS
- No limitation in buffer size for transmit and receive messages. Multipart messages are reassembled.
- Fully asynchronous and non-blocking
- No dependencies
- Flexible and powerful event-based API
  - `onTopic()` event which calls a callback every time a message on a specific subscribed topic is received
- Handles reconnects automatically

## Usage

## Important

- No blocking code inside event handler functions. These must return fast. Especially no `delay()`, hardware functions or any other blocking code. Store the results and notify your main loop to take actions. If applicable use FreeRTOS toolings like queues or semaphores to protect against race conditions.
- The preferred method to take action on received messages is the `onTopic()` event handler. It takes care of subscriptions automatically.

## Advanced Usage and SSL/TLS

### Single CA Root Certificate

### CA Root Certificate Bundles

#### PIO Build System

#### Attach X509 Certificate Bundle

#### Use ESP32 Global Certificate Store

## License

MIT License

Copyright (c) 2024 elims

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
