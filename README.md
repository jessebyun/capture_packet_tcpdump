In this project, will capture and filter network traffic in a Linux environment using tcpdump. Capture data in a packet capture (p-cap) file and then examine the contents of the captured packet data. <br/><br/>
First, identify network interfaces to capture network packet data. Second, use tcpdump to filter live network traffic. Third, capture network traffic using tcpdump. Finally, filter the captured packet data. 

<h2>Identify network interfaces</h2>
<h3>Use ifconfig to identify the interfaces that are available: sudo ifconfig</h3>

<img src="https://i.imgur.com/YGPz35B.png" alt="image"/>
The Ethernet network interface is identified by the entry with the eth prefix. In this lab, I'll use eth0 as the interface that you will capture network packet data from in the following tasks.
<br/><br/><br/>

<h3>Use tcpdump to identify the interface options available for packet capture: sudo tcpdump -D</h3>

<img src="https://i.imgur.com/lt4lU8c.png" alt="image"/>
This command will also allow you to identify which network interfaces are available. This may be useful on systems that do not include the ifconfig command.
