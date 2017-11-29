---
layout: post
title:  "Memo for CANopen messages"
author: "WillyKaze"
date:   2016-11-23 16:24:31 +0100
thumbnail: sticky-note
categories: Memo
tags: CANopen protocol
---
This is a quick memo for CANopen message types.

# Message types

## Network Management messages

NMT messages are used to notify and change node status on CAN bus.
At startup, each node send a Boot-up message with its node ID.

Nodes have operational modes that can be switched either automatically or by the network management node. The operational modes are:

- Pre operational mode — Nodes enter this mode if boot-up of node and CAN bus are ok.
- Operational mode — The normal mode when nodes exchange data.
- Stopped mode — Mode activated either by the network manager node or if the node encounter a fatal error.

After a change of operational mode, node must notify by sending a ###NMT message### of their operational mode.

## Service Data Object messages

SDO messages are used to exchange data between nodes. They are available in pre operational and operational modes.
They can be used to configure [OD][Object Data] entries in order to setup PDO data exchange.
Although they can be used to exchange data, the request -> response communication could lead to heavy bandwidth usage on the CAN bus.

## Process Data Object messages

PDO messages are used to exchange data. Related OD entries can be configured to send data periodically, on request or in sync.
The data type of PDO messages is defined in pre operational mode. PDO messages are only transmitted when the node is in operational mode.
This is the most reliable way to exchange data in CANopen as the data payload can be precisely configured in a constant way.

## Sync messages

SYNC messages are used to trigger data exchange of PDOs.

## Emergency messages

Emergency messages are used to notify the network when a error raises.

* * *

Table of different message types in CANopen

| COB-ID          | Message Type     | Description                    |
|:----------------|:-----------------|:-------------------------------|
|`00h`            |NMT               |Network management              |
|`700h + $ID`     |NMT Error control |Network management error control|
|`700h + $ID`     |Boot-up           |Boot-up message                 |
|`80h`            |SYNC              |Synchronization message         |
|`80h + $ID`      |Emergency         |Emergency message               |
|`180h—500h + $ID`|PDO               |Process Data Object message     |
|`580h—600h + $ID`|SDO               |Service Data Object message     |

# Message details

## NMT messages

| COB-ID | Data bytes | Description                       |
|:-------|:-----------|:----------------------------------|
|`00h`   |`01h $ID`   |Switch to operational mode         |
|`00h`   |`02h $ID`   |Switch to stopped mode             |
|`00h`   |`80h $ID`   |Switch to pre operational mode     |
|`00h`   |`81h $ID`   |Reset the node                     |
|`00h`   |`82h $ID`   |Reset the communication on node $ID|


# SDO messages

SDO messages are available in pre-operationnal and operationnal modes.

## 4 or less bytes to transmit

| COB-ID     | Data bytes                        | Description           |
|:-----------|:----------------------------------|:----------------------|
|`600h + $ID`|`40h $OLSB $OMSB $SI 4x00h`|SDO client read command|
|`580h + $ID`|`$RP $OLSB $OMSB $SI 4x$DB`|SDO server reply       |

`$OLSB` and `$OMSB` are the object index LSB and MSB.
`$SI` is the subindex.

`$RP` is the response code indicating either the data length or the error code:

* `$RP` is `4Fh` if data is 1 byte long
* `$RP` is `4Bh` if data is 2 bytes long
* `$RP` is `43h` if data is 4 bytes long
* Error codes are summarized at the end of this section.  
  In case of an error code, the last 4 bytes provides a detailed error.

## More than 4 bytes to transmit

| COB-ID     | Data bytes                |        Description           |
|:-----------|:--------------------------|:-----------------------------|
|`600h + $ID`|`40h $OLSB $OMSB $SI 4x00h`|SDO client read command       |
|`580h + $ID`|`$RP $OLSB $OMSB $SI $DL`  |SDO server data length reply  |
|`600h + $ID`|`60h 7x00h`                |SDO client ready reply        |
|`580h + $ID`|`$RP 7x$DB`                |SDO server data reply         |

## Error codes for SDO access

| Error code | Description           |
|:-----------|:----------------------|
|`0503 0000h`| Toggle bit not alternated |
|`0504 0000h`| SDO protocol timed out |
|`0504 0001h`| Command specifier not valid |
|`0504 0002h`| Invalid block size (block mode only, see DS301) |
|`0504 0003h`| Invalid sequence number (block mode only, see DS301) |
|`0504 0004h`| CRC error (block mode only, see DS301) |
|`0504 0005h`| Out of memory |
|`0601 0000h`| Unsupported access to an object |
|`0601 0001h`| Attempt to read a write only object |
|`0601 0002h`| Attempt to write a read only object |
|`0602 0000h`| Object does not exist in the object dictionary |
|`0604 0041h`| Object cannot be mapped to the PDO |
|`0604 0042h`| The number and length of the objects to be mapped would exceed PDO length |
|`0604 0043h`| General parameter incompatibility reason |
|`0604 0047h`| General internal incompatibility in the device |
|`0606 0000h`| Access failed due to a hardware error |
|`0607 0010h`| Data type does not match, length of service parameter does not match |
|`0607 0012h`| Data type does not match, length of service parameter too high |
|`0607 0013h`| Data type does not match, length of service parameter too low |
|`0609 0011h`| Sub-index does not exist |
|`0609 0030h`| Value range of parameter exceeded (only for write access) |
|`0609 0031h`| Value of parameter written too high |
|`0609 0032h`| Value of parameter written too low |
|`0609 0036h`| Maximum value is less than minimum value |
|`0800 0000h`| General error |
|`0800 0020h`| Data cannot be transferre d or stored to the application  |
|`0800 0021h`| Data cannot be transferred or stored to the application because of local control |
|`0800 0022h`| Data cannot be transferred or stored to the application because of present device state |
|`0800 0023h`| Object dictionary dynamic generation fails or no ob ject dictionary is present (object dictionary loads from file and file error occurred) |

# Resources

Here some useful resources:

[http://www.a-m-c.com/download/sw/dw300_3-0-3/CAN_Manual300_3-0-3.pdf]()
