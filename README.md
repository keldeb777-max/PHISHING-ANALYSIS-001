# PHISHING ANALYSIS OF A PHISHING MAIL

## Project Overview
Phishing is one of the common ways malicious actors gain sensitive information and access credentials from unsuspecting targets or ask them to perform certain tasks. Malicious actors send emails that look as if they are coming from a legitimate source. Most phishing emails impersonate IT teams and prompt users to reset passwords or inquire certain information and usually attached with a warning or a deadline. 
As a cybersecurity analysis, it an important skill to notice, analyze and categorize such emails. 

## Project Objectives
1. Download email file.
2. Inspect source code of the email.
3. Analyze the information of the sender.
4. Inspect and analyze the content of the mail.
5. Scan mail for malicious links.
6. Determine if the mail is malicious or not.

## Resources used in this project
The following resources(and tools) were used for this test:
1. Emails for analysis were downloaded from `https://github.com/rf-peixoto/phishing_pot`
2. Thunderbird Mail as our Mail User Agent (MUA)
3. Sublime Text 
4. emldump.py and oledump are open-source python tools for email forensic analysis created by Didier Stevens `https://github.com/didierstevens/didierstevenssuite`
5. eioc.py is another open-source python tool for forensic analysis created by MalwareCube `https://github.com/MalwareCube/Email-IOC-Extractor`
6. Virus Total `https://www.virustotal.com/gui/home/upload`
7. Whois Domain tools `https://whois.domaintools.com/`
8. Cyberchef `https://gchq.github.io/CyberChef/`

## Email Analysis 
In order to perfom this email analysis, I downloaded a sample email of a real email from the github repository `phishing_pot` collected by the creator, Rodrigo F. Peixoto (`rf-peixoto`).
The email, upon opening displays what appears to be a promotional mail for okx web3 extension for Google Chrome. It gives instructions on how to install the extension and list benefits of the extension. At first glance at the mail, it looks legitimate but the sender address looks less likely to have come from okx.  

<img width="198" height="47" alt="Screenshot 2026-11-17 115407" src="https://github.com/user-attachments/assets/dc7e700d-faa7-44aa-856b-abbced20c9be" />

The sender's email seems suspicious so further analysis had to be done. A quick look at sender's email is shown as `hpayMonitorPlatform@handpay.com.cn` and looks less likely to be an official email of the okx organization. It breeds suspicion to receive a legitimate looking mail from an email that does not bear the official domain of the organization hence further analysis was needed. The email file is then open in sublime text to analyze the headers and other components. In the opened file as shown below, I noticed the `Return Path` and `X-Sender-IP` did not match the records of okx.com so I ran a quick whois scan on `whois.domaintools.com`.

<img width="736" height="371" alt="Screenshot 2026-11-18 122110" src="https://github.com/user-attachments/assets/03779927-73e9-4ac8-a8f3-33e9de2fd8a9" />  

(Return Path marked in red, Sender's IP marked in green) 

I also run the used the `eioc.py` email analysis tool to analyze and collect information about the headers, IP and links in the email.  
<img width="518" height="345" alt="eioc screenshot " src="https://github.com/user-attachments/assets/7c95f94f-391f-426f-9ee5-03adb3626ca1" />   

The screenshot shows the extracted IP addresses, embedded link URLs and the relevant headers that are worth examining all in their defanged format so that it will not be clickable. 


<img width="741" height="347" alt="Screenshot 2026-11-18 083003" src="https://github.com/user-attachments/assets/4e8cdd14-2b21-453d-be70-d928134c857b" /> 

From the screenshot above from `whois.domaintools.com`, is is evident that the sender's IP traces to a different source other than the official sites of okx.com. I proceeded to search for spf records for okx.com and as shown below none matched the domain of the sender. This raised more suspicions.  

<img width="733" height="35" alt="nslookup spf records" src="https://github.com/user-attachments/assets/9ea0ec0c-502d-4098-9a84-b0f053e18644" />

At this point, I had been able to establish the following facts:
1. The mail did not originate from the official domains registered in the spf records of okx.com
2. The domain of the email failed spf and DMARC checks according the the mail headers
3. The IP of the sender points to China and routes through servers in Russia which usually raises flags

At this point I was intrigued to prove further. I asked why a legitimate looking mail is being received from a non-legit email. 
The mail contained instructions for a user to add extension to their Chrome Browser. The link on the page was copied and pasted through platforms such as Virus Total, Cisco Talos, UrlVoid, and they all did not label or tag it as malicious.  

<img width="734" height="370" alt="Virus total for chrome extension" src="https://github.com/user-attachments/assets/262d82c6-d2fa-4614-a349-6c8efedd0a88" />
<img width="738" height="347" alt="urlvoid com chrome extension " src="https://github.com/user-attachments/assets/d5844977-6691-49d2-bd83-87f34322b9bb" />
<img width="739" height="347" alt="Cisco Talos scan google chrome" src="https://github.com/user-attachments/assets/5e3571e0-9a73-487d-8c0d-664a31ee576e" />

googlewebstore.google.com will always show as clean but since the sender of the mail did not come from one of the domains available on the spf records of okc and neither did it pass the DMARC checks. 

## Verdict
The sender's email and IP does not correspond with the official domain of the organization it portrays to be. The country of origin of the source IP raises some suspicions. The mail and the link to the `googlewebstore` look legit but as to whether the extension is safe to use without collecting information of an individual is unknown. 
Based on the origin, as a security analyst I will flag the mail as a phishing mail and escalate for further analysis and triage. 

## Recommendations
It is imperative to always take not of the sender of a mail and pair it with the domain of the message of the mail. Also users should always be careful and skeptical about mails that instruct them to perform some tasks that will make them add extensions or perform any downloads into their systems. Again, some links may lead to legitimate sites but it clearly does not mean they are harmless. Some attackers usually spoof their domains and even take advantage of the institution's offerings, ie., online store.

## Author 
Kelvin Agyapong Debrah
Cybersecurity Analyst 
