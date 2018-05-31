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
name: Background
layout: true

The problem at hand
=========

---
Several controlsystems (Tango databases)
* Accelerator and storage rings ~ 10 000 devices
* One per beamline ~ 700 devices x 15
* Other systems such as testlabs ~ 500 devices each

Total number of Tango devices at MAX IV > 20 000

Multiple sources of information from the different subsystems that make up a synchrotron:
* Magnets
 * various xls-files
* Vacuum and Diagnostics
 * Allen Bradley PLC tag lists in csv format
 * Cable database 
* Motorization
 * yet another xls file


---

name: Problem
layout: true

Necessity is the mother of invention
=========

---
###We need to:
* Keep our configuration consistent and correct
* Keep up with demands for updates and changes
* Keep track of changes

###How do we do it?
* Ensure consistency of source files
 * MAXIV naming convention is common ground
 * Track versions
* Conversion tools for each type of source file
* Intermediate format that can be used by a
* Simple deployment tool that also allows us to
* Track our configuration

---

name:
layout: true


A comfortable situation?
=========

---
The current state
----
* Source files are delivered to us with versioning through Alfresco
* Beamline installation process is well defined
* Updates are applied in a safe and predictable manner 

Plans
----
* Web interface to allow subsystem owners to check their source files
* Add conversion tools for even more subsystems
* Allow some subsystems owners to apply their own updates 
 
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
=======

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
name: Getting to the Json
layout: true

Getting to the Json - worst and best case
=========

---

Magnet Configuration
-----
The magnet properties are converted to the json file format by a script that takes a total of four input sources: 
1. A lattice file that contains all the magnets in the Linac, their type and length 
2. A magnet to power supply mapping file (xls)
3. The xls sheet of calibration data  
4. A sheet of alarm interlock tags from the PLC 


PLC-controlled Devices
-----
Facade-devices based on groups of PLC-tags are generated from jinja-templates with a script that takes a CSV-file dumped directly from the PLC program as input. All tag names follow the MAXIV naming convention.
