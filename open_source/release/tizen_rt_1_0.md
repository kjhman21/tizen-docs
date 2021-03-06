# Tizen RT 1.0

Release Date: 23 Dec, 2016

## Release Notes

**New and Changed Features**

- Architecture
  - ARM
    - Cortex-R4
      - SIDK-S5JT200
- Kernel
  - FIFO / RoundRobin Scheduling and Interrupt
    - Fully Preemptible
  - Task and Pthread
  - IPC
    - Message Queue
    - Signal
    - Semaphore, Mutex
  - Clock, Timer, Systick
  - Work Queue
  - Environment
  - Number of last error (errno)
  - Power Management
  - Dynamic Memory Management
  - Debug
    - Stackmonitor
    - Heap analyzer
      - Allocated heap size (total and per thread)
      - Address calling mm API
    - Ramdump / Coredump
  - Logger Module
  - System logging (Syslog)
  - Shell tools: date, dmesg, getenv, free, heapinfo, kill / killall, ps, setenv, stkmon, unsetenv
- Device Driver
  - GPIO, I2C, UART, PWM and Pipe
    - Controlled by file operation
    - SPI
    - USB CDC/ACM
- File System
  - Bad Sector Management
    - File System checks the bad sectors during write operation and maintains list of bad sectors inside of the physical flash.
  - Dynamic Header
    - A technique to reuse a used sector of smartfs throughout all write operation into that sector without relocation (smartfs reallocates sectors when a file is rewritten).
  - Sector Recovery
    - A tool to recover the sectors lost in case of power failure (smartfs only)
  - Journaling
    - Added Transaction-Logging based Journaling Feature
    - It takes several sectors based on number of file descriptors.
- AraStorage

  - Query Interface
    - SQL like query interface exposed: Simple and Standard  relations
    - Wrapper for AQL command input to the relational query parser
    - Readable, interactive, expandable form constructs for specifying search constraints and AQL object attributes
  - Index Algorithm

    - Optimizes the execution of database queries by delimiting the set of tuples that must be processed.

      - Inline: Useful for Ordered Data sets
      - B+ based Index: Useful for MASS Data sets
  - Storage Abstraction

    - Use a basic POSIX APIs in VFS to support Database operations.
  - Cursor

    - Added a cursor structure to allow easier use of arastorage.
- Network and Connectivity
  - LWIP
    - LWIP 1.4.1 release code is taken as the base network stack for TinyAra.
    - Support of IPv4, TCP, UDP, ICMP, ARP, IGMP , DHCPc & Generic sockets are provided.
    - Missing POSIX API support for all the network API's is enabled in TinyAra.TinyAra OS compatibility and performance improvements done.
    - Fixed in the LWIP stack to give desired result on applications, such as DHCP, DNS, HTTP, IoTivity, TELNET, and WEBSOCKETS, is completed.
    - Samples for tcp/UDP for unicast/multicast testing and stress testing are provided.
    - LWIP stack is verified over performance tool iperf to measure the reliability and throughput of the stack.
    - Added Arp retransmission and updated arp timer.
    - Socket structures belong to task (not global variables). So when the task is terminated, the socket is also destroyed. Dup can be also applied for being shared among multiple threads.
  - IoTivity
    - IoTivity 1.1.0 release code is taken as base code for TinyAra.
    - It supports secured resources for device to device (through the UDP adapter).
    - Provisioning, ownership transfer, and access control features are enabled for the d2d scenario.
    - Registering to the IoTivity 1.1.0 cloud and getting the auth provider is supported.
    - Publishing the IoTivity resources over IoTivity 1.1.0 cloud is supported.
    - CoAP over TCP is supported for communication to IoTivity 1.1.0 cloud infrastructure.
    - Sample support for IoTivity_nosecurity, Security, Cloud, and IoTivity over UDp is provided.
- Shell (TASH)
  - Supports 2 command types
    - Synchronization
      - Execute a command immediately in the shell context.
    - Asynchronization
      - Execute a command in a new pthread context.
  - Support API to register commands by each module has been enabled.
  - Executing a shell script is supported.
- Framework
  - System I/O (5 API sets)
    - GPIO (General Purpose Input/Output)
    - I2C (Inter-Integrated Circuit)
    - SPI (Serial Peripheral Interface)
    - PWM (Pulse-width modulation)
    - UART (Universal Asynchronous Receiver/Transmitter)
  - Device Management (2 API sets)
    - LWM2M Client
    - Connectivity Monitoring
  - AraStorage (1 API set)
    - Query data and Cursor
- Library
  - Built-in Libc and Libxx APIs
- External
  - Device Management (Wakaama EPL v1.0)
  - Iotivity v1.1.0
- Examples
  - Artik_demo
  - Dtls_client
  - Dtls_server
  - Fota_sample
  - Hello
  - Hello_tash
  - helloxx
  - IotivityIperf
  - Kernel_sample
  - Mdns_test
  - Mpu
  - Nettest
  - Ntpclient_test
  - Proc_test
  - Select_test
  - Sysio_test
  - Telnetd
  - Testcase
  - Tls_client
  - Tls_selftest
  - Tls_server
  - Wakaama_client
  - Webclient
  - Webserver
  - Websocket
  - Workqueue

Known Issues

- Pthread Cancellation
  - When the pthread_cancel() function is called to terminate a pthread, resource used for the pthread is not freed.
- FOTA Support
  - Device driver and HAL APIs are provided, but chip dependency codes must be updated. After updates, the APIs will be fixed.
- Wi-Fi Support
  - Network stack is provided, but there is no Wi-Fi stack.  Wi-Fi stack is provided soon. After the Wi-Fi stack is added, the network functionality will work correctly.
