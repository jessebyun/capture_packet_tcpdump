In this project, will capture and filter network traffic in a Linux environment using tcpdump. Capture data in a packet capture (p-cap) file and then examine the contents of the captured packet data. <br/>
First, identify network interfaces to capture network packet data. Second, use tcpdump to filter live network traffic. Third, capture network traffic using tcpdump. Finally, filter the captured packet data. 

<h2>Task 1. Identify network interfaces</h2>
<h4>1. Use ifconfig to identify the interfaces that are available: sudo ifconfig</h4>

<img src="https://i.imgur.com/YGPz35B.png" alt="image"/>
The Ethernet network interface is identified by the entry with the eth prefix. In this lab, I'll use eth0 as the interface that you will capture network packet data from in the following tasks.
<br/><br/>

<h4>2. Use tcpdump to identify the interface options available for packet capture:</h4>
sudo tcpdump -D

<img src="https://i.imgur.com/lt4lU8c.png" alt="image"/>
This command will also allow you to identify which network interfaces are available. This may be useful on systems that do not include the ifconfig command.
<br/><br/>

<h2>Task 2. Inspect the network traffic of a network interface with tcpdump</h2>
<h3>Filter live network packet data from the eth0 interface with tcpdump:</h3>
sudo tcpdump -i eth0 -v -c5

<img src="https://i.imgur.com/SzxyOAg.png" alt="image"/>
This command will run tcpdump with the following options:<br/>
-i eth0: Capture data specifically from the eth0 interface.<br/>
-v: Display detailed packet data.<br/>
-c5: Capture 5 packets of data.
<br/><br/>

<h3>Exploring network packet details</h3>

In the example data at the start of the packet output, tcpdump reported that it was listening on the eth0 interface, and it provided information on the link type and the capture size in bytes:
<br/>
<img src="https://i.imgur.com/OSrLpdQ.png" alt="image"/>

On the next line, the first field is the packet's timestamp, followed by the protocol type, IP:
<img src="https://i.imgur.com/vRx9HWa.png" alt="image"/>

The verbose option, -v, has provided more details about the IP packet fields, such as TOS, TTL, offset, flags, internal protocol type (in this case, TCP (6)), and the length of the outer IP packet in bytes:

<img src="https://i.imgur.com/UF6iHMr.png" alt="image"/>

In the next section, the data shows the systems that are communicating with each other:

<img src="https://i.imgur.com/WgJuqnm.png" alt="image"/>

By default, tcpdump will convert IP addresses into names. The direction of the arrow (>) indicates the direction of the traffic flow in this packet. Each system name includes a suffix with the port number (.5000 in the screenshot), which is used by the source and the destination systems for this packet.

The remaining data filters the header data for the inner TCP packet:
<img src="https://i.imgur.com/7RHETAW.png" alt="image"/>

The flags field identifies TCP flags. In this case, the P represents the push flag and the period indicates it's an ACK flag. This means the packet is pushing out data.
<br/><br/>

<h2>Task 3. Capture network traffic with tcpdump</h2>

In the previous command, used tcpdump to stream all network traffic. Here, I will use a filter and other tcpdump configuration options to save a small sample that contains only web (TCP port 80) network packet data.
<br/>

<h4>1. Capture packet data into a file called capture.pcap: </h4>

sudo tcpdump -i eth0 -nn -c9 port 80 -w capture.pcap &
<img src="https://i.imgur.com/hhbmDBx.png" alt="image"/>

This command will run tcpdump in the background with the following options:
<br/>
-i eth0: Capture data from the eth0 interface.<br/>
-nn: Do not attempt to resolve IP addresses or ports to names.This is best practice from a security perspective, as the lookup data may not be valid. It also prevents malicious actors from being alerted to an investigation.<br/>
-c9: Capture 9 packets of data and then exit.<br/>
-port 80: Filter only port 80 traffic. This is the default HTTP port.<br/>
-w capture.pcap: Save the captured data to the named file.<br/>
-&: This is an instruction to the Bash shell to run the command in the background.
<br/><br/>

<h4>Use curl to generate some HTTP (port 80) traffic: curl opensource.google.com</h4>

<img src="https://i.imgur.com/s4fePeT.png" alt="image"/>

When the curl command is used like this to open a website, it generates some HTTP (TCP port 80) traffic that can be captured.

<h4>3. Verify that packet data has been captured:</h4>

ls -l capture.pcap
<br/>
<img src="https://i.imgur.com/ycabIZS.png" alt="image"/>
