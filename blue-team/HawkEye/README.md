#  HawkEye Lab

**Platform:** CyberDefenders    
**Difficulty:** Medium
**Duration:** ~90 min   
**Category:** Network Forensics
**Link:** https://cyberdefenders.org/blueteam-ctf-challenges/hawkeye/
 
## Scenario
An accountant at your organization received an email regarding an invoice with a download link. Suspicious network traffic was observed shortly after opening the email. As a SOC analyst, investigate the network trace and analyze exfiltration attempts.

## Q1
How many packets does the capture have?

By opening the PCAP file in Wireshark, we can find the number of packets in the bottom-right corner.

![q1](./screenshots/q1.png) 

## Q2
At what time was the first packet captured (UTC)?

To display the time in UTC format, we need to change the time column format:

![q2](./screenshots/q2.png) 

![q2-1](./screenshots/q2-1.png) 

## Q3
What is the duration of the capture?

We can find the duration of the capture in the capture file properties summary:

![q3](./screenshots/q3.png) 

![q3-1](./screenshots/q3-1.png) 

## Q4
What is the most active computer at the link level?

To identify the most active computer, we can check the endpoint table under the Statistics menu:


![q4](./screenshots/q4.png) 

![q4-1](./screenshots/q4-1.png) 

## Q5
Manufacturer of the NIC of the most active system at the link level?

Using the previosly obtained MAC address "00:08:02:1c:47:ae", we can use a MAC lookup tool to identify the manufacturer.  

![q5](./screenshots/q5.png) 

For this query, the tool used was "dnschecker.org".

---


## Q6
Where is the headquarter of the company that manufactured the NIC of the most active computer at the link level?

Knowing the manufacturer, the headquarters can be located with a quick search

![q6](./screenshots/q6.png) 

## Q7
The organization works with private addressing and netmask /24. How many computers in the organization are involved in the capture?

Using the same endpoint summary, we can clearly identify the IP addresses. Knowing the netmask is /24, we find four different IPs with that subnet, but the last of them is a broadcast.

![q7](./screenshots/q7.png) 

## Q8
What is the name of the most active computer at the network level?

We already know the most active IP. Now we need to find its hostname. To do so, we can filter by DHCP to find the information frame.

![q8](./screenshots/q8.png) 

Host Name: Beijing-5cd1-PC

## Q9
What is the IP of the organization's DNS server?

By filtering by DNS we can quickly find the DNS server IP in the response column.

![q9](./screenshots/q9.png)

## Q10
What domain is the victim asking about in packet 204?

Using the same filter, we just have to locate packet 204.

![q10](./screenshots/q10.png)

## Q11
What is the IP of the domain in the previous question?

For this question, I will use VirusTotal. By going to the relations menu we can find the IP address.

![q11](./screenshots/q11.png)

## Q12
Indicate the country to which the IP in the previous section belongs.

This can easily be found by using an online IP geolocation tool: 


![q12](./screenshots/q12.png)

## Q13
What operating system does the victim's computer run?
