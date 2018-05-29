name: empty layout
layout: true

---
name: title
class: center, middle

Title
========================

Author

Date


---
---

name: Dsconfig 
layout: true

The Json format
=========

---

Close to the jive server view:

```json
    "servers": {
        "some-server/instance": {
            "SomeDeviceClass": {
                "some/device/1": {
                    "properties": {
                        "someImportantProperty": [
                            "foo",
                            "bar"
                        ],
                        "otherProperty": ["7"]
                    },
                    "attribute_properties": {
                        "anAttribute": {
                            "min_value": ["-5"],
                            "unit": ["mV"]
                        }
                    }
                }
            }
        }
    },
```
---

name: Dsconfig 
layout: true

Deployment
=========

---


```bash
 antdup@w-v-kitslab-cc-0: json2tango someconfig.json 
+ Device: some/device/1
  Server: some-server/instance
  Class: SomeDeviceClass
  Properties:
    + otherProperty
      7
    + someImportantProperty
      foo
      bar
  Attribute properties:
    anAttribute
      + unit
        mV


Summary:
Add 1 servers.
Add 1 devices to 1 servers.
Add/change 2 device properties in 1 devices.
Add/change 2 device attribute properties in 1 devices.


```
---

name: Dsconfig 
layout: true

Renaming
=========

---



```bash
- Device: some/device/1
  Server: some-server/instance
  Class: SomeDeviceClass

+ Device: some/device/2
  Server: some-server/instance
  Class: SomeDeviceClass
  Properties:
    + otherProperty
      7
    + someImportantProperty
      foo
      bar
  Attribute properties:
    anAttribute
      + unit
        mV


Summary:
Add 1 devices to 1 servers.
Delete 1 devices.
Add/change 2 device properties in 1 devices.
Add/change 2 device attribute properties in 1 devices.
```
---

name: Dsconfig 
layout: true

Editing
=========

---



```bash
= Device: some/device/1
  Properties:
    + extraProperty
      89
    = otherProperty
      - 7
      + 8


Summary:
Add/change 2 device properties in 1 devices.



```

---

