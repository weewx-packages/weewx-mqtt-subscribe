---
title: Invocation
parent: Parser Mode
nav_order: 2
---

## Invocation

First [initialize the station paths and run mode](../readme.md#initialize-mqttsubscribes-runtime).
Use the Python environment running WeeWX.

```
PYTHONPATH="$WEEWX_ROOT/bin${PYTHONPATH:+:$PYTHONPATH}" python3 -m user.mqttsubscribe parse $RUN_MODE --help
```
