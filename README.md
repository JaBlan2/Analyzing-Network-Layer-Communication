# Analyzing Network Layer Communication

## Introduction
This is an incident report done as a practice to hone my skills and give me experience in doing incident reports. This report is performed on the fictitious website [www.yummyrecipesforme.com](), and was completed as a part of my Cybersecurity Portfolio alongside Google's [Cybersecurity Professional Certificate](https://www.coursera.org/google-certificates/cybersecurity-certificate) on Coursera. This is an activity in course 3, [Connect and Protect: Networks and Network Security](https://www.coursera.org/learn/networks-and-network-security/home/welcome).

The goal of this report is to analyze DNS and ICMP traffic to identify aspects of a cybersecurity incident. I will identify which network protocols are utilized and convey the issues directly and coherently in the report.

## Scenerio
I am a cybersecurity analyst working at a company that specializes in providing IT services for clients. Several customers of clients reported that they were not able to access the client company website [www.yummyrecipesforme.com](), and saw the error “destination port unreachable” after waiting for the page to load. 

I am tasked with analyzing the situation and determining which network protocol was affected during this incident. To start, I attempt to visit the website and I also receive the error “destination port unreachable.” To troubleshoot the issue, I load my network analyzer tool, tcpdump, and attempt to load the webpage again. To load the webpage, my browser sends a query to a DNS server via the UDP protocol to retrieve the IP address for the website's domain name; this is part of the DNS protocol. My browser then uses this IP address as the destination IP for sending an HTTPS request to the web server to display the webpage  The analyzer shows that when I send UDP packets to the DNS server, I receive ICMP packets containing the error message: “udp port 53 unreachable.” 

13:24:32.192571 IP 192.51.100.15.52444 > 203.0.113.2.domain: 35084+ A?
yummyrecipesforme.com. (24)
13:24:36.098564 IP 203.0.113.2 > 192.51.100.15: ICMP 203.0.113.2
udp port 53 unreachable length 254

13:26:32.192571 IP 192.51.100.15.52444 > 203.0.113.2.domain: 35084+ A?
yummyrecipesforme.com. (24)
13:27:15.934126 IP 203.0.113.2 > 192.51.100.15: ICMP 203.0.113.2
udp port 53 unreachable length 320

13:28:32.192571 IP 192.51.100.15.52444 > 203.0.113.2.domain: 35084+ A?
yummyrecipesforme.com. (24)
13:28:50.022967 IP 203.0.113.2 > 192.51.100.15: ICMP 203.0.113.2
udp port 53 unreachable length 150

In the tcpdump log, I find the following information:

The first two lines of the log file show the initial outgoing request from my computer to the DNS server requesting the IP address of yummyrecipesforme.com. This request is sent in a UDP packet.

The third and fourth lines of the log show the response to my UDP packet. In this case, the ICMP 203.0.113.2 line is the start of the error message indicating that the UDP packet was undeliverable to port 53 of the DNS server.

In front of each request and response, I find timestamps that indicate when the incident happened. In the log, this is the first sequence of numbers displayed: 13:24:32.192571. This means the time is 1:24 p.m., 32.192571 seconds.

After the timestamps, I will find the source and destination IP addresses. In the first line, where the UDP packet travels from my browser to the DNS server, this information is displayed as: 192.51.100.15 > 203.0.113.2.domain. The IP address to the left of the greater than (>) symbol is the source address, which in this example is my computer’s IP address. The IP address to the right of the greater than (>) symbol is the destination IP address. In this case, it is the IP address for the DNS server: 203.0.113.2.domain. For the ICMP error response, the source address is 203.0.113.2 and the destination is my computers IP address 192.51.100.15.

After the source and destination IP addresses, there can be a number of additional details like the protocol, port number of the source, and flags. In the first line of the error log, the query identification number appears as: 35084. The plus sign after the query identification number indicates there are flags associated with the UDP message. The "A?" indicates a flag associated with the DNS request for an A record, where an A record maps a domain name to an IP address. The third line displays the protocol of the response message to the browser: "ICMP," which is followed by an ICMP error message.

The error message, "udp port 53 unreachable" is mentioned in the last line. Port 53 is a port for DNS service. The word "unreachable" in the message indicates the UDP message requesting an IP address for the domain "www.yummyrecipesforme.com" did not go through to the DNS server because no service was listening on the receiving DNS port.

The remaining lines in the log indicate that ICMP packets were sent two more times, but the same delivery error was received both times. 

Now that I have captured data packets using a network analyzer tool, it is my job to identify which network protocol and service were impacted by this incident. Then, I will need to write a follow-up report. 

As an analyst, I can inspect network traffic and network data to determine what is causing network-related issues during cybersecurity incidents. Later in this course, I will demonstrate how to manage and resolve incidents. For now, I only need to analyze the situation. 

This event, in the meantime, is being handled by security engineers after I and other analysts have reported the issue to my direct supervisor. 

## Cybersecurity Incident Report: Network Traffic Analysis
### Part 1: Provide a summary of the problem found in the DNS and ICMP traffic log.
The tcpdump logs show an ICMP message that indicates that port 53 is unreachable when attempting to access the website [www.yummyrecipesforme.com](), specifically over UDP. Port 53 is normally used for TCP and UDP communication, and is the default port for DNS queries. This may indicate a problem with the DNS server itself. The ICMP message gives some flags that also indicate failure, as a plus sign appears after the query identification number 35084 and flags with the “A?”, indicating DNS protocol operations.

### Part 2: Explain your analysis of the data and provide at least one cause of the incident.
The incident occurred early this afternoon when users reported not being able to reach the client website, [www.yummyrecipesforme.com](). The network security team responded and began running tests with the network protocol analyzer tool tcpdump. The resulting logs revealed that port 53, which is mainly used for DNS queries, is not reachable. The team is continuing to investigate the cause of the problem, however initial results indicate a possible DOS attack or server issue.

## Conclusion
After completing this activity and reviewing my response, I see that I did well in conveying the issue in the report. I was able to include pertanant information and didn't include much unnecessary information. I can improve my ability to write these reports by practicing more.
