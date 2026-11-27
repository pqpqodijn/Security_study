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
      1. os에 관해서 물어보는 것이니 root\Windows\system32\config에서 software(registry)파일 추출(log파일들도 같이 있으면 dirty한 상태이니 clean하게 rla로 합쳐줘야함)
	  2. regisrty explorer로 분석하는데 mincrosoft\windows nt\currentversion에서 확인
	  	-> Windows XP

  ### Q3. When was the install date?
      1. 2번 문제에서 본 폴더에 install date 정보도 들어있음 -> 1092955707(Unix 변환 필요) -> 08/19/04 10:48:27 PM -> 그러나 시간대가 다름
	  2. 시간대가 다르니 image는 UTC로부터 얼만큼 변경해야하는지 system 레지스토리에서 controlset001\control\timezoneinformation의 active timebias(실제 적용시간)이 500인 것을 확인
	  	-> 08/19/04 05:48:27 PM
      
  ### Q4. What is the timezone settings?
      1. 3번 문제에서 얻은 정보들로 풀 수 있음
	  	-> Central Daylight Time(-5 hours)
			• 왜 standardName이 정답이 아닌가?
				• activetimebias를 기준으로 
					ActiveTimeBias == Bias이면 표준시 적용 (standardName)
					ActiveTimeBias == Bias + DaylightBias이면 서머타임 적용(daylightName)

  ### Q5. Who is the registered owner?
      1. 처음에는 software의 profileList파일에 있는 명단 중 한명이라고 생각해서 가보니 Evil유저 한명 있었음 -> 정답 아님
	  2. 그래서 인터넷에 registered owner에 대해서 검색해보니 이는 설치할 때 적은 이름표 즉 windows 운영체제에 등록된 소유자에 대해서 물어본 것
	  		• profileList : 실제로 컴퓨터에 로그인 해서 로그인을 시도하면서 만들어진 프로필 모음
			• SAM\domains\account\users : 로그인 시도를 하지 않았던 계정들 전부 포함한 list
	  3. 2번 문제에서 확인했던 파일에 registered owner 칸 살펴보면 됨
	  	-> Greg Schardt
  
  ### Q6. What is the computer account name?
      1. computer의 자체에 관한 이름을 물어보는 것이니 system registry에서 찾아야함
	  2. controlset이 001과 002가 있기에 인터넷에 왜 두개가 있는지 검색해봄
	  		• 이는 컴퓨터가 부팅하기 위해 만들어둔 백업본이고 현재 사용하고 있는건 system\select의 current 값에서 확인 가능함 -> 여기서는 001사용중
	  3. controlset001\control\computername\computername 파일에 들어가면 확인할 수 있음
	  	-> N-1A9ODN6ZXK4LQ

  ### Q7. What is the primary domain name?
      1. 처음에는 domain이라길래 DNS에서의 domain을 말하는 것인가 싶었음 -> 아무리 생각해도 뜬금 없어서 인터넷에 검색함
	  		• 여기서의 domain은 windows의 domain을 말하는 것임 (대학교에서 내 정보가 중앙 서버에 있다고 가정하면 중앙 서버가 domain controler이고 내 학생 정보(domain)로 대학교에 존재라는 컴퓨터들로 로그인이 가능한 것)
			  또한 대학교 내의 컴퓨터들(중앙 서버의 domain에 가입된 컴퓨터들)은 domain controler에서 모두다 관리 가능하다고 이해하면 됨
	  2. windows 관련이니 software\mincrosoft\windows nt\currentversion 까지는 생각했지만 그 이후를 모르겠어서 찾아보니 winlogon파일에 있는 것을 확인함
	  		• defaultusername의 값을 확인하면 됨
	  	-> Mr. Evil
      
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


