#  Web Investigation Lab

**Platform:** CyberDefenders    
**Difficulty:** Easy  
**Duration:** ~45 min   
**Category:** Network Forensics
**Link:** https://cyberdefenders.org/blueteam-ctf-challenges/web-investigation/
 
## Scenario
You are a cybersecurity analyst working in the Security Operations Center (SOC) of BookWorld, an expansive online bookstore renowned for its vast selection of literature. BookWorld prides itself on providing a seamless and secure shopping experience for book enthusiasts around the globe. Recently, you've been tasked with reinforcing the company's cybersecurity posture, monitoring network traffic, and ensuring that the digital environment remains safe from threats.
Late one evening, an automated alert is triggered by an unusual spike in database queries and server resource usage, indicating potential malicious activity. This anomaly raises concerns about the integrity of BookWorld's customer data and internal systems, prompting an immediate and thorough investigation.
As the lead analyst in this case, you are required to analyze the network traffic to uncover the nature of the suspicious activity. Your objectives include identifying the attack vector, assessing the scope of any potential data breach, and determining if the attacker gained further access to BookWorld's internal systems.

## Q1
By knowing the attacker's IP, we can analyze all logs and actions related to that IP and determine the extent of the attack, the duration of the attack, and the techniques used. Can you provide the attacker's IP?

In order to quickly find the attacker's IP address, we can use the analysis tab in Wireshark. 

![q1](./screenshots/q1.png) 


![q1-1](./screenshots/q1-1.png) 

As we can see in the capture above, we have two distinct IP addresses with a distinctive amount of packets sent. Most likely the Victim and the Hacker.  

We can find which one is the attacker by looking at the ports used by each one.  

![q1-2](./screenshots/q1-2.png) 

As we can observe, the IP "111.224.250.131" is reaching out to many different ports, likely scanning for open ports.

## Q2
If the geographical origin of an IP address is known to be from a region that has no business or expected traffic with our network, this can be an indicator of a targeted attack. Can you determine the origin city of the attacker?  

To find the origin of an IP address we can use any IP location tool. I will be using a website called "iplocation.com"

![q2](./screenshots/q2.png) 

The origin city of the attacker is Shijiazhuang

## Q3
Identifying the exploited script allows security teams to understand exactly which vulnerability was used in the attack. This knowledge is critical for finding the appropriate patch or workaround to close the security gap and prevent future exploitation. Can you provide the vulnerable PHP script name?

Following the HTTP stream, we can easily find many queries to "search.php", brute-forcing it to exploit the vulnerable script.

![q3](./screenshots/q3.png) 

## Q4
Establishing the timeline of an attack, starting from the initial exploitation attempt, what is the complete request URI of the first SQLi attempt by the attacker?

Now that we know the name of the script, we can filter it to clearly see the queries made by the attacker.    

![q4-0](./screenshots/q4-0.png) 

Inspecting the first frame containing SQLi, we can copy the query and decode it.  

![q4](./screenshots/q4.png) 

There are many tools to do this, but I will be using as follows:

![q4-1](./screenshots/q4-1.png) 

## Q5
Can you provide the complete request URI that was used to read the web server's available databases?

We can narrow the search by applying a filter searching for the word "SCHEMA", as it is a common way to discover the available databases using SQLi.

![q5](./screenshots/q5.png) 

Investigating the first frame, we can see many databases listed in the response, which is a clear indicator that we have found the request we were searching for.

![q5-1](./screenshots/q5-1.png) 

Now we need to decode the URL as we did before.

![q5-2](./screenshots/q5-2.png) 


## Q6
Assessing the impact of the breach and data access is crucial, including the potential harm to the organization's reputation. What's the table name containing the website users data?

To find the breached table name, we can filter by the common SQLi variable that contains it, INFORMATION_SCHEMA or just SCHEMA as I have filtered it. 

![q6](./screenshots/q6.png) 

With the suspicious frames located, we now have to follow the http stream, looking at the answer from the server to see the information obtained. 

![q6-1](./screenshots/q6-1.png) 

As we can see in the capture above, the server is returning 3 tables, admin, books and customers. 

## Q7
The website directories hidden from the public could serve as an unauthorized access point or contain sensitive functionalities not intended for public access. Can you provide the name of the directory discovered by the attacker?

To filter this correctly, keep in mind that any successful access will return a 200 http code, we could also search for other response codes such as 301, 302, 401 or 403 in case we don't obtain results with our first query.

![q7](./screenshots/q7.png)  

Now we just have to inspect a random frame to see which directories have been compromised.


![q7-1](./screenshots/q7-1.png) 

As we can observe, the admin directory is vulnerable and I have come across a login frame, where the attacker is attempting to log in.

## Q8
Knowing which credentials were used allows us to determine the extent of account compromise. What are the credentials used by the attacker for logging in?

As stated before, we have already found the frames where the threat actor is attempting to log in, therefore we just need to follow the http stream. 


![q8](./screenshots/q8.png)  

By doing so, we find a successful attempt with the credentials admin:admin123!, remember that %21 means "!"

## Q9
We need to determine if the attacker gained further access or control of our web server. What's the name of the malicious script uploaded by the attacker?

We can quickly locate the malicious script by searching for POST requests after the admin credentials breach. For this specific case a simple http POST filter will suffice.

![q9](./screenshots/q9.png) 

Inspecting the last frame, we can locate the name of the file, NVri2vhp.php

![q9-1](./screenshots/q9-1.png) 

