# zigate
python library for zigate http://zigate.fr/


This library manage communication between python and zigate key, both USB and WiFi key are supported (wifi is almost untested)


Usage :

```
# if you want logging
import logging
logging.basicConfig()
logging.root.setLevel(logging.DEBUG)

import zigate
z = zigate.ZiGate(port=None) # Leave None to auto-discover the port

>>> print(z.get_version())
OrderedDict([('major', 1), ('installer', '30b'), ('rssi', 0), ('version', '3.0b')])

>>> print(z.get_version_text())
3.0b

# refresh devices list
z.get_devices_list()

# start inclusion mode
>>> z.permit_join()
>>> z.is_permitting_join()
True

# list devices
>>> z.devices
[Device 677c , Device b8ce , Device 92a7 , Device 59ef ]
>>> z.devices[0].addr
'677c'

# get all discovered endpoints
>>> z.devices[0].endpoints
{1: {'0405_0000': {'endpoint': 1,
   'data': 3997,
   'cluster': 1029,
   'attribute': 0,
   'status': 0,
   'friendly_name': 'humidity',
   'value': 39.97,
   'unit': '%'},
  '0000_0005': {'endpoint': 1,
   'data': '6c756d692e77656174686572',
   'cluster': 0,
   'attribute': 5,
   'status': 0,
   'friendly_name': 'type',
   'value': 'lumi.weather',
   'unit': ''},
  '0000_0001': {'cluster': 0,
   'endpoint': 1,
   'status': 0,
   'attribute': 1,
   'data': 3},
  '0403_0000': {'endpoint': 1,
   'data': 976,
   'cluster': 1027,
   'attribute': 0,
   'status': 0,
   'friendly_name': 'pressure',
   'value': 976,
   'unit': 'mb'},
  '0000_ff01': {'cluster': 0,
   'endpoint': 1,
   'status': 0,
   'attribute': 65281,
   'data': '0121ef0b0421a81305210800062401000000006429a6096521a30f662b737d01000a210000'},
  '0403_0010': {'endpoint': 1,
   'data': 9762,
   'cluster': 1027,
   'attribute': 16,
   'status': 0,
   'friendly_name': 'detailled pressure',
   'value': 976.2,
   'unit': 'mb'},
  '0403_0014': {'cluster': 1027,
   'endpoint': 1,
   'status': 0,
   'attribute': 20,
   'data': -1},
  '0402_0000': {'endpoint': 1,
   'data': 2447,
   'cluster': 1026,
   'attribute': 0,
   'status': 0,
   'friendly_name': 'temperature',
   'value': 24.47,
   'unit': '°C'}}}


get well known attributes
>>> for attribute in z.devices[0].properties:
    	print(attribute)
{'endpoint': 1, 'data': 3997, 'cluster': 1029, 'attribute': 0, 'status': 0, 'friendly_name': 'humidity', 'value': 39.97, 'unit': '%'}
{'endpoint': 1, 'data': '6c756d692e77656174686572', 'cluster': 0, 'attribute': 5, 'status': 0, 'friendly_name': 'type', 'value': 'lumi.weather', 'unit': ''}
{'endpoint': 1, 'data': 976, 'cluster': 1027, 'attribute': 0, 'status': 0, 'friendly_name': 'pressure', 'value': 976, 'unit': 'mb'}
{'endpoint': 1, 'data': 9762, 'cluster': 1027, 'attribute': 16, 'status': 0, 'friendly_name': 'detailled pressure', 'value': 976.2, 'unit': 'mb'}
{'endpoint': 1, 'data': 2447, 'cluster': 1026, 'attribute': 0, 'status': 0, 'friendly_name': 'temperature', 'value': 24.47, 'unit': '°C'}

# get specific property
>>> z.devices[0].get_property('temperature')
{'endpoint': 1,
 'data': 2447,
 'cluster': 1026,
 'attribute': 0,
 'status': 0,
 'friendly_name': 'temperature',
 'value': 24.47,
 'unit': '°C'}
 

```

You can use callback to catch some events

```
def my_callback(event, **kwargs):
	print(event)
	print(kwargs)

z = ZiGate(callback=my_callback)
```

event can be :
zigate.ZGT_CMD_NEW_DEVICE = 'new_device'
zigate.ZGT_CMD_DEVICE_UPDATE = 'device_update'
zigate.ZGT_CMD_REMOVE_DEVICE = 'remove_device'

kwargs depends of the event type
for zigate.ZGT_CMD_NEW_DEVICE:
kwargs contains device

for zigate.ZGT_CMD_DEVICE_UPDATE
kwargs contains device
and attribute updated (if applicable)

for zigate.ZGT_CMD_REMOVE_DEVICE
kwargs contains addr (the device short address) 



Wifi ZiGate:

```
import zigate
z = zigate.ZiGateWiFi(host='192.168.0.10', port=9999)
print(z.get_version())

# list devices
z.devices

# start inclusion mode
z.permit_join()
```

