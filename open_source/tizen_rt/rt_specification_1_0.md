# Tizen RT 1.0 Specification

## IoT Data Management

### 1 File System Support

Tizen RT supports not only a lightweight file system called SmartFS but also a virtual file system (VFS). The VFS can provide a common interface set in the form of the POSIX API. Standard libc APIs are already supported. In addition, some advanced features are also included:


### 2 Database (AraStorage) Support

A lightweight database called AraStorage is ready to manipulate collected sensor data with SQL-compatible interfaces. AraStorage also provides some advanced features, such as:


## Device Management

Tizen RT incorporates the OMA-based Lightweight M2M (LWM2M) protocol for device management. LWM2M is an application layer communication protocol between an LWM2M server and an LWM2M client that typically resides on a resource-constrained device. The LWM2M protocol can be briefly described in the context of its interface and stack design:

### 1 LWM2M Interfaces

- Bootstrap: The LWM2M client obtains information about the LWM2M server from the LWM2M bootstrap server. The LWM2M bootstrap server holds the credentials of all registered LWM2M servers, and can provide the details either on request (client-initiated bootstrap), or explicitly (server-initiated bootstrap). At present, client-initiated bootstrap is already developed and verified. Server-initiated bootstrapping will also be included.
- Registering: Once the LWM2M client has the necessary information about an LWM2M server, it opens a connection to the server and registers itself as an LWM2M device. Registering, in this context, also includes providing details of the LWM2M objects that the client has created during initialization.
- Device management and service enablement: The LWM2M server uses this interface for query (read and discover), modify (write), and execute operations, as well as for creating and deleting attributes on a registered LWM2M client object. While basic read operations are currently verified, support for other operations will be provided later.
- Information reporting: The LWM2M server uses this interface to periodically query the registered LWM2M client to send updates on an object attribute (sensor value or communication signal strength). Alternatively, the LWM2M server can also direct the LWM2M client to notify it about changes in attribute values over a programmable time interval. The LWM2M protocol allows the LWM2M server to specify the observation time interval, and the range of permissible attribute values, beyond which a notification must be carried out.

### 2 LWM2M Protocol Stack

