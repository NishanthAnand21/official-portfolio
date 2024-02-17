---
title: "OSINT Challenges in Encrypt Edge"
tags: ["OSINT", "CTF", "Cybersecurity"]
style: 
color: 
image: "/assets/images/cs50/cover.jpg"
description: "In the world of cybersecurity, OSINT (Open Source Intelligence) plays a crucial role. OSINT involves gathering, analyzing, and utilizing intelligence from publicly available sources, such as information found on the internet."
permalink: osint-challenges-in-encrypt-edge
---

# OSINT Challenges in Encrypt Edge

![Cover](/assets/images/osint/cover.png)

**Let's first learn what is OSINT**

Let's start by defining what open-source means. **OSINT** is short for "open source intelligence". When we say "open source", we refer to any publicly available source that anyone can access legally and without violating any copyright, patent, or privacy laws. For most people, this means content that is freely available on the internet. Now that we understand what open-source means. Open-source intelligence (OSINT) refers to any intelligence that is created from open-source information and is then collected, analyzed, and communicated to the appropriate audience in a timely manner to meet a specific intelligence requirement. Information can be in various forms like audio, video, image, text, file etc. Here's a list of the available data categories that can be found on the internet:
1.  Social media platforms such as Twitter and Facebook hold vast amounts of user data. 

1. Public-facing web servers, which contain information about different individuals and organizations, are also sources of data. 
2. Additionally, newsletters, articles, and software/code repositories like CodeChef and GitHub are other sources of information, although we only access what we specifically search for.

There is a wealth of open-source information available online that can be used to gather information about a target regarding cyber security. Let's focus on the CTF (Capture The Flag) perspective. There are various **OSINT** tools available online, some of which are popular and listed below:

