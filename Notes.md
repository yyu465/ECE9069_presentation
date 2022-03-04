# __Presentation Notes of Hping__

## 1. What is Packet Crafting and Hping?*
####	Packet Crafting: 
Packet crafting a technique which can be divided in to four stages, packet assemble, packet editing, packer replay and packet decoding. [^1] This technique allows network admins to investigate firewall rules settings and locate entry ports into an oriented machine or network. [^2]
####	Hping: 
Hping is a free, cross-platform command-line tool which works on unix-like systems such as Windows, Linux, and Mac OSX. As one of the most widely used tool in security monitoring, and firewall/network testing, it allows packet crafting and TCP/IP protocol analyzing. Many popular protocols such as TCP, UDP, ICMP were supported by Hping. [^3]

## 2. Basic Information of Hping
The latest version Hping3 was implemented with a string-based engine in Tcl language. The description of packets can be human readable. It aims to provide convenience for users to write scripts or codes and save their time. Basic Tcl programming knowledge is necessary when applied hping3.

## 3. Development of Hping
Hping is developed and maintained in C programming language by Antirez and released under the GPL License version 2. The latest version of Hping is Hping3 which was released on November 5, 2005.  Although Hping is no longer actively developed, development is open. So, users could still commit and submit changes to the main source tree. [^4] Nowadays, it is implemented in the Nmap Security Scanner which is often considered as a complementary. The Nmap Project created and maintains Nping which is like Hping but with more modern features such as IPv6 support and unique echo mode. [^5]



## 4.	Hping website and download

*  Primary site: http://www.hping.org/
*  Download the latest code: http://www.hping.org/download.html
*  Hping main source tree: https://github.com/antirez/hping



## 5. Interface of Hping
*  Usage

