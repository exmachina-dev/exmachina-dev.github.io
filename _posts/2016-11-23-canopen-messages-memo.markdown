---
layout: post
title:  "Memo for CANopen messages"
author: "WillyKaze"
date:   2016-11-23 16:24:31 +0100
thumbnail: sticky-note
tags: CANopen protocol
---
This is a quick memo for CANopen message types.

# Message types

| COB-ID | Message Type | Description |
|:-------|:-------------|:------------|
|`00h`|NMT|Network management|
|`700h + $ID`|NMT Error control|Network management error control|
|`700h + $ID`|Boot-up|Boot-up message|
|`80h`|SYNC|Synchronization message|
|`80h + $ID`|Emergency|Emergency message|
|`180h—500h + $ID`|PDO|Process Data Object message|
|`580h—600h + $ID`|SDO|Service Data Object message|


# NMT messages

| COB-ID | Data bytes | Description |
|:------:|:-------------|:------------|
|`00h`|`01h $ID`|Switch to operationnal mode|
|`00h`|`02h $ID`|Switch to stopped mode|
|`00h`|`80h $ID`|Switch to pre-operationnal mode|
|`00h`|`81h $ID`|Reset the node|
|`00h`|`82h $ID`|Reset the communication on node $ID|


# SDO messages

SDO messages are available in pre-operationnal and operationnal modes.

## 4 or less bytes to transmit

| COB-ID | Data bytes | Description |
|:-------|:-------------|:------------|
|`600h + $ID`|`40h $O_LSB $O_MSB $SubIndex 4x00h`|SDO client read command|
|`580h + $ID`|`$RP $ID`|SDO server reply|

`$O_LSB` and `$O_MSB` are the object index Less Significant Byte and Most Significant Byte

* `$RP` is `4Fh` if data is 1 byte long
* `$RP` is `4Bh` if data is 2 bytes long
* `$RP` is `43h` if data is 4 bytes long

## more than 4 bytes to transmit

| COB-ID | Data bytes | Description |
|:------:|:-------------|:------------|
|`600h + $ID`|`40h $OBJindex(2) $SubIndex 4x00h`|SDO client read command|
|`580h + $ID`|`02h $ID`|Switch to stopped mode|
