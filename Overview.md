# Velocio Protocol Overview

The Velocio USB Protocol appears to be designed by a programmer with a background in Modbus and PLC systems.
It incorporates several Modbus concepts such as headers, message byte counters, and function codes.
Similar to Modbus, it uses a client polling-server model, with the PLC being the Server and vBuilder or vFactory being the client.

The protocol exhibits a notable preference for ASCII control characters.
For instance, the Acknowledgment Control Character (0x06) is commonly used as distinction between client and server messages.
Other ASCII control characters, such as Form Feed (0x0C), are also used.
Specifically, two Form Feeds (`FFFF`) signal the end of a string within the Message Data section.

## Message Structure

The Velocio USB Protocol message is structured as follows:

| Protocol Header | Message Byte Size | Function Code | Response Indicator | Data Field  |
| :-------------: | :---------------: | :-----------: | :----------------: | :---------: |
|     3 Bytes     |      3 Bytes      |    2 Bytes    |      2 Bytes       | N x 2 Bytes |

### Protocol Header

The Protocol Header always uses `56 FF FF`, which translates to an uppercase 'V'.
In the context of this protocol, `FF FF` signifies the end of a single ASCII character string.

### Message Byte Size

The Message Byte Size is a 6-byte unsigned integer representing the total number of bytes in the message.
This includes all bytes of the message: the Protocol Header, the Message Byte Size itself, and any additional data.

### Function Codes

The Velocio USB Protocol utilizes function codes inspired by Modbus.
Traditional function codes for basic operations, such as reading and writing tag values.

Velocio appears to also be inspired by Schneider Electric's recent expansion of Modbus by developing programming function codes that trigger firmware processes.
These function codes are used to for the actual programming of the PLC.
They are used to instruct the PLC to create Main Routines, Sub Routines, Tags, Services, Calibration, and  Debugging controls.

### Response Indicator

The Response Indicator field is used to signal if the message is from the client or from the server.
Only messages from the PLC, server, include the 0x06 Response Indictor.

### Data Field

The Data field contains the actual information or parameters for the querry, command, or response.
It is structured as N x 2 Bytes, where N varries to include the number of data elements.

## Procotol Example

Below is an example of a Velocio USB Protocol used to set the PLC into Stop Mode:

``` Hex
56 ff ff 00 07 f1 02
56 ff ff 00 08 f1 06 02
```

## Client Message Breakdown

- **Protocol Header**: `0x56FFFF`
- **Message Byte Size**: `0x0007`
- **Function Code**: `0xf1`
- **Data**: `0x02`

## Server Message Breakdown

- **Protocol Header**: `0x56FFFF`
- **Message Byte Size**: `0x0008`
- **Function Code**: `0xf1`
- **Response Indication**: `0x06`
- **Data**: `0x02`

## Summary

The Velocio USB Protocol combines Modbus-inspired concepts with custom features, including a significant use of ASCII control characters.
It operates on a client polling-server model.
The protocol’s unique characteristics, such as the use of Acknowledgment Control Character (0x06) and Form Feed (0x0C), play a crucial role in message formatting and communication.