![image](https://user-images.githubusercontent.com/46683010/156851947-ff1b6d3a-1bd5-474a-b114-382a5ded7219.png)

*  Mode


![image](https://user-images.githubusercontent.com/46683010/156851959-bd2d2749-b1dc-4cb2-b3f7-1f3890217d31.png)

* IP


![image](https://user-images.githubusercontent.com/46683010/156851966-7ed3d15c-db68-4d2f-a347-7fd03a9e295d.png)


* ICMP


![image](https://user-images.githubusercontent.com/46683010/156851975-700cf1ab-85da-4d37-9016-1116f712580f.png)


* UDP/TCP


![image](https://user-images.githubusercontent.com/46683010/156851983-d2f09ee8-26c9-42fb-8760-4033d4745f52.png)


* Common


![image](https://user-images.githubusercontent.com/46683010/156851995-a6973089-e9c3-4908-bdc8-1ac9e0c1245f.png)


* ARS Packet Description (new, unstable)


![image](https://user-images.githubusercontent.com/46683010/156852006-84a7087a-cf7e-4829-92ce-2b4bb414c6f4.png)





## 6.	Main Applications of Hping*

*  _"Firewall testing"_
*  _"Advanced port scanning"_
*  _"Network testing, using different protocols, TOS, fragmentation"_
*  _"Manual path MTU discovery"_
*  _"Advanced traceroute, under all the supported protocols"_
*  _"Remote OS fingerprinting"_
*  _"Remote uptime guessing"_
*  _"TCP/IP stacks auditing"_
*  _"hping can also be useful to students that are learning TCP/IP"_.[^6] 
*  Additionally, hackers could use Hping to do some attacks however, some attacks can be defended by the firewall of system. 

## 7. Some Attacks Implemented by Hping (demos of attacks were included in the video)*

- SYN Flood Attack

  A type of DDoS (distributed denial-of-service) attack. Aims to let a server cost server resource to wait for half-opened connections. In TCP protocol, when the server receives   an initial SYN packet from the attacker, it sends back SYN/ACK packets to respond the request and wait the final process in handshake. Attackers use this fact to do SYN flood   attack. Specifically, SYN flood attack can be divided into two parts:
  
  The attacker establishes a TCP connection with server.
  
  
  ![image](https://user-images.githubusercontent.com/46683010/156853767-2273fb9b-d021-4382-91bb-5c1a7c12dc5c.png) [^7]
  
  
  Then the attacker sends a larger amount of SYN packets to the server. Every request will be responded, meanwhile, a port will be available for each request. However, the final   Ack packet which the server is waiting for will never be sent by the attacker. The attacker continues sending SYN packet to occupy all the ports. [^8]
  
  
  ![image](https://user-images.githubusercontent.com/46683010/156853949-b9a11d03-1a39-47ba-91c7-8beb296ce9e0.png) [^9]

  - Command used in this attack
  ```sh
  #sudo hping3 -S -p 3000 10.0.0.71 --flood
  ```
  | Flag | Description |
  | ------ | ------ |
  | -S | specify SYN mode |
  | -p | identify the port  |
  | --flood | identify flood attack mode |
  | 10.0.0.71 | IP address of targeted network |


- SMURF Attack
  
  SMURF attack is another type of DDoS attack which aims to make computer network unable to function normally. Vulnerabilities of IP and ICMP would be exploited during the       attack. Specifically, the attacker spoofs the IP address (the targeted server address) and create a ping request. The request was injected into the ICMP packet and sent to IP broadcast network. All network hosts would receive the request and open an ICMP response to the spoofed address. If a large number of responses exist, the network would be affected. [^10]
  
  ![image](https://user-images.githubusercontent.com/46683010/156854241-b04da951-1274-4e84-9424-eeb993e6244d.png) [^11]
  
  
  
  - Command used in this attack
  ```sh
  #sudo hping3 -S --icmp --flood 10.0.0.71 -a 10.0.0.71
  ```
  | Flag | Description |
  | ------ | ------ |
  | -S | specify SYN mode |
  | --flood | specify flood attack mode |
  | --icmp | specify icmp mode |
  |-a | prefix before entering targeted IP address |
  | 10.0.0.71 | IP address of targeted network |
  
  

- Random Source Attack


  The attacker sends random packets from different addresses to the targeted IP address. Some other attack techniques may be combined with the random source attack. After the server or the network is attacked, it is hard to recognize the actual source address.
  
  
  - Command used in this attack
  ```sh
  #sudo hping3 -S -p 3000 10.0.0.71 --flood --rand-source
  ```
  | Flag | Description |
  | ------ | ------ |
  | -S | specify SYN mode |
  | --flood | specify flood attack mode |
  | -p | specify port number |
  |--rand-source | specify random IP source address |
  | 10.0.0.71 | IP address of targeted network |
  
  
  
- ISN Prediction (TCP Sequence Prediction Attack)


  TCP sequence prediction attack aims to predict the sequence number and generate fake packets according to the sequence number. Specifically, during TCP connection, a sequence number used to track the packet would be included in the packet. By using Hping, the sequence number of TCP packets would be displayed and attackers often use the sequence number of TCP packets to carry out harmful actions.
  
  ![image](https://user-images.githubusercontent.com/46683010/156855226-551a3ae6-02fe-4614-bf11-b6f04ed36779.png) [^12]
  
  - Command used in this attack
  ```sh
  #sudo hping3 -S -p 3000 -Q 10.0.0.71 
  ```
  | Flag | Description |
  | ------ | ------ |
  | -S | specify SYN mode |
  | -p | specify port number |
  |-Q | only show TCP sequence number |
  | 10.0.0.71 | IP address of targeted network |
  









## 8. Open Bug
Hping accepts bug reports in a variety of circumstances. The report should specify a few information.
For example:
- Operating system and version
-	Hping version
-	GCC version (if applied)
-	Tcl/Tk version (you linked against)
-	The function to reproduce the bug




# References 

[^1]: https://en.wikipedia.org/wiki/Packet_crafting 
[^2]: https://ravi73079.medium.com/attacks-to-be-performed-using-hping3-packet-crafting-98bc25584745
[^3]: http://wiki.hping.org/
[^4]: https://en.wikipedia.org/wiki/Hping
[^5]: https://sectools.org/
[^6]: Original words from http://www.hping.org/
[^7]: Image from:  https://www.cloudflare.com/learning/ddos/syn-flood-ddos-attack/
[^8]: https://www.cloudflare.com/learning/ddos/syn-flood-ddos-attack/ 
[^9]: Image from https://www.imperva.com/learn/ddos/syn-flood/
[^10]: https://www.imperva.com/learn/ddos/smurf-attack-ddos/ 
[^11]: Image from https://www.imperva.com/learn/ddos/smurf-attack-ddos/
[^12]: Image from https://www.tech-faq.com/tcp-sequence-prediction-attack.html
