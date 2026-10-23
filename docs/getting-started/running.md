---
title: Running MQTTSubscribe with WeeWX
parent: Getting Started
nav_order: 6
---

## Running MQTTSubscribe with WeeWX

Follow the local [configuration workflow](../readme.md#configure-mqttsubscribe)
after installing the fixed archive. For driver mode, initialize the shell
variables described there, then select the installed lowercase module:

```sh
$WEECTL station reconfigure --driver=user.mqttsubscribe --config "$CONFIG_FILE" --no-prompt
```

For service mode, keep your station driver and
[enable the service](../readme.md#if-running-as-a-service-enable-it).
Then [import the configuration](../readme.md#update-weewxconf) and restart WeeWX.
