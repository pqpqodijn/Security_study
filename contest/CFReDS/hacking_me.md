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
      1. 나는 Encase image파일을 다운받았기 때문에 CFReDS portal 사이트에 있는 HackingCase_E01_hashes.html(encase image관련 파일)에 적힌 MD5 hash값들과 내가 다운 받은 파일들의 MD5값을 hashcalc 프로그램으로 계산함 -> 일치
	  2. 혹시 모르니 FTK imager에서 drive/image verify results를 통해서도 hash 확인함 -> 일치
	  	-> 943243e71eda7481fee7b83f06698993(E01), 4931253cc91dffdef5867c3dfbd99516(E02)
      
  ### Q2. What operating system was used on the computer?
      1. os에 관해서 물어보는 것이니 root/Windows/system32/config에서 software(registry)파일 추출(log파일들도 같이 있으면 dirty한 상태이니 clean하게 rla로 합쳐줘야함)
	  2. regisrty explorer로 분석하는데 mincrosoft/windows nt/currentversion에서 확인
	  	-> Windows XP

  ### Q3. When was the install date?
      1. 2번 문제에서 본 폴더에 install date 정보도 들어있음 -> 1092955707(Unix 변환 필요) -> 08/19/04 10:48:27 PM -> 그러나 시간대가 다름
	  2. 시간대가 다르니 image는 UTC로부터 얼만큼 변경해야하는지 system 레지스토리에서 controlset001/control/timezoneinformation의 active timebias(실제 적용시간)이 500인 것을 확인
	  	-> 08/19/04 05:48:27 PM
			• 왜 standardName이 정답이 아닌가?
				• activetimebias를 기준으로 
					ActiveTimeBias == Bias이면 표준시 적용 (standardName)
					ActiveTimeBias == Bias + DaylightBias이면 서머타임 적용(daylightName)
      
  ### Q4. What is the timezone settings?
      1. 3번 문제에서 얻은 정보들로 풀 수 있음
	  	-> Central Daylight Time(-5 hours)

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