- CoAP protocol: The LWM2M entities (client, server, and bootstrap server) use the CoAP (Constrained Application Protocol) for implementing the 4 LWM2M interfaces described above. CoAP offers the advantage of an efficient payload structure, which is necessary for resource-constrained client devices. In this context, LWM2M-based request and response messages are mapped on to appropriate CoAP methods (such as GET, PUT, POST), as described in [RFC 7252](https://tools.ietf.org/html/rfc7252).
- DTLS security: While LWM2M optionally functions in a 'NoSec' mode (no security), it also allows the use of DTLS (Datagram Transport Layer Security) to ensure authentication, data confidentiality, and integrity between an LWM2M client and an LWM2M server or bootstrap server. The LWM2M client has the option of bootstrapping with a pre-shared secret, or with public certificates (either raw public keys or X.509v3). In all cases, the LWM2M client must possess a unique key for securing communication with the LWM2M bootstrap server and the LWM2M server. These features will be supported on Tizen RT.
- UDP: LWM2M allows the use of both UDP and SMS protocol for communication between a client and server. The lightweight M2M component in Tizen RT uses the UDP binding over port 5683. Reliability over UDP is achieved by the retransmission mechanism of CoAP.

## IP Network

For TCP, UDP, and IPv4 protocols, LWIP is already ported on Tizen RT and successfully verified. Regarding IPv6, uIP-based stack is already implemented and is granted IPv6 Ready Logo from IPv6 Forum (go to [https://www.ipv6ready.org/db/index.php/public/?o=4](https://www.ipv6ready.org/db/index.php/public/?o=4) and search with the **Tizen **keyword). By the way, a common base for a network stack is needed in order to increase maintainability. Both IPv4 and IPv6 based on the common project (such as LWIP or uIP) will be released.

Transition between IPv4 and IPv6 is also required. Suppose that sensor devices are equipped with only IPv6 over IEEE 802.15.4 or IPSP/BLE. These devices are required to connect to Internet directly or through hubs or other IoT devices. Tizen RT is getting ready to fit into these relaying IoT devices by implementing the transition functions between IPv4 and IPv6.

## IoTivity

Tizen RT 1.0 supports IoTivity 1.1.0 for now. The IoTivity support is updated to 1.2 in the next release.

### 1 IoTivity 1.2 Base Layer Support (OCF 1.0 Base Layer Ready)

Tizen RT supports the IoTivity base layer for constrained-device communication in the IoT world. It supports IoTivity 1.2 release as base code with the OCF 1.0 spec ready.

IP transport (TCP/UDP over Wi-Fi) is currently supported in the Connectivity Abstraction (CA) layer.

**Figure: IoTivity base layer architecture**

[![img](https://source.tizen.org/sites/default/files/resize/images/iotivity_baseline_architecture-700x245.png)](https://source.tizen.org/sites/default/files/images/iotivity_baseline_architecture.png)

The above figure shows the IoTivity base layer architecture. The architecture can be divided into discovery, messaging, and security modules. The following table describes the IoTivity 1.2 features of Tizen RT.

**Table: Iotivity features**

| MODULE (BASE LAYER) | FEATURE                              | DESCRIPTION                              |
| ------------------- | ------------------------------------ | ---------------------------------------- |
| Discovery           | Multicast Discovery, Device Presence | Discovers resources, checks the device presence. |
|                     | Resource Introspection               | Manages resource types and properties.   |
| Messaging           | CoAP Messaging                       | Transmits messages between devices.      |
|                     | Message switching                    | Tizen RT does not support message switching. |
|                     | Connectivity Abstraction             | Supports Wi-Fi transport currently. Future support for, for example, BT, BLE, and NFC. |
|                     | Block-wise transfer                  | Provides block data transfer (more than 1 KB data). |
|                     | CoAP over TCP                        | Provides reliable transmission. It can be used for messaging between a device and cloud. |
| Security            | DTLS                                 | Provides a secure channel with data encryption for UDP. |
|                     | Security Resource Manager            | Provides an access control mechanism.    |
|                     | Security Provisioning Manager        | Transmits credentials for authentication. |

### 2 Feature support in IoTivity 1.2

Tizen RT supports:


## IoTBus Framework

IoTBus Framework supports system I/O APIs, including the following 5 categories:

### 1 GPIO (General Purpose Input/Output)

Provides functions to control generic pins. They can be configured to be input or output, because GPIO pins have no predefined purpose.

### 2 I2C (Inter Integrated Circuit)

Provides functions to read values of I2C devices or write commands to I2C devices. These APIs are typically used to connect sensor devices or for intra-board communications.

### 3 SPI (Serial Peripheral Interface)

Provides functions to communicate with SPI devices. These APIs support synchronous serial communication interfaces used for short distance communication. Full duplex modes using a master-slave architecture with a single master are also served.

### 4 PWM (Pulse Width Modulation)

Provides functions to get and set duty cycles and periods of PWM devices. These APIs are typically used to control servo motors or LEDs.

### 5 UART (Universal Asynchronous Receiver/Transmitter)

Provides functions to read and write for asynchronous serial communication. These APIs are usually used in conjunction with communication standards, such as RS-232, RS-422, and RS-485.

## Device Management Framework

The following features will be made available under the Device Management Framework:

### 1 Configuration

The LWM2M client is configured with a set of parameters that include the LWM2M bootstrap server address, bootstrap server port, and LWM2M session lifetime. Alternatively, if a direct connection to the LWM2M server is preferred, the client is configured with the LWM2M server address and port information.

### 2 Temporary Halt and Resumption

Wireless links, especially in indoor deployments, are prone to intermittent failures, and can momentarily halt an ongoing LWM2M session. Taking this into account, the Device Management Framework must gracefully close all LWM2M sessions with their respective servers, and also logically resume the sessions once the wireless link is restored.

### 3 Support for Multiple Servers

The LWM2M specification allows multiple servers to perform device management with a registered LWM2M client. To this end, the framework must facilitate the seamless addition of LWM2M server information.

### 4 Device Management Services

Services provided under Device Management can be broadly categorized into 4 types: 



Tizen RT 1.0 supports the first 3 service types. The S/W update is included in the next version.