# [endpoint] Hacking me

* **Platform / Source**: CFReDS
* **Category**: endpoint(disk)
* **Target File / URL**: https://cfreds-archive.nist.gov/Hacking_Case.html


On 09/20/04 , a Dell CPi notebook computer, serial # VLQLW, was found abandoned along with a wireless PCMCIA card and an external homemade 802.11b antennae. It is suspected that this computer was used for hacking purposes, although cannot be tied to a hacking suspect, G=r=e=g S=c=h=a=r=d=t. (The equal signs are just to prevent web crawlers from indexing this name; there are no equal signs in the image files.)  Schardt also goes by the online nickname of “Mr. Evil” and some of his associates have said that he would park his vehicle within range of Wireless Access Points (like Starbucks and other T-Mobile Hotspots) where he would then intercept internet traffic, attempting to get credit card numbers, usernames & passwords.

 Find any hacking software, evidence of their use, and any data that might have been generated. Attempt to tie the computer to the suspect, G=r=e=g S=c=h=a=r=d=t.

 A DD image (in seven parts: 1, 2, 3, 4, 5, 6, 7, 8, and notes) and a EnCase image (second part) of the abandoned computer have already been made.
 
 -> Greg Schardt의 해킹을 분석하기
---


  ### Q1. What is the image hash? Does the acquisition and verification hash match?
      1. 
      
  ### Q2. What operating system was used on the computer?
      1. 

  ### Q3. When was the install date?
      1. 
      
  ### Q4. What is the timezone settings?
      1. 

  ### Q5. Who is the registered owner?
      1. 
  
  ### Q6. What is the computer account name?
      1.

  ### Q7. What is the primary domain name?
      1. 
      
  ### Q8. When was the last recorded computer shutdown date/time?
      1. 

  ### Q9.  How many accounts are recorded (total number)?
      1. 
      
  ### Q10. What is the account name of the user who mostly uses the computer?
      1. 

  ### Q11. Who was the last user to logon to the computer?
      1. 
  
  ### Q12. A search for the name of “G=r=e=g S=c=h=a=r=d=t” reveals multiple hits. One of these proves that G=r=e=g S=c=h=a=r=d=t is Mr. Evil and is also the administrator of this computer. What file is it? What software program does this file relate to?
      1.

  ### Q13. List the network cards used by this computer
      1.

  ### Q14. This same file reports the IP address and MAC address of the computer. What are they?
      1. 
  
  ### Q15. An internet search for vendor name/model of NIC cards by MAC address can be used to find out which network interface was used. In the above answer, the first 3 hex characters of the MAC address report the vendor of the card. Which NIC card was used during the installation and set-up for LOOK@LAN?
      1.

  ### Q16. Find 6 installed programs that may be used for hacking.
      1.

  ### Q17. What is the SMTP email address for Mr. Evil?
      1. 
      
  ### Q18. What are the NNTP (news server) settings for Mr. Evil?
      1. 

  ### Q19. What two installed programs show this information?
      1. 
      
  ### Q20. List 5 newsgroups that Mr. Evil has subscribed to?
      1. 

  ### Q21.  A popular IRC (Internet Relay Chat) program called MIRC was installed.  What are the user settings that was shown when the user was online and in a chat channel?
      1. 
  
  ### Q22. This IRC program has the capability to log chat sessions. List 3 IRC channels that the user of this computer accessed.
      1.

  ### Q23. Ethereal, a popular “sniffing” program that can be used to intercept wired and wireless internet packets was also found to be installed. When TCP packets are collected and re-assembled, the default save directory is that users \My Documents directory. What is the name of the file that contains the intercepted data?
      1. 
      
  ### Q24. Viewing the file in a text format reveals much information about who and what was intercepted. What type of wireless computer was the victim (person who had his internet surfing recorded) using?
      1. 

  ### Q25. What websites was the victim accessing?
      1. 
      
  ### Q26. Search for the main users web based email address. What is it?
      1. 

  ### Q27. Yahoo mail, a popular web based email service, saves copies of the email under what file name?
      1. 
  
  ### Q28. How many executable files are in the recycle bin?
      1.

  ### Q29. Are these files really deleted?
      1.

  ### Q30. How many files are actually reported to be deleted by the file system?
      1. 
  
  ### Q31. Perform a Anti-Virus check. Are there any viruses on the computer?
      1.





		

### 소감
1. 사실 pcap관련된 문제들은 나만의 문제 해결 가이드를 만들지 못했는데 이번 기회에 만듬
     • 기간, 패킷 수, 링크 타입은?	Statistics → Capture File Properties
     • 어떤 프로토콜이 섞여 있나?	Statistics → Protocol Hierarchy
     • 누가 누구와, 얼마나 주고받나?	Statistics → Conversations (TCP/UDP 탭, Bytes 기준 정렬)
     • 포트와 내용이 맞는가? 내부↔내부 직접 통신이 있는가?	Conversations 결과를 보고 가설 세우기
                                                        (port 마다 정해진 사용법이 있는데 port와 내용이 다른지 확인)
                                                        (ex) 443port → TLS
     • 실제로 오간 데이터는?	우클릭 → Follow → TCP Stream
     • 파일이 오갔다면 꺼내서 해시 계산	Export Objects / Follow Stream → Raw 저장 → md5sum
2. wireshark가 pacp에 대해서 계층별 해석하는 방식을 이해했고 이 과정에서 오류로 해석될 수 있다는 점도 배움
3. port 마다 대부분 대응되는 protocol이 있다는 점을 배움

• 


