
# [endpoint] Wire DIve (medium)

* **Platform / Source**: cyberdefenders
* **Category**: network/
* **Target File / URL**: https://cyberdefenders.org/blueteam-ctf-challenges/wiredive

---


  ### Q1. What IP address is requested by the client? (dhcp.pcanpng)
      1. IP 요청시 진행 순서는 Discover -> Offer -> request -> ACK 로 나뉜다. 이중 요청(Discover)에 응답하여 서버가 주는 Offer 패킷을 살펴봐야함
	  2. your(client) IP address를 보면 서버가 제공한 IP 주소를 확인할 수 있음.
	  	-> 192.168.2.244

  ### Q2. What is the transaction ID for the DHCP release? (dhcp.pcanpng)
      1. transaction ID는 응답에 따른 질의들을 구분하기 위한 16비트 식별자임.
	  2. ctrl + f 로 내용중 release가 있는걸 검색하면 쉽게 확인 가능
	  	-> 0x9f8fa557

  ### Q3. What is the MAC address of the client? (dhcp.pcanpng)
      1. 1번문제의 관련된 패킷들 내용을 확인하면 Client MAC address 부분에서 정답에 대한 정보를 찾을 수 있음
       	-> 00:0c:29:82:f5:94
		
  ### Q4. What is the response for the lookup for flag.fruitinc.xyz? (dns.pcanpng)
      1. flag.fruitinc.xyz에서  TXT에 관해서 찾는 것을 질문(23번 패킷)을 통해 확인
	  2. 응답 패킷(24번 패킷)에서 Answer의 내용을 자세히 보면 뭐가 적혀있는지 알 수 있음
	  	-> ACOOLDNSFLAG

  ### Q5. Which root server responds to the google.com query? Hostname. (dns.pcanpng)
      1. 1~8번 패킷을 분석해보면 처음에는 루트의 NameServer들을 확인 후 그중 한 곳에 google.com에 관해서 질문해봄
	  2. 질문한 패킷의 destination이 여러 루트서버중 정답
	  3. 192.203.230.10을 인터넷에 검색하니 hostname을 확인할 수 임
	  	-> e.root-servers.net
	
  ### Q6. What is the path of the file that is opened? (smb.pcanpng)
      1. 이번 파일에서는 SMB2 protocol이 많아서 검색해보니 네트워크에서 파일이나 프린터 공유(공유폴더 같은)시 사용하는 프로토콜임
	  2. 패킷을 보면 share(공유폴더)를 열고 정보들을 확인해 원하는 파일 내용을 찾는 것으로 확인됨
	  3. 마지막에 read명령어가 있던 파일 경로가 답으로 생각됨
	  	-> HelloWorld\TradeSecrets.txt
	  
  ### Q7. What was the hex status code when the user SAMBA\jtomato logs in? (smb.pcanpng) 
      1. 패킷에 log에 대한 문자열로 검색결과 SAMBA\jtomato에 로그인 시도한 패킷을 찾음
	  2. 실패라고 응답한 패킷에서 NT status를 확인하면 알 수 있음
	  	-> 0xc000006d
	  
  ### Q8. What is the tree that is being browsed? (smb.pcanpng)
      1. smb2로 필터링해 분석하다 보면 Tree에 connect하려는 패킷들이 보임
	  2. 실패한 패킷 말고 성공해서 share에 접근한 tree가 정답
	  	-> \\192.168.2.10\public

  ### Q9. What is the flag in the file? (smb.pcanpng)
      1. 읽은 파일(TradeSecrets.txt) 내부에 있을 것으로 추측
	  2. Read 명령 패킷의 응답들을 복사후 분석(flag를 기준으로 찾으면 빠름)
	  3. flag<~~>형식으로 적힌 것을 찾음
	  	-> OneSuperDuperSecret

  ### Q10. What port is the shell listening on? (shell.pcanpng)
      1. 
        
  ### Q11. What is the port for the second shell? (shell.pcanpng)
      1. 

  ### Q12. What version of netcat is installed? (shell.pcanpng)
      1. 
	  
  ### Q13. What file is added to the second shell? (shell.pcanpng)
      1. 

  ### Q14. What password is used to elevate the shell? (shell.pcanpng)
      1. 

  ### Q15. What is the codename of the target system's OS version? (shell.pcanpng)
      1. 
	  
  ### Q16. How many users are on the target system? (shell.pcanpng)
      1. 

  ### Q17. What is the IPv6 NTP server IP? (network.pcanpng)
      1. 
        
  ### Q18. What is the first IP address that is requested by the DHCP client? (network.pcanpng)
      1. 

  ### Q19. What is the first authoritative name server returned for the domain that is being queried? (network.pcanpng)
       1. 

  ### Q20. What is the number of the first VLAN to have a topology change occur? (network.pcanpng)
      1. 

  ### Q21. What is the port for CDP for CCNP-LAB-S2? (network.pcanpng)
      1. 
				
  ### Q22. What is the MAC address for the root bridge for VLAN 60? (network.pcanpng)
      1. 

  ### Q23. What is the IOS version running on CCNP-LAB-S2? (network.pcanpng)
      1. 
	  
  ### Q24. What is the virtual IP address used for HSRP group 121?	(network.pcanpng)
      1. 

  ### Q25. How many router solicitations were sent? (network.pcanpng)
      1. 

  ### Q26. What is the management address of CCNP-LAB-S2? (network.pcanpng)
      1. 

  ### Q27. What is the interface being reported on in the first SNMP query? (network.pcanpng)
      1. 
	  
  ### Q28. When was the NVRAM config last updated? (network.pcanpng)
      1. 
	        
  ### Q29. What is the IPv6 of the RADIUS server? (network.pcanpng)
      1. 

  ### Q30. What has been added to web interaction with web01.fruitinc.xyz? (https.pcanpng)
      1. 

  ### Q31. What is the name of the photo that is viewed in slack? (https.pcanpng)
      1. 

  ### Q32. What is the username and password to login to 192.168.2.1? (https.pcanpng)
      1. 
	  
  ### Q33. What is the certStatus for the certificate with a serial number of 07752cebe5222fcf5c7d2038984c5198? (https.pcanpng)
      1. 

  ### Q34. What is the email of someone who needs to change their password? (https.pcanpng)
      1. 
	  
  ### Q35. A service is assigned to an interface. What is the interface, and what is the service? (https.pcanpng)
      1. 
		