## **OSINT Tools 
1. Google (free! if you know how to use it properly)
2. [Shodan.io](http://Shodan.io) (A database that lists millions of internet-connected devices)
3. Have I Been Pwned (Data Breaches Information)
4. ExifTool ( Metadata)
5. Whois (Get info on domain names, IPs, and network devices registered with ICANN.)**

**Let's now explore the world of CTFs.**

### **Challenges**

### 1. **3NUM3RA73_m3**

Level: Easy
Points: 50

**CREATED BY: JOY_4_U**

[acad1@cyberlearning.org](mailto:acad1@cyberlearning.org)

Verify if this email exists or not if yes then Using OSINT tools to Enumerate it find the Registry Domain ID and submit it as flag {Registry_Domain_ID}
Solution:

Here we can use the popular tool called **Whois**
we consider [cyberlearning.org](http://cyberlearning.org) as the domain
So, now open the terminal and type this command then you will find the Registry Domain ID.

```bash
─$ whois cyberlearning.org
Domain Name: cyberlearning.org
Registry Domain ID: 9fb3dc8e87c9451eb3dd2c171c337006-LROR
Registrar WHOIS Server: http://whois.godaddy.com
Registrar URL: http://www.whois.godaddy.com
Updated Date: 2023-08-17T16:41:07Z
Creation Date: 1996-09-25T04:00:00Z
Registry Expiry Date: 2025-09-24T04:00:00Z
Registrar: GoDaddy.com, LLC
Registrar IANA ID: 146
Registrar Abuse Contact Email: abuse@godaddy.com
Registrar Abuse Contact Phone: +1.4806242505
Domain Status: clientDeleteProhibited https://icann.org/epp#clientDeleteProhibited
Domain Status: clientRenewProhibited https://icann.org/epp#clientRenewProhibited
Domain Status: clientTransferProhibited https://icann.org/epp#clientTransferProhibited
Domain Status: clientUpdateProhibited https://icann.org/epp#clientUpdateProhibited
Registry Registrant ID: REDACTED FOR PRIVACY
Registrant Name: REDACTED FOR PRIVACY
Registrant Organization: Domains By Proxy, LLC
Registrant Street: REDACTED FOR PRIVACY
Registrant City: REDACTED FOR PRIVACY
Registrant State/Province: Arizona
```

There you go! we got the Registry Domain ID: 9fb3dc8e87c9451eb3dd2c171c337006-LROR

Therefore the flag is **flag{9fb3dc8e87c9451eb3dd2c171c337006}**

2. **Gum1's_v1br4nt_h34rt** 

Level: Easy
Points: 50
**CREATED BY: JOY_4_U**

Gumi is a fictional Character loved by People. The flag to capture lies hidden within a geotag, sending them on an exciting journey through the city's landmarks and culture. Can they unravel the puzzle and locate the hidden gem within Gumi's vibrant heart?

**[gumi.zip](https://play.encryptedge.in/files/9bf9d173332d695939e77e4cf92dd7d7/gumi.zip?token=eyJ1c2VyX2lkIjoxNDUsInRlYW1faWQiOm51bGwsImZpbGVfaWQiOjI3fQ.ZWcaqg.0oQCe90Ep1LAGNxKjshWDZytN1A)**

firstly download the zip file provided and then extract it you will get an image file

![Gumi](/assets/images/osint/gumi.png)

Read the information provided carefully then you will find a word **geotag,** go and search it on Google. You will see a link at the top of your search results [**https://tool.geoimgr.com/**](https://tool.geoimgr.com/)

[https://tool.geoimgr.com/](https://tool.geoimgr.com/)

Geotagging is nothing but adding geographical metadata to any media.

Open the website and just upload the image over there and wait for the results

![Results](/assets/images/osint/results.png)

voila, there you go there is the flag!
**flag{UWU_G07_M3}**

**3. C1ph3r_M1nd (Medium, 100 points)
CREATED BY: JOY_4_U**

As aspiring digital sleuths delved into the enigmatic world of Freddie, the legendary South African rock sensation, they encountered a unique challenge. To unveil the hidden treasure, they needed to embark on a journey reminiscent of Freddie's iconic performances. Just like his stage persona defied norms, participants had to think outside the box. Let's Find Freddie's Remote Server IP which has an RDP connection to the Windows server and submit the flag as flag{base64_of_IP}

[https://ciphermind1.wordpress.com/](https://ciphermind1.wordpress.com/)

Firstly, read the information carefully! after reading visit the website provided.
We can use [Shodan.io](http://Shodan.io) here to get the remote server IP address of Freddie Van Niekerk, South Africa, Johannesburg (got from the website provided)
Simply navigate to [**shodan.io**](http://shodan.io), search with these keywords and you will get the IP Address of the server.
Encode it using [**cyberchef](http://cyberchef.io).io** to base64 and submit the flag.

### 4. **F1nd_My_P0k3M0n**

Level: Medium
Points: 100

**CREATED BY: JOY_4_U**

Let's help Ash to find his lost Pokeball, Yesterday he went to a Movie store in the Poke Center where during enjoyment he lost his Pokeball of his Pikachu. Using this image locate the Movie theatre and submit the Note: Don't add the name of the mall into the flag only use the Movie Store name Also Remove all the blank spaces from the name flag{base64_of_Movie_store_name}.

![pikachu](/assets/images/osint/pika.jpg)

Read carefully. Analyze image.

It is essential to identify the name of the movie store and convert it into a base64 string to retrieve the flag. An easy way to do this is by using Google Lens to scan the image. Once the scan is complete, similar images will be displayed, and by examining them, I was able to identify the store name. The store name is "Crayon Shin-chan Cinema Parade" and it is located behind the Pokemon character.

![Pikachu](/assets/images/osint/pikachu.png)

To convert the name to base64, we will use a popular site called [**cyberchef.io](http://cyberchef.io).** Simply copy and paste the store name, select "To Base64" and you will receive the output as a base64 string. The flag is: **flag{Q2luZW1hIFBhcmFkZQ==}.**

### 5. **Ohh.S1nt**

Level: Medium
Points: 100
**CREATED BY: JOY_4_U**

Lovely Faculty of Education in collaboration with the Educational Research Association (ERA) Punjab organized a World Conference between June and July 2010. There was a PressRelease Announcement for that Let's Osint the information to find the name of the AIAER Conference and submit as flag {Conference_Nam3}
Always, read the information carefully!

**Google Dorks help us discover hidden resources crawled by Google.**

> Google Dorks refer to specialized search queries that can be used to identify specific information on the internet. These queries can be used to uncover vulnerabilities, find exposed data, or search for specific file types, among other things.
> 

There are a lot of operators in the Google Dorks list here are the few popular operators are listed below:

| Operator | Function | Example |
| --- | --- | --- |
| cache: | Returns the cached version of a website | cache:techtarget.com |
| site: | Returns a list of all indexed https://www.techtarget.com/searchnetworking/definition/URL from a website or domain | site:techtarget.com |
| filetype: | Returns various kinds of files, depending on the file extension provided | filetype:pdf |
| inurl: | Searches for a specific term in the URL | inurl:register.php |
| allinurl: | Returns results whose URL contains all the specified characters | allinurl:clientarea |
| intext: | Locates webpages that contain certain characters or strings inside their text | intext:"Google Dork Query" |
| inanchor: | Searches for an exact anchor text used on any links | inanchor:"cyber attacks" |
| | | Shows all sites that contain either or both specified words in the query | hacking | Google dork |
| + | Concatenates words to detect pages using more than one specific key | hacking + Google dork |
| - | Used to avoid displaying results containing certain words | hacking - dork |

Here we used **“inurl:”** operator to search in the Google search, Here is the keyword used in the search **inurl:world Conference 2010 lpu**

![Google Dorking](/assets/images/osint/dork.png)

![Blog](/assets/images/osint/blog.png)

There you can see the Name of the Conference that is EDUCON-2010

yay! We solved this challenge the flag is **flag{EDUCON2010}**

Below are additional OSINT tools that were not mentioned:

1. Maltego
Maltego is an open-source application for forensics and intelligence that provides easy-to-understand information representation.
2. Spiderfoot
A tool that automatically gathers intelligence from over 100 public data sources (OSINT) on IP addresses, domain names, email addresses, and names.
3. Censys
A search platform is web-based and it helps to assess the attack surface of devices that are connected to the Internet.
4. Recon-ng
A powerful reconnaissance framework designed for efficient web-based open-source reconnaissance.
5. TheHarvester
This tool gathers subdomains, emails, virtual hosts, open ports/banners, and employee names from public sources such as search engines and PGP key servers.
6. ExploitDB
This is a tool that can be used to search for exploits in databases that contain them.
7. OSINT Framework
My main focus is to collect information using free tools and resources available.
8. Amass
Performs network discovery and mapping of attack surfaces, utilizing open-source information gathering and active reconnaissance methods to identify external assets.
9. Sherlock
This is an open-source Python tool that helps find social media usernames and is easily accessible on GitHub.
10. Sublist3r
A [Python](https://www.pynetlabs.com/python-for-network-engineers/) utility was made to list website subdomains using OSINT. 

If you want to try out these tools and get some hands-on experience, go ahead and give it a shot!
Our goal is to use hacking as a means of protection, not harm. We prioritize the ethical use of hacking techniques to secure systems and data, rather than using them for malicious purposes.
**Summary :**

In the world of cybersecurity, OSINT (Open Source Intelligence) plays a crucial role. OSINT involves gathering, analyzing, and utilizing intelligence from publicly available sources, such as information found on the internet. It is a valuable method for collecting data to meet specific intelligence requirements. OSINT can be obtained from various sources, including social media platforms, public-facing web servers, articles, and software repositories. There are several useful OSINT tools, such as Google, [Shodan.io](http://shodan.io/), Have I Been Pwned, ExifTool, and Whois, which aid in the collection and analysis of open-source information. OSINT skills are often put to the test in Capture The Flag (CTF) challenges, where participants use their knowledge and tools to solve puzzles and uncover hidden information. By leveraging OSINT effectively, cybersecurity professionals can enhance their investigations and strengthen their overall security posture.