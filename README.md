# WeeWX MQTTSubscribe

MQTTSubscribe receives observations from MQTT topics as JSON, keyword payloads
such as `field=value`, or individual values. Use it as a service to supplement
a station driver, or as the station driver when MQTT supplies the observations.
Field mappings support conversion, aggregation and expiring caches.

This repository mirrors [weewx-mqtt/subscribe](https://github.com/weewx-mqtt/subscribe).
Use the separately repaired catalog archive `3.2.1rc1.post20261022` below.
A source-branch ZIP does not include those packaging repairs. The catalog is a prerelease based on upstream `3.2.1-rc01a`.
Its suffix normalizes the installer version; runtime code is unchanged.

## Install

Use WeeWX 5.5–5.x and Python 3.10–3.14 for the catalog archive. Install the Paho
client in WeeWX's Python environment:

```sh
python -m pip install paho-mqtt
```

Install the published archive with WeeWX 5's extension installer:

```sh
weectl extension install https://github.com/weewx-packages/catalog/releases/download/mqtt-subscribe/v3.2.1rc1.post20261022/mqtt-subscribe-3.2.1rc1.post20261022.tar.gz
```

Use the `weectl` from your WeeWX environment. Add
`--config /path/to/weewx.conf` when the station configuration is not found
automatically. Package installations may require `sudo`; activate the WeeWX
virtual environment for pip installations. Restart your WeeWX service after
configuration, for example `sudo systemctl restart weewx` on systemd.
The installer does not restart it for you.

The installer copies `bin/user/mqttsubscribe.py`, registers
`user.mqttsubscribe.MQTTSubscribeService` in `data_services`, and creates driver
and service sections. The service starts disabled. Keep your existing driver
when using the service; do not run both modes for the same observations.

## Configure a service

This example consumes JSON such as `{"outTemp": 18.5}` from your local broker.
Values use `METRICWX` units. Change the topic, host, units and fields to match
your publisher:

```ini
[MQTTSubscribeService]
    enable = true
    host = localhost
    port = 1883
    binding = loop
    [[topics]]
        unit_system = METRICWX
        [[[weather/observations]]]
            [[[[message]]]]
                type = json
```

To generate the complete example or validate a configuration without connecting
to a broker, use the installed module. Replace `/path/to/station/bin` with the
parent of the station's `user` directory:

```sh
PYTHONPATH=/path/to/station/bin python -m user.mqttsubscribe configure --create-example mqttsubscribe.template.conf
PYTHONPATH=/path/to/station/bin python -m user.mqttsubscribe configure service --validate --conf mqttsubscribe.template.conf
```

Edit the generated example first; its placeholder topic and message type need
values. Simulation mode connects to the configured broker, so use it only when
you intend to make that connection. See the [configuration guide](docs/readme.md)
for importing a template into `weewx.conf`, driver setup, TLS, and diagnostics.
Restart WeeWX after applying configuration and inspect its log.

MQTT supplies readings only while the publisher, broker and network work.
Set cache expiry and aggregation to match the publisher's timing. Extra fields
need matching archive columns if they should be stored historically.

## Upgrade, removal and license

Back up `weewx.conf`, install the desired fixed archive and validate the current
configuration. The installed module is lowercase `user.mqttsubscribe`.
The installer removes deprecated uppercase service references. Remove with
`weectl extension uninstall MQTTSubscribe` and restart WeeWX.

Rich Bell's project retains its [GPLv3 license](LICENSE). The
[upstream reference](https://weewx-mqtt.github.io/subscribe/) covers all options;
use the installation commands in this repository for the catalog distribution.
