# amcrest2mqtt

A simple app to expose all events generated by an Amcrest device to MQTT using the
[`python-amcrest`](https://github.com/tchellomello/python-amcrest) library.

It supports the following environment variables:

- `AMCREST_HOST` (required)
- `AMCREST_PORT` (optional, default = 80)
- `AMCREST_USERNAME` (optional, default = admin)
- `AMCREST_PASSWORD` (required)
- `MQTT_USERNAME` (required)
- `MQTT_PASSWORD` (optional, default = empty password)
- `MQTT_HOST` (optional, default = 'localhost')
- `MQTT_QOS` (optional, default = 0)
- `MQTT_PORT` (optional, default = 1883)
- `HOME_ASSISTANT` (optional, default = false)
- `HOME_ASSISTANT_PREFIX` (optional, default = 'homeassistant')

It exposes events to the `amcrest2mqtt/[SERIAL_NUMBER]/event` MQTT topic. If the device is an AD110 doorbell it will expose the
doorbell status to `amcrest2mqtt/[SERIAL_NUMBER]/doorbell`. If the device supports motion events it will expose motion events
to `amcrest2mqtt/[SERIAL_NUMBER]/motion`.

## Device Support

The app supports events for any Amcrest device supported by [`python-amcrest`](https://github.com/tchellomello/python-amcrest).

## Home Assistant

The app has built-in support for Home Assistant discovery. Set the `HOME_ASSISTANT` environment variable to `true` to enable support.
If you are using a different MQTT prefix to the default, you will need to set the `HOME_ASSISTANT_PREFIX` environment variable.

## Running the app

The easiest way to run the app is via Docker Compose, e.g.

```
version: "3"
services:
  amcrest2mqtt:
    container_name: amcrest2mqtt
    image: dchesterton/amcrest2mqtt:latest
    restart: unless-stopped
    environment:
      AMCREST_HOST: 192.168.0.1
      AMCREST_PASSWORD: password
      MQTT_HOST: 192.168.0.2
      MQTT_USERNAME: admin
      MQTT_PASSWORD: password
      HOME_ASSISTANT: "true"
```

## Out of Scope

### Multiple Devices

The app will not support multiple devices. You can run multiple instances of the app if you need to expose events for multiple devies.

### Non-Docker Environments

Docker is the only supported way of deploying the application. The app should run directly via Python but this is not supported.