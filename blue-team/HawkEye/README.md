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

---

## Q13
What operating system does the victim's computer run?

To find this, we can inspect the User-Agent of an HTTP-request. Although this method is not always reliable and can be modified by the attacker, is easy to check and worth trying more often than not.

Before we check the User-Agent, another fast verification is the TTL.Since it is different for every OS, a TTL close to 128 usually means the OS is Windows.


![q13](./screenshots/q13.png)

As stated before, we find a TTL of 128, so it's likely to be Windows. To search for the exact model we can use the User-Agent.

![q13-1](./screenshots/q13-1.png)

As shown in the capture above, the OS is Windows NT 6.1.

## 14
What is the name of the malicious file downloaded by the accountant?

In the same HTTP request, we find a suspicious exe file, obviously the malicious file we are searching for.


![q14](./screenshots/q14.png)


## Q15
What is the md5 hash of the downloaded file?

To obtain the md5 hash, we can use the md5sum command on the malicious file.

We can extract the file using Wireshark like this:

![q15](./screenshots/q15.png)

![q15-1](./screenshots/q15-1.png)

Code to obtain the hash:

```
md5sum tkraw_Protected99.exe 
    71826ba081e303866ce2a2534491a2f7  tkraw_Protected99.exe
```

## Q16
What software runs the webserver that hosts the malware?

To find information about the webserver, we have to look into the server response.

![q16](./screenshots/q16.png)

We find that the webserver name is LiteSpeed

## Q17
What is the public IP of the victim's computer?

To find the public IP of the victim (10.4.10.132), we could look for NAT devices or gateways, as they communicate directly with private IPs. However, in this case it's much simpler, as the following HTTP request asks a bot for its public address.

![q17](./screenshots/q17.png)

## Q18
In which country is the email server to which the stolen information is sent?

We can obtain the IP of the email server by filtering for the SMTP protocol. Once we have the IP, we just need to use an IP geolocation tool like we did before.


![q18](./screenshots/q18.png)


![q18-1](./screenshots/q18-1.png)


## Q19
Analyzing the first extraction of information. What software runs the email server to which the stolen data is sent?

This can be found in the first SMTP message exchanged:

![q19](./screenshots/q19.png)

## Q20
To which email account is the stolen information sent?

As seen before, the email account is sales.del@macwinlogistics.in

## Q21
What is the password used by the malware to send the email?

![q21](./screenshots/q21.png)

The password can be located easily with our current filter. Decoding it from base64 we obtain "Sales@23"

## Q22
Which malware variant exfiltrated the data?

The malware variant can be found in the data sent via email. We can decode it using CyberChef.

![q22](./screenshots/q22.png)

The variant is "Reborn v9".

## Q23
What are the bankofamerica access credentials? (username:password)

As we did before, using the decoded information we can find the credentials.

![q23](./screenshots/q23.png)

Credentials: roman.mcguire:P@ssw0rd$


## Q24
Every how many minutes does the collected data get exfiltrated?

The data is sent to the email every 10 minutes

![q24](./screenshots/q24.png)
