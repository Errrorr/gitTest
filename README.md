# PPHOOK
Carrying all M2M, H2M and H2H communication services.

## Current Ver. API 1.5.0 & Core 2.0.0
> Todo list
> 1. SN auto testing connect to online devices
> 2. add new-version event
> 3. support IPv6
> 4. [disscuss] support stand alone connection (ActiveNode)
> 5. docs - handover.txt
> 6. docs - sdp.txt
> 7. add pause read event of PPHookNode

* [Folder](#folder)
* [Usage](#usage)
  * [Build](#build)
  * [API](#api)
  * [Examples](#example)
  * [Tests](#tests)
  * [Run & Monitor Servers](#run-and-monitor-servers)
* [Version History](#version-history)
* [License](#license)

## Folder
Top-level directory layout

    ├── docs              # The document of protocols and mechanisms.
    ├── monitor           # Monitor codes
    ├── other             # others
    ├── samples           # Sample codes
    ├── servers           # The server of PPHook System
    ├── src               # Source codes
    ├── tests             # Tests codes
    └── third-party-lib   # Third-party library

## Usage

### Build library
````sh
> sudo apt-get install cmake
> cd PPHookV3TCP
> chmod 777 build.sh
> ./build.sh       # Normal mode library
````

### API
Looks at [PPHookAPI.h](src/PPHookV2API.h).

### Examples
Examples can be found on the [Samples folder](samples).

### Tests
Before testing, install google test.
````sh
> cd tests
> unzip googletest-release-1.8.0.zip
> cd googletest-release-1.8.0
> mkdir build && cd build
> cmake ..
> sudo make install
````

Testing is not working. Copy gtest library to system.
````sh
> cp /usr/local/lib/*.a /usr/lib/
> cp -R /usr/local/include/ /usr/include/
````

Running of individual tests is done through `./Test <module name of test>`
````sh
> cd tests
> chmod 777 test.sh
> ./test all    # Test all
````

Showing the module names of testing
````sh
> ./test -h
````

### Run and Monitor Servers

#### Prebuilt
Install libev
````sh
> wget http://dist.schmorp.de/libev/libev-4.24.tar.gz
> tar -xvf libev-4.24.tar.gz
> cd libev-4.24
> ./configure
> make
> sudo make install
````

Install hiredis
````sh
> wget https://codeload.github.com/redis/hiredis/zip/master
> unzip hiredis-master.zip
> cd hiredis-master
> make
> sudo make install
````

Install libGeoIP from https://github.com/maxmind/geoip-api-c

Copy libraries to /usr/lib/
````sh
sudo cp -r /usr/local/lib/*.a /usr/lib/
````

#### Build & Run
The following describes how to execute the server in the foreground.
The background execution with `screen` is described in [Monitor](#Monitor)

The advantage of `screen` is that it can reserved the logs at the printer in spite of closing tty.
````sh
> ./build.sh -bs   # Bootstrap mode  (BS) library
> cd servers
> make bs
> ./PPHookBS sysfilePath
````
````sh
> ./build.sh -ds   # Device mode     (DS) library
> cd servers
> make ds
> ./PPHookDS sysfilePath
````
````sh
> ./build.sh -sn   # Super Node mode (SN) library
> cd servers
> make sn
> ./PPHookSN sysfilePath
````

#### Monitor
Looks at [README.md](monitor/README.md) of Monitor.

## Version History
* 1.5.0
````
1. refactor, modify and test cert function
    > changed initSetup/initProcess.c .h to init/init.c .h
2. decrease the frequence of checking cert valid time
3. modify log mechanism
    > apiLogPrint.c .h
    > pphooklogPrint -> PPLOG
4. add singal mechanism
    > only work in bs, ds, and sn mode
    > listen signal SIGUSR1 and reply signal SIGUSR2
5. add monitor mechanism
    > the program of monitor is in the folder "monitor"
    > using config.yml set enalbe/disable.
6. sysfile can determine whether relay or not.
7. add new dev API PPHookConnNodePunchIs to get the punch type of connNode
8. add more result of API PPHookNodeSend,
9. add "PPHOOK_ERROR_SOCK_ERROR", "PPHOOK_ERROR_OLD_SYSFILE" to API enum PPHOOK_ERROR
10. replace MS report to new format
    > defined at mscp/msRepo.c.h and mscp/msFormat.c.h
11. The log of core changed at [coreChangedLog.txt](docs/coreChangedLog.txt)
````

---
### PPHookV2TCP
* 1.4.3
````
1. add signal listener.
2. add new API PPHookPIDSave in the PPHookDevAPI.h
3. add signal detector sample in the folder "sample/process_detector"
````

* 1.4.2
````
1. add some error information in the PPHookNodeSend.
    > When the memory less, PPHookNodeSend will return 0
2. add new error PPHOOK_ERROR_SOCK_ERROR in the PPHookV2API.h
````

* 1.4.1
````
1. add PPHookConnNodePunchIs to get the punch state of PPHook Connection
2. If the tlv doesn't relay, the PPHook conneciotn will automatically disconnect when relay.
3. add error code ERRORNUM_NOT_SUP_RELAY and API_WARNING_NOT_SUP_RELAY
````

* 1.4.0
````
1. add ENABLE_CONSOLE in PPHookConfig.h
2. add some loss include header files
3. change PPHOOK_RC4 to RC4
````

* 1.3.9
````
1. add send event function to PPHookNode.
    > add PPHOOK_ATTR_SEND_EVENT_CLOSE and PPHOOK_ATTR_SEND_EVENT_OPEN to PPHOOK_ATTR in PPHookV2API.h
    > add PPHOOK_EVENT_SEND to PPHOOK_EVENT in PPHookV2API.h
    > add some function in apiConnManager.c/.h
    > modify handover.c/.h
    > modify v2Core
2. add io select to user.
    > add enum PPHOOK_IO_EVENT in PPHookV2API.h
    > add PPHOOK_IO_CB in PPHookV2API.h
    > add PPHookIOListen function in PPHookV2API.h
    > add PPHookIOUnListen function in PPHookV2API.h
    > add some function in apiConnManager.c/.h
    > modify v2Core
3. modify PPLL_HEAD in PPllist.h
4. add send event unittest
5. add io select unittest
````

* 1.3.8
````
1. modify tests/README.txt
````

* 1.3.7
````
1. add build.sh to make libpphook
2. revise the PPHookConfig.h, in order to write build.sh
3. fixed the problem that remove free(activeNode->nodeWrapper) to the end of _freeActiveNode();
4. fixed the ENALBE_RC4 to ENABLE_RC4 in apiConnManager.h
5. mark CalStatistics in unit test.
6. fixed p2p unit test. (replace IS_TEST_YES with IS_TEST_No)
````

* 1.3.6
````
1. modify unit test, split INIT_INTEGRATION test to INIT_TEST, PROBE_TEST, CONN_TEST
2. add ENABLE_RC4 in PPHookConfig.h
3. add RC4 processing .c/.h
4. replace dtls library with dtls library of PPHookAPI - 1.3.3
5. modify MSCP & TEST Conn, need pass encode token to about API:
    > PPHookMscpProbeNodeCreate()
    > PPHookMscpConnNodeCreate()
    > PPHookTestConnNodeCreate()
6. add ERRORNUM_PEER_SHUT_DOWN to apiConnManager.h, this errnum occur when peer
   closes udp connection (udp use connect() function).
7. fixed the problem, if the node of connection failure isn't freed, _handleActiveNodesAction()
   will continue to show "connecting" info
````

* 1.3.5
````
1. remove hmac_key from CONN_INFO, which is record in CORESOCKET when call regCoresocket()
2. add call callback at state INITPROCESS_EV_NOTYETSTARTED when checkCertValidTime()
   in statusManager.c
3. add README.txt in tests directory
4. modify makefile of tests to fixed compile error
5. add test HO.TRY_PUNCH
6. add void PPHookWorkingModeSet(PPHOOK_WORKING_MODE mode) for developer to control
   the working mode of pphook core
7. add define MAX_TCP_PUNCH_RETRY_COUNT in apiConnManager.h
8. add int handoverTryTcpPunch(void *ownerNode) in handover.c for making decision
   of TCP-TCP connection handover need to do punch or not
9. fixed "can't handover" bug
    > add two parameters "ownerRecvCb" and "hoRecvCb" to checkHandoverAtSending()
      for switching CORESOCKET between CONN_INFO
    > add two parameters "ownerRecvCb" and "hoRecvCb" to switchHoConnInfo() for
      switching CORESOCKET between CONN_INFO
    > use moveCoresocket() in switchHoConnInfo() to copy entire CORESOCKET or
      connection will be broken when use it to send/recv data
10. set PPHook Core TCP Version to 2.0.0
````

* 1.3.4
````
1. Support PPHook Core TCP Version
````

---
### PPHookV2
* 1.3.2
````
1. modify description of defintions, structs, enums and functions in coreAPI.h
2. rename enumeration PPHOOK_ENCRY to PPHOOK_SECURITY in PPHOOKV2API.h
3. reanme connEncry to connSecurityLevel of struct activenode in apiConnManager.h
````

* 1.3.1
````
1. add MS report of SN network usage. The report interval is SN_REPORT_USAGE_INTERVAL
   and the retry times of reporting is SN_REPORT_USAGE_RETRY_TIMES which are defined in mscp.c.
````

* 1.3.0
````
1. add Public API PPHookTokenGet() and GET_PPHOOK_TOKEN test
2. add Developer API PPHookNetIfSet(), setNetif.c/.h, ENABLE_BIND_INTERFACE in
   PPHookConfig.h and SET_NETWORK_INTERFACE test
3. add argument "isBindOnNetif" for dnsInit() and tands_init(), because of bound
   network interface maybe not use system defined DNS server
4. modify windows bug on close socket in apiConnManager.c
````

* 1.2.6
````
1. add WIN32 definintion in PPHook soruces
````

* 1.2.5
````
1. add RC4 module
2. add high level (AES) and low level encry to API
3. revise the report code in API (shorten code);
4. add new api, which can add additional attribute to connection, such as dynamic open/close ho.
````

* 1.2.4
````
1. revise txRxData.c to txData.c
2. avoid recurrence data processing
3. define all data buf max szie is 4096
   > DTLS: 4096
   > API: buf1 = buf2 = 4096
   > KCP: 4096
4. the max size of user data is 4000
````

* 1.2.3
````
1. revise time (millisecond) report
2. add dns setting function
3. add report online time
4. shorten txData.c code
5. add tdns resend and add two dns domain
````

* 1.2.2
````
1. add MS conn report
2. add show connect time to user
3. add show connecting infomation to user (per second)
4. revise the problem that call to connect before cert ready, this problem will lock connect.
5. add API error and warning report
6. add timer to reget the cert
7. add cert report to MS
8. add cert getting infomation to user
9. add log callback
10. revise handover problem of the sdp timing
````

* 1.2.1
````
1. add handover module
2. revise user log
````

* 1.1.1
````
1. add KCP library
2. delete utp library
````

* 1.0.1
````
1. modify CMakeList.txt
2. move src/lib to third-party-lib/libsrc
3. move some definition in udpDataPort.h to PPHookConfig.h
4. add definitions in PPHookConfig.h
````

* 1.0.0
````
1. first completed verion!
````

* 0.9.4
````
1. add manual to PPHookV2API.h
2. revise the auth to encrypt
3. add NO_SERT stat to self event.
4. add log to query dns to determind whether having network or not.
````

* 0.9.3
````
1. add MSCP devSetMscpIpPortV4() and devSetMscpReportToken()
2. add SDP_CLOSE_NOTIFY definintion and functions
3. add PPHOOK_ERROR_CONNECTION_TIMEOUT in coreAPI.h
4. add sendPPHookDebugMsg()
````

* 0.9.2
````
1. revise the mscp to report connect failure token
2. modify definition of auxServerRecvData() in PPHookDevAPI.h
3. modify the get sysfile string in initProcess_getQryTlvPkt()
4. modify freeActiveNode() AUX Server pass the close socket function
5. add log printf
6. revise the PPHookNodeFree in order to the node doesn't insert to list
7. When expired, the client doesn't search and connect the server doesn't
   accept the normal connecion except test and ms connection
8. revise using dtls connection problem
9. revise the initProcess qry tlv (for android)
````

* 0.9.1
````
1. revise the utp thread-safe problem
2. revise the relay pattern judge
3. change all timer function
4. add pad to all struct if need
5. change "define TOKEN_LEN" name
````

* 0.9.0
````
1. add PPHookGetActiveNodeNum dev API
2. check and revise the end case of all FOREACH of apiConnManger.c
3. revise the problem, which the buf1 and buf2 is uninitialized at apiConnManger.c
4. add change network API and call v2api function to re-start PPHook core (lightly)
5. revise the problem, which the keepAliveTimer is uninitialized
6. change the unittest mechanism from Manually trigger to automatic trigger
7. modify callback cases
8. revise the utp thread-safe problem
9. revise the relay pattern judge
10. change all timer function
11. add pad to all struct if need
12. change "define TOKEN_LEN" name
````

* 0.8.2
````
1. change PPHook Core to 0.9.8.7
2. add report error number and relay type (in connect callback)
3. add definition of RASPBARRY_PI3_SN for testing
````

* 0.8.1
````
1. change PPHook Core to 0.9.8.6 AES version
2. modify RC4_datarecv/send to AES_datarecv/send
3. modify broadcast callback
4. modify _insertActiveNodeToTail() mutex_lock() to mutex_trylock()
5. add definitions of API_DEBUG and CORE_DEBUG
````

* 0.8.0
````
1. add install in CMakeList.txt
````

* 0.7.2
````
1. add AUX server functions
2. create PPHookServerSample.cpp v0.0.1
3. create PPHookClientSample.cpp v0.0.1
4. create PPHookMSCP.cpp v0.0.1
5. add close active node socket
6. addPPHookMscpProbeNodeCreate(), PPHookMscpConnNodeCreate() and PPHookTestConnNodeCreate()
````

* 0.7.1
````
1. define ENABLE_GET_SYSFILE_FROM_AUX_SRV
2. add AUX for retrive new system file
````

* 0.7.0
````
1. add PPHook Core
2. define PPHOOK_CORE
````

* 0.5.1
````
1. define developer used definition DEVELOPER_VERSION, ENABLE_MSCP_CONNECT, ENABLE_TEST_CONNECT and ENABLE_BROADCAST
2. integrate MSCP and CONN_TEST, and add PPHookDevAPI.h
3. add test for MSCP and CONN_TEST
4. add structure "TAG_MSG" in sdp.h for transmitting meta data
5. add PRE_PPHOOK_CALLBACK for handling the bug "data callback might be called after close callback of one PPHookNode"
6. move test functions to testHelper.cpp
7. note that the arg of PPHookNode should take care by API user (ex. use mutex)
````

* 0.5
````
1. integrate DTLS and UTP
2. define dtls and utp log
````

* 0.4
````
1. add DTLS module
2. add some test for DTLS
3. add new function 'removeAllEventOfNode' at PPHookCallback.c for reslove memory leak problem
4. adjust DTLS library
````

## License

Copyright YounGiant Inc. 2015-2018. Author by Xue-Yi, Tobby Lai.

