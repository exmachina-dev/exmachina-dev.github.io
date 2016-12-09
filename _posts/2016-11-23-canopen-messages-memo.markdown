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
|`600h + $ID`|`40h $O_LSB $O_MSB $SubIndex 4x00h`|SDO client read command|
|`580h + $ID`|`$RP $O_LSB $O_MSB $SubIndex 4x$DB`|SDO server reply       |

`$O_LSB` and `$O_MSB` are the object index Less Significant Byte and Most Significant Byte

`$RP` is the response code indicating either the data length or the error code:

* `$RP` is `4Fh` if data is 1 byte long
* `$RP` is `4Bh` if data is 2 bytes long
* `$RP` is `43h` if data is 4 bytes long
* Error codes are summarized at the end of this section.  
  In case of an error code, the last 4 bytes provides a detailed error.

## More than 4 bytes to transmit

| COB-ID     | Data bytes                       | Description           |
|:-----------|:---------------------------------|:----------------------|
|`600h + $ID`|`40h $OBJindex(2) $SubIndex 4x00h`|SDO client read command|
|`580h + $ID`|`$RP $ID`                         |SDO server reply       |

## Error codes for SDO access

| Error code | Description           |
|:-----------|:----------------------|
|`60h`       |ERROR|
|`60h`       |ERROR|
