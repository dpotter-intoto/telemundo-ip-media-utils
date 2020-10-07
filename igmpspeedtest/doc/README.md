# igmpspeedtest
*Version 1.1.0*

Igmpspeedtest is a "Swiss Army Knife" for testing multicast stream JOIN/LEAVE requests.  Originally developed to help assess the network's response time for a large number of IGMP requests, it now supports a variety of IGMP-related tasks including:
* Multicast stream generation (up to 254 streams)
* Multicast stream JOINs and LEAVES with performance measurement (up to 254 streams)
* Scheduled operations

## Usage

````
  -l, --local=VALUE          IP address of local NIC to use (default: 127.0.0.1)
  -n, --network=VALUE        Starting multicast group address (default: 230.8.97.1)
  -c, --count=VALUE          Number of multicast streams to test (default: 1)
      --leave                Include IGMP LEAVE tests (requires WinPcap 4.1.3)
  -r, --receiver             This instance will JOIN the stream instead of emit the stream.
  -t, --timeout=VALUE        Duration (in seconds) to wait for data to arrive on multicast before quitting (default: 5)
      --ttl=VALUE            Time to live for emitted packets (default: 32)
  -q, --quiet                Just do it, dont ask.
      --json                 Output results in JSON
      --pbl                  Pause before issuing LEAVE messages
      --startat=VALUE        Time to start
  -h, --help                 Show this message
````

**-l|--local** ***ip-address***
Specify the local IP address of the NIC you would like to use for all operations  
Default:  127.0.0.1

**-n|--network** ***ip-address***
Specify the first multicast IP address that you would like to use as the start for all operations.  If you request more than a single multicast  using the **-c** parameter, subsequent multicasts will increment the last digit of this IP address.  
Default:  230.8.97.1

Example:  **igmpspeedtest -l 192.168.1.1 -n 232.40.1.9 -c 3** will test three multicast streams:  232.40.1.9, 232.40.1.10 and 232.40.1.11, all using the NIC with IP address 192.168.1.1

**-c|--count** ***number***
Specify the number of multicasts to test  
Default: 1

**-r|--receiver**  causes **igmpspeedtest** to emit IGMP JOIN messages for each multicast stream and measure the time until traffic begins to arrive for each strem.  If omitted, then **igmspeedtest** will EMIT multicast streams, and not issue JOIN commands.

**--leave** when used in conjunction with **-r** will cause **igmpspeedtest** to also emit an IGMP LEAVE command for each stream and measure the time until traffic for that stream stops arriving

**-t|--timeout** specified the duration (in seconds) that **igmpspeedtest** should emit a stream (in emit mode), or wait to receive a stream (in receive mode).  
Default: 5 seconds

**--ttl** ***hop count*** specifies the time-to-live for emitted multicast streams (expressed in router hops).  
Default: 32 hops

**-q|--quiet** instructs **igmpspeedtest** to begin operations upon execution without asking the operator to press RETURN before starting.

**--json** instructs **igmpspeedtest** to output all test results in JSON format.

**--pbl** instructs **igmpspeedtest** to ask the operator to press RETURN before issuing IGMP LEAVE commands.

**--startat** ***datetime*** instructs **igmpspeedtest** to wait until the specified datetime before beginning to emit streams (or issue IGMP JOIN commands, in receive mode).  ***datetime*** should be specified in a valid ISO 8601 format.

Examples:  
  `igmpspeedtest -l 192.168.1.1 -c 2 --startat=13:35`  
  `igmpspeedtest -l 192.168.1.1 -c 2 --startat=2020-04-02T13:00:30`  
  `igmpspeedtest -l 192.168.1.1 -c 2 --startat=22:45:00Z`  
