
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
      1. 일단 문제의 의도부터 해석하기 위해 검색해보니 "특정 네트워크 포트를 열어두고 외부의 연결 신호를 기다리고 있는 상태"로 확인
	  2. 여기에는 Bind shell(피해자가 포트 열리고 공격자가 침투)와 Reverse shell(공격자가 포트 열고 피해자가 들어옴)는 두개로 구분되어 진다.
	  3. wireshark에서 통계로 IP간의 통신 횟수를 분석하니 192.168.2.5(52242)와 192.168.2.244(4444)두개의 IP가 압도적으로 많아 의심
	  4. listening on port이니 대기중이다 syn받은 port가 정답
	  	-> 4444(처음 SYN을 보낸게 4444가 아니다보니 reverse shell로 추정)
        
  ### Q11. What is the port for the second shell? (shell.pcanpng)
      1. 10번 패킷을 tcp 따라가기로 전체 내용 분석하면 도중에 이상한점을 발견함
	  2. echo "*umR@Q%4V&RC" | sudo -S nc -nvlp 9999 < /etc/passwd 이 내용이 공격자가 피해자에게 보내고 피해자가 실행하는 것을 알 수 있음
	  3. netcat에서 -l(listenin 모드) -p [~](포트설정)으로 9999포트를 listen으로 피해자의 만든 것을 확인할 수 있음(bind shell하려는 목적으로 추측)
	  	-> 9999

  ### Q12. What version of netcat is installed? (shell.pcanpng)
      1. tcp 따라가기로 분석하다보면 The following NEW packages will be installed: netcat을 찾아 netcat을 피해자 컴퓨터에 설치하려는 모습이 보임
	  	-> 1.10-41.1
	  
  ### Q13. What file is added to the second shell? (shell.pcanpng)
      1. echo "*umR@Q%4V&RC" | sudo -S nc -nvlp 9999 < /etc/passwd이 명령어가 shell에서 실행되는 것을 보임
	  	 # ehco "*umR@Q%4V&RC" | : 피해자 비번 입력(sudo 사용을 위해), < /etc/passwd : 포트 연결시 이 파일 입력으로 넣음
		-> /etc/passwd 

  ### Q14. What password is used to elevate the shell? (shell.pcanpng)
      1. shell 권한 올리는 비밀번호도 13번 문제에서 확인 가능
	  	-> *umR@Q%4V&RC

  ### Q15. What is the codename of the target system's OS version? (shell.pcanpng)
      1. 처음에는 OS에 우분투가 해당되는지 까먹고 계속 찾다가 깨달음
	  2. 내용을 보다보면 http://us.archive.ubuntu.com/ubuntu bionic/universe amd64 netcat all 1.10-41.1 [3,436 B] 확인함
	  3. ubuntu bionic을 분석하니 ubuntu의 버전 이름인 것을 확인
	  	-> bionic
	  
  ### Q16. How many users are on the target system? (shell.pcanpng)
      1. target system 즉 아까 공격자가 14번 문제에서 꺼내가려는 파일을 확인하면 될 것으로 추측
	  2. 해당  "Connection from 192.168.2.244 34972 received!"의 바로 다음 패킷의 len 길이(즉 내용 데이터의 크기)가 매우 큰 것을 확인
	  3. 해당 데이터를 추출 후 분석해보니 "qqroot:x:0:0:root:/root:/bin/bash" 같은 형식으로 여러개 있음(etc/passwd의 계정 목록)
	  	-> 31

  ### Q17. What is the IPv6 NTP server IP? (network.pcanpng)
      1. NTP(network time protocol)로 시간 동기화를 위해 사용하는 프로토콜을 기준으로 필터링함
	  2. 여러개의 패킷이 나오는데 조건의 IPv6는 2개의 패킷만 있고 여기서 서버 IPv6를 찾으면 됨
	  	-> 2003:51:6012:110::dcf7:123
        
  ### Q18. What is the first IP address that is requested by the DHCP client? (network.pcanpng)
      1. DHCP(dynamic host configuration protocol)를 기준으로 필터링해서 검색
	  2. 시간순으로 볼 때 처음에 request한 IP는 Option: (50) Requested IP Addressd로 확인
	  	-> 192.168.20.11

  ### Q19. What is the first authoritative name server returned for the domain that is being queried? (network.pcanpng)
       1. 쭉 내려서 분석하거나 DNS를 기준으로 필터링하면 처음으로 blog.webernetz.net에 대해서 standard query한게 보임
	   	-> ns1.hans.hosteurope.de

  ### Q20. What is the number of the first VLAN to have a topology change occur? (network.pcanpng)
      1. topology change에 대해서 물으니 빈도수가 많은 STP와 관련된 것으로 추측
	  2. STP를 검색하니 spanning tree protocol로 loop를 방지하는 2계층 모델이라는 것을 확인
	  3. STP필터링 후 BPDU(bridge protocol data unit) flags 즉 데이터를 주고받는 bridge가 변경되면 이 BPDU flags의 최하위 비트가 1로 바뀜
	  4. 42번 프레임이 변한 것으로 확인, 여기의 vlan을 찾으면 완료
	  	-> 20

  ### Q21. What is the port for CDP for CCNP-LAB-S2? (network.pcanpng)
      1. CDP(cisco discovery protocol)은 cisco에서 개발안 2계층 프로토콜임, cdp를 기준으로 필터링
	  2. 조건의 ccnp-lab-s2는 133프레임에서 처음으로 발견 가능, 여기의 port ID를 확인하면 됨
	  	-> GigabitEthernet0/2
				
  ### Q22. What is the MAC address for the root bridge for VLAN 60? (network.pcanpng)
      1. vlan.id == 60을 조건으로 필터링 후 spanning tree protocol-> root identifier에서 확인할 수 있음(즉 BPDU 내용안에 있음)
	  	-> 00:21:1b:ae:31:80	

  ### Q23. What is the IOS version running on CCNP-LAB-S2? (network.pcanpng)
      1. 21번 문제처럼 CDP를 기준으로 필터링 후 ccnp-lab-s2를 선택
	  2. cisco discovery protocol -> Software version -> IOS 적힌거 찾기, version확인 가능
	  	-> 12.1(22)EA14
	  
  ### Q24. What is the virtual IP address used for HSRP group 121?	(network.pcanpng)
      1. HSRP를 검색하니 (hot standard router protocol)로 여러 라우터들이 하나의 라우터처럼 가상의 그룹으로 묶고 송신자 입장에서는 하나의 가상 IP로 보이는 protocol임.
	  2. HSRP2.group == 121로 필터링 후 virtual IP address 확인
	  	-> 192.168.121.1

  ### Q25. How many router solicitations were sent? (network.pcanpng)
      1. HSRP에서 solicitations(권유)는 살아있는지 확인을 보내는 것으로 인터넷에서 확인함
	  2. 이를 구분하는 방법 또한 찾아보니 hsrp for ipv6에서 호스트가 게이트웨이를 찾을 때 icmpv6(Internet control message protocol version 6)를 사용하는 것을 확인
	  3. icmpv6.type == 133(호스트가 라우터가 누구인지 묻는 조건)으로 검색
	  	-> 3

  ### Q26. What is the management address of CCNP-LAB-S2? (network.pcanpng)
      1. cdp로 필터링 후 cisco discovery protocol-> management address에 가면 나옴
	  	-> 192.168.121.20

  ### Q27. What is the interface being reported on in the first SNMP query? (network.pcanpng)
      1. snmp(simple network managemnet protocol)은 네트워크 상 장비들을 관리하는 프로토콜임
      2. snmp로 필터링 후 요청과 응답이 짝지어서 여러 있는 것을 확인 및 분석 방법을 인터넷에 찾아봄
	  3. 각 패킷은 simple network management protocol -> data -> get response -> variable-bindings로 가면 질문들과 그에 대항 응답을 확인할 수 있음
	  4. 1.3.6.1.2.1.31.1.1.1.().2는 공통이고 ()안 숫자에 따라 질문이 바뀜
	  	1 -> 인터페이스 이름, 6 -> 수신한 총 바이트, 10 -> 송신한 총 바이트
	  5. get-response가 적힌 응답 패킷에서 인터페이스 이름 확인함
	  	-> FA0/1
	  
  ### Q28. When was the NVRAM config last updated? (network.pcanpng)
      1. nvram(non volatile random access memory)인데 이걸 어디서 찾는지는 몰라서 힌트를 봄
	  2. 내부 바이트로 설정 후 nvram키워드를 검색 시 나옴
	  	-> 2017-03-03 21:02
	        
  ### Q29. What is the IPv6 of the RADIUS server? (network.pcanpng)
      1. 28번 문제처럼 radius로 검색하니 "aaa group server radius foobar 및 server name blubb"로 나오고 인터넷에 검색하니 foobar라는 radius 그룹서버를 설정한 후 거기에 blubb 서버를 넣어버린 듯 함 
	  2. bubble를 패킷 바이트에서 검색하니 "radius server blubb address ipv6 2001:DB8::1812 auth-port 1812 acct-port 1813" 이 문장 발견
	  	-> 2001:DB8::1812

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
       

