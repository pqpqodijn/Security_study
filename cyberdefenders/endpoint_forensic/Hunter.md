# [endpoint] Hunter (medium)

* **Platform / Source**: cyberdefenders
* **Category**: endpoint
* **Target File / URL**: https://cyberdefenders.org/blueteam-ctf-challenges/hunter/

---


  ### Q1. What is the computer name of the suspect machine?
      1. 컴퓨터 이름은 레지스트리에 저장(System)
      2. controlset001\control\computername에 있음
        -> 4ORENSICS

  ### Q2. What is the computer IP?
      1. 컴퓨터에 할당된 IP를 찾는것으로 해석
      2. 컴퓨터에 관한 설정이라 SYSTEM 레지스트리, 그중 service에 있을 것으로 추측(computer 전체를 관리하는 설정값이 아니라서)
      3. controlset001\service\tcpip\parameters\interfaces  
        -> 10.0.2.15

  ### Q3. What was the DHCP LeaseObtainedTime?
      1. 2번 문제와 경로는 같으면서 바로 옆에 정답이 있음 -> 2016-06-21 12:58:06 (근데 정답이 아님)
      2. 다른 write up을 보니 방법은 같아서 dirty한 상태의 레지스트리를 분석함
	  	-> 21/06/2016 02:24:12 UTC(아마도 문제 제작자가 dirty한 상태에서 분석하도록 의도한 것 같음)
       
  ### Q4. What is the computer SID?
      1. FTK imager에서 쓰레기통에서 보거나 SOFTWARE 레지스트리중 mincrosoft\windows nt\currentversion\profilelist에서 확인 가능함
        -> S-1-5-21-2489440558-2754304563-710705792(SID랑 PID 구별해야함)

  ### Q5. What is the Operating System(OS) version?
      1. OS란 하드웨어랑 응용 소프트웨어를 관리하는 window, mac os 같은거라 SOFTWARE 레지스트리에서 분석
	  2. mincrosoft\windows nt\currentversion에서 product name 행에 window 버전이 몇인지 나옴
        -> 8.1	

  ### Q6. What was the computer timezone?
      1. 컴퓨터 시간 관련된 전체 관리하는데 필요한 정보라 SYSTEM 레지스트리에서 분석
	  2. controlset001\control\timezoneinformation에서 확인 가능
	  3. active time(실제 적용 시간)인 행에서 정보 얻음
	  	-> UTC-07:00

  ### Q7. How many times did this user log on to the computer?
      1. this user 즉 hunter의 로그인 횟수이니 개인적인 정보라 SAM 레지스트리에 있을 것임
	  2. Domain\account\users에서 hunter의 PID(1001)를 찾으면 total login이라는 열에 나와있음
	  	-> 3
	  
  ### Q8. When was the last login time for the discovered account? Format: one-space between date and time
      1. 이전 문제랑 같은 경로에 last login time이라는 열이 있음
	  	-> 2016-06-21 01:42:40

  ### Q9. There was a “Network Scanner” running on this computer, what was it? And when was the last time the suspect used it? Format: program.exe,YYYY-MM-DD HH:MM:SS UTC
      1. 일단 network scanner의 종류가 뭐가 있는지 검색해보니
	  	Wireless Network Watcher, SoftPerfect Network Scanner, Nmap / Zenmap 등.. 나옴
	  2. 잘 보면 ftk imager로 분석시 zenmap을 쉽게 발견할 수 있음
	  3. 프로그램 실행관련이니 prefetch를 FTK에서 추출 후 분석해봄(2016-06-21 오후 9:08:13)
	  4. 그러나 winprefetchview는 내 컴퓨터 기준으로 UTC + 라서 9를 뺀 시간을 정답으로 제출
	  	-> zenmap.exe,2016-06-21 12:08:13 UTC

  ### Q10. When did the port scan end? (Example: Sat Jan 23 hh:mm:ss 2016)
      1. zenmap이 포트 스캔을 했을 것이기에 이 기록을 찾아야함
	  2. FTK imager에서 발견한 zenmap 폴더에서 zenmap.db를 분석함(Tue Jun 21 05:12:18 2016 발견) -> 근데 정답이 아님
	  3. 다른 결과가 있는 것 같아서 FTK imager를 분석하다보니 desktop에 nmapscan.xml이라는 파일을 발견
	  	-> Tue Jun 21 05:12:09 2016(db파일에서 어디로 저장했다는 정보는 못찾음)
        
  ### Q11. How many ports were scanned?
      1. 이전 문제에서 발견한 nmapscan.xml 내용중 Completed SYN Stealth Scan at 05:11, 12.69s elapsed (1000 total ports) 라는 결과 정보가 있음
	  	-> 1000

  ### Q12. What ports were found "open"?(comma-separated, ascending)
      1. 11번 문제랑 마찬가지로 nmapscan.xml에서 어떤 포트가 open되었는지 결과 나옴 
        -> 22,80,9929,31337
  ### Q13. What was the version of the network scanner running on this computer?
      1. nmapscan.xml에서 version키워드로 검색하면 정보 나옴
	  	-> 7.1

  ### Q14. The employee engaged in a Skype conversation with someone. What is the skype username of the other party?
      1. skype의 사용 기록이 어디에 저장되는지 검색해보니 Users\사용자이름\AppData\Roaming\Skype\Skype사용자이름\main.db를 발견 후 분석함
	  2. chats 테이블에서 friends name열에서 이름 발견
	  	-> linux-rul3z

  ### Q15. What is the name of the application both parties agreed to use to exfiltrate data and provide remote access for the external attacker in their Skype conversation?
      1. main.db에서 messages 테이블에서 발견함 
	  	-> Teamviewer(맥락상 확인 가능)
  ### Q16. What is the Gmail email address of the suspect employee?
      1. FTK imager에서 제일 상위 폴더를 추출 후 autopsy로 logical files선택 후 분석하면 linux-rul3z로 메일을 보낸 사람의 이메일 주소를 확인 가능함
	  	-> ehptmsgs@gmail.com

  ### Q17. It looks like the suspect user deleted an important diagram after his conversation with the external attacker. What is the file name of the deleted diagram?
      1. 이메일을 지울 때 어느 폴더에 저장되거나 이름이 뭔지 검색해보니 결과중에 outlook사용자라는 특이한 내용을 발견함
	  2. outlook을 검색해보니 mincrosoft의 컴퓨터용 종합업무 프로그램이고 user\hunter\documents에 outlook파일을 발견
	  3. 내부에는 backup.pst파일을 발견함(outlook 전용 파일이라 이를 분석하는 SysTools Outlook PST Viewer.exe 설치)
	  	-> home-network-design-networking-for-a-single-family-home-case-house-arkko-1433-x-792.jpg
        
  ### Q18. The user Documents' directory contained a PDF file discussing data exfiltration techniques. What is the name of the file?
      1. pdf 파일을 분석중 내용 제목이 EXFILTRATION TECHNIQUES: AN EXAMINATION AND EMULATION인 폴더 발견
	  	-> Ryan_VanAntwerp_thesis.pdf

  ### Q19. What was the name of the Disk Encryption application Installed on the victim system? (two words space separated)
       1. 처음에는 피해자 시스템에 설치된 파일이니 대회 기록에 힌트가 있을거로 판단해 분석했지만 안나옴
	   2. 컴퓨터에 설치된 것은 아니니 분명 다른 무엇가에 기록이 남겨진 형태인데 잘 모르겠어서 write up을 참고함
	   3. BCwipe(찾아보니 데이터를 지우는 프로세스)의 기록을 보길래 분석함
	   4. encryption과 비슷한 crypto swap.lnk를 발견
	   	-> Crypto Swap

  ### Q20. What are the serial numbers of the two identified USB storage?
      1. USB또한 SYSTEM 레지스트리에 남기에 controlset001\enum\usbstor에 2개의 USB에 대한 serial number가 존재한다.
	  	-> 07B20C03C80830A9,AAI6UXDKZDV8E9OU

  ### Q21. One of the installed applications is a file shredder. What is the name of the application? (two words space separated)
      1. 이전 문제에서 찾은 bcwipe에 대해서 말하는 것 같은데 두 단어로 분리 된다는 점에서 정답은 아니라고 추측
	  2. prefetch를 마저 분석했지만 bcwipe에 대해서 두개의 단어로 구분되는 프로세스는 없음
	  3. 힌트를 확인하니 설치 경로나 공급업체에 대해서 적는 것이기에 상위 폴더와 같이 정답으로 제출
	  	-> Jetico BCWipe(그러나 문제에서 
				

  ### Q22. How many prefetch files were discovered on the system?
      1. winprefetch로 분석하면 총 190개의 파일이 발견됨 -> 답이 아님
	  2. 사이즈별로 나누니 사이즈가 0인 16개 발견 
	  	-> 174

  ### Q23. How many times was the file shredder application executed?
      1. winprefetch에서 bcwipe를 검색하면 run counter를 확인 가능함
	  	-> 5
	  
  ### Q24. Using prefetch, determine when was the last time ZENMAP.EXE-56B17C4C.pf was executed?
      1. winprefetch에서 검색 결과 last run time은 2016-06-21 오후 9:08:13로 나옴
	  2. 단 winprefetch는 분석하는 컴퓨터 기준으로 UTC가 정해지기에 -9 해야함
	  	-> 06/21/2016 12:08:13 PM

  ### Q25. A JAR file for an offensive traffic manipulation tool was executed. What is the absolute path of the file?
      1. JAR 파일(java파일)실행한거라 winprefetch로 보면 javaws.exe라는 자바 실행 프로세스를 발견
	  2. 근데 이게 뭔 자바 파일을 실행시킨건지는 확인 불가능함
	  3. 찾아보니 PECmd로 프로세스가 로드한 파일들을 확인 할 수 있는 것을 찾음
	  4. KAPE에서 PECmd로 prefetch파일 분석 후 "java"키워드로 거르고 ".jar" 키워드로 찾으면 발견할 수 있음
	  	-> C:\Users\Hunter\Downloads\Burpsuite_free_v1.7.03.jar

			
  ### Q26. The suspect employee tried to exfiltrate data by sending it as an email attachment. What is the name of the suspected attachment?
      1. email 내용이다보니 17번 문제처럼 분석하거나 autopsy에서 attachment를 분석하면 나옴
	  	->Pictures.7z
        

  ### Q27. Shellbags shows that the employee created a folder to include all the data he will exfiltrate. What is the full path of that folder?
      1. shellbags란 사용자가 특정 폴더를 수정하면 동일한 설정을 보여주기 위해 저장한 정보들로 분석 가치가 높은 정보임
	  2. autopsy에서 "exf" 키워드로 검색하면 exfil.lik 즉 바로가기가 나오는데 이 경로를 확인하면됨
	  	-> C:\Users\Hunter\Pictures\Exfil
	  
  ### Q28. The user deleted two JPG files from the system and moved them to $Recycle-Bin. What is the file name that has the resolution of 1920x1200?
      1. autopsy에서 recycle-bin 폴더를 보면 2개의 jpg중 하나만 타입이 image\jpg라는 것을 알 수 있음
	  2. 짤린 사진과 똑같은 완벽한 사진을 pictures폴더에서 찾으면 이름에도 1920x1200가 붙은 누가봐도 정답인 사진이 있음
	  	-> ws_Small_cute_kitty_1920x1200.jpg
	        
  ### Q29. Provide the name of the directory where information about jump lists items (created automatically by the system) is stored?
      1. 인터넷에 jumplist(즉 자주 사용한 폴더, 파일등)에 대한 정보가 어디에 있는지 찾아보니 (특히 자동으로) AutomaticDestinations 및 CustomDestinations 폴더에 저장됨을 확임
	  2. 경로는 appdata\roaming\mincrosft\windows\recent 내부에 있는 걸 알 수 있음
	  3. 자동으로 생성되는 거니 이름에 auto가 붙은 파일을 정답으로 선택
	  	-> AutomaticDestinations

  ### Q30. Using JUMP LIST analysis, provide the full path of the application with the AppID of "aa28770954eaeaaa" used to bypass network security monitoring controls.
      1. 
		

### 소감

       