### 소감
1. Jumplist, shellbags와 같은 새로운 컴퓨터 내부 정보들도 새로 알게되는 방면에 outlook, skype, zenmap, bcwipe와 같은 처음 들어보는 어플에 관해 정보를 분석하는 방법을 배우게 되었다. 
2. 특히 25번 문제를 풀면서 프로그램이 실행한 파일에 대해서 분석하는 사고 과정은 처음이라 생소하고 신기했다.
3. 처음으로 autopsy를 이용하면서 문제를 푸니 생각보다 레지스트리를 분석하거나 시간이 많이 걸리는 부분들이 간단히 찾아지니 좋았고 이중 분석하기도 용이했다.
4. 다만 이렇게 문제를 풀면서 인터넷을 참고하며 푸는 비중이 아직 있고 단시간 내에 빨리 풀지 못한다는 점이 아직 많이 공부해야하는 것 같다.			         												 (참고로 2번 풀면서 write up은 2번째로 풀때 작성하는데도 잘 못푸는 문제도 몇개 있었다...)
5. 계속 문제만 풀다보니 무언가 아는 정보는 많아지지만 전문 지식이 채워지지 않는 것 같아서 디지털 포렌식 전문가 2급 자격증을 병행하며 공부해야겠다. (특히 복학하면 앞으로 배우는 내용과 겹처서 선행겸...)
       

