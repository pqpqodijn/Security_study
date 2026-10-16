# [endpoint] corporatesecrets (medium)

* **Platform / Source**: cyberdefenders
* **Category**: endpoint
* **Target File / URL**: https://cyberdefenders.org/blueteam-ctf-challenges/corporatesecrets/

---

  ### Q1. What is the current build number on the system?
      1. current build number란? -> OS가 수정되거나 개발되는 과정에서 생긴 고유 번호(os 관련 설정이라 software)
      2. software 레지스트리에서 root\mincrosoft\windows nt\currentversion을 클릭하면 확인 가능
        -> 16299  

  ### Q2. How many users are there?
      1. FTK imager에서 User 폴더에 6명 있음

  ### Q3. What is the CRC64 hash of the file "fruit_apricot.jpg"?
      1. fruit_apricot.jpg : hansel.apricot가 보유한 jpg파일
      2. 파일 추출후 인터넷에 crc64 계산기에 돌리니 
        -> ed865aa6dfd756bf

  ### Q4. What is the logical size of the file "strawberry.jpg" in bytes?
      1. strawberry.jpg : suzy의 폴더에 있는 이미지
        -> FTK imager에서 Size는 72448이라고 나옴

  ### Q5. What is the processor architecture of the system? (one word)
      1. 컴퓨터의 프로세서 구조를 확인해야함 -> SYSTEM 레지스트리 내부에 있을 거임(그러나 자세한 경로는 몰라서 검색해봄)
      2. Registry exploer도구로 SYSTEM내부에서 controlSet001\Control\SessionManager\Environment(Sessionmanager 부분을 기억못했음)
        -> PROCESSOR_ARCHITECTURE : amd64
        

  ### Q6. Which user has a photo of a dog in their recycling bin?
      1. recycle.bin에서 1005번의 RID를 가진 유저폴더에 있는걸 확인(1000번 이상이면 유저들을 나타냄)
      2. 1005번이 누구인지 확인할려고 SOFTWARE 레지스트리에 접근
      3. 근데 자세한 경로는 몰라서 검색해보니 profiles.List에 나와있는걸 확인
        (mincrosoft\windows nt\currentversion\profiles.list) windows nt에 os와 관련된 설정들 있음
      -> 1005번은 hansel.apricot

  ### Q7. What type of file is "vegetable"? Provide the extension without a dot.
      1. FTK imgaer에서 miriam에게 vegetable파일 확인 및 추출
      2. Hxd로 헤더 시그니처 분석(37 7A BC AF)결과 7-zip파일인 것 확인
      -> 7z  
  ### Q8. What type of girls does Miriam Grapes design phones for (Target audience)?
      1. FTK imager로 miriam의 사진들 확인했지만 뭔가 발견은 못함
          (구조가 바뀐 파일 혹은 Steganography 종류는 아닌 것 같음) -> 힌트를 확인해보니 브라우저 기록을 확인하라고 함
      2. Miriam에게는 firefox 브라우저 확인 및 ~~.default-release\places.sqlite 파일에 브라우저 방문, 다운로드 등 기록들이 존재하는걸 인터넷에서 확인
      3. DB sqlite로 분석하니 (what kind of phones do vsco girls enjoy)문장 발견, VSCO girls 검색 결과 어떤 스타일을 나타내는 단어임
      -> VSCO 

  ### Q9. What is the name of the device?
      1. device 이름이라 SYSTEM 레지스트리에서 controlset001\control 내부의 device 이름의 파일들을 확인했더니 발견 못함
      2. 인터넷 검색 결과 computername이라는 폴더에 있는걸 확인
      -> DESKTOP-3A4NLVQ
        (추후 문제 풀다보니 security.evtx 파일에서도 4624 ID로 검색하니 찾을 수 있었음)

  ### Q10. What is the SID of the machine?
      * 아까 찾은 RID 앞 부분들이 SID임
      ->S-1-5-21-2446097003-76624807-2828106174
        
  ### Q11. How many web browsers are present?
      1. 브라우저들 경로를 찾아봄
        • 구글 크롬 (Google Chrome)
	          • 설치 경로: C:\Program Files\Google\Chrome\Application (또는 64비트/개인별 설치에 따라 C:\Users\사용자이름\AppData\Local\Google\Chrome\Application)
  	        • 실행 파일: chrome.exe
        • 마이크로소프트 엣지 (Microsoft Edge)
	          • 설치 경로: C:\Program Files (x86)\Microsoft\Edge\Application
        	  • 실행 파일: msedge.exe
        • 네이버 웨일 (Naver Whale)
          	• 설치 경로: C:\Program Files\Naver\Naver Whale\Application
          	• 실행 파일: whale.exe
        • 모질라 파이어폭스 (Mozilla Firefox)
	          • 설치 경로: C:\Program Files\Mozilla Firefox (또는 사용자 설치 시 C:\Users\사용자이름\AppData\Local\Mozilla Firefox)

      2. 각 존재하는 브라우저 종류 찾기
      -> 5개(edge, chrome, firefox, internet exploer, tor browser)
         • 참고로 tor browser은 생소한 브라우저라 못찾았는데 program1파일 내부에 있었음

  ### Q12. How many super secret CEO plans does Tim have? (Dr. Doofenshmirtz Type Beat)
      1. Tim 파일중에 secret.odt라는 수상한 워드 파일을 발견
      2. 내용중에 빈공간인데 드래그가 가능함 -> 색을 바꿔봄
       • Take over the world 
       • Destroy Google
       • Release the new Fruit Phone
       • Fire Jim Tomato
      -> 4개 발견
        
  ### Q13. Which employee does Tim plan to fire? (He's Dead, Tim. Enter the full name - two words - space separated)
      1. 12번 문제의 정답 중 하나임
      -> Jim Tomato

  ### Q14. What was the last used username? (I didn't start this conversation, but I'm ending it!)
      1. 처음에는 Security.evtx중 4624 ID(로그인 성공)으로 찾아봤는데 사용자 이름은 안나옴
      2. 인터넷에 다른 방법을 찾아보니 SAM 레지스트리에 시스템의 로컬 사용자 계정 정보와 보안 관련 데이터가 있는 것을 확인
      3. domains\account\user에 가보니 last login time이 있었고 PID 1006 즉 miriam.grapes -> 근데 틀림
      4. 다시 인터넷에 검색해보니 SOFTWARE 레지스트리 내부에 microsoft\windows nt\currentversion\winlogon 파일에 마지막 사용자 정보 있는걸 확인
      -> Jim Tomato

  ### Q15. What was the role of the employee Tim was flirting with?
      1. 사진이나 문서들에서는 플러팅한 흔적 발견 못함
	  2. 브라우저를 앞선 문제에서 언급한 적 있으니 브라우저 이용 기록 확인해보기
	  3. firefox기록 먼저 확인하니  So Your Secretary Is Cute, Now What? 라는 제목을 확인함
	  -> Secretary

  ### Q16. What is the SID of the user "suzy.strawberry"?
      1. 6번 문제에서 찾은 방법으로 확인 가능
	  -> 1004

  ### Q17. List the file path for the install location of the Tor Browser.
      1. 문제 11번에서 찾은 경로
	  -> Program1
        
  ### Q18. What was the URL for the Youtube video watched by Jim?
      1. URL을 확인하려면 브라우저 기록을 봐야함(chrome : history, firefox : palce.sqlite)
	  2. chrome만 있어서 확인해본 결과 https://www.youtube.com/watch?v=Y-CsIqTFEyY 발견
	  -> https://www.youtube.com/watch?v=Y-CsIqTFEyY

  ### Q19. Which user installed LibreCAD on the system?
       1. FTK imager로 유저들 폴더 분석해보니 miriam.graspes 내부에서 LibreCAD installer 발견
	   -> miriam.graspes
			• 혹시 몰라서 교차 검증할려고 Security.extv 에서 ID 4688(프로세스 생성)로 필터링해서 LibreCAD를 검색하니 안나왔음

  ### Q20. How many times "admin" logged into the system?
      1. 로그인 횟수이기에 Security.evtx에서 ID 4624(로그인 성공) 및 admin 조건으로 확인하니 20번이 넘는 개수가 나옴 -> 정답 아님
	  2. 계정 로그인 횟수와 관련된 정보를 인터넷에 검색해보니 SAM(스템의 로컬 사용자 계정 정보와 보안 관련 데이터가 있음) 파일에 domains\account\users 폴더에 유저별 로그인 횟수 나옴
	  -> 10
			• 이벤트 로그랑 SAM 파일 횟수가 다른 이유를 검색해보니 이벤트 로그는 다른 로그인 성공까지 더하기 때문에 많이 부풀려진다는 것을 인터넷에서 확인함
        

  ### Q21. What is the name of the DHCP domain the device was connected to?
      1. DHCP(dynamic host configuration protocol) domain이라 Web 데이터를 봐야한다고 생각함
	  2. 근데 분석 가능한게 브라우저만 있다고 생각함 -> 모든 유저들 브라우저 기록을 봐도 뭐가 DHCP domain인지는 모르겠음
	  3. DHCP는 SYSTEM쪽이니 레지스트리로 분석해보기 -> 근데 뭘 분석하면 되는지 몰라서 인터넷에 검색해봄
	  4. controlset001\serviecs\tcpip\paramaters\interfaces에서 확인 가능
	  -> fruitinc.xyz
			• serviecs와 control폴더의 분류 기준에 대해서 궁금하여 찾아본 결과
				특정 드라이버/서비스 하나가 쓰는 설정/상태 -> services
				시스템 전체가 공유하는 규칙/목록 -> control
				인식된 하드웨어 장치 목록 -> enum
				이고 IP관련인 Tcpip.sys는 서비스 측에 속해서 services에 있었음
				

  ### Q22. What time did Tim download his background image? (Oh Boy 3AM . Answer in MM/DD/YYYY HH:MM format (UTC).)
      1. Tim이 가지고 있는 사진은 hqdefault.jpg밖에 없음
	  2. FTK imager에서 파일에 관한 시간 확인
	  -> 04/05/2020 03:49
        

  ### Q23. How many times did Jim launch the Tor Browser?
      1. tor browser 자체의 places.splite를 분석했지만 안나옴
	  2. SOFTWARE가서 브라우저나 관련된 것들 찾아봤는데 뭘 분석할지 확신이 안섬
	  3. 개인 기록이라 NTUSER.DAT에서 확인해봄 -> 근데 뭘 봐야할지 모르겠어서 힌트봄 -> regripper로 Program1\firefox.exe(tor browser가 실행되는 곳) 횟수를 확인하라고 함
	  -> 2(아마 regripper은 결과가 정리되어서 나오니 이걸로 찾아보라고 한 것 같다.)
			• 교차 검증을 위해 register exploer로는 어떤 폴더를 분석해야하나 인터넷에 검색해보니 
			  NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\UserAssist\{GUID}\Count이고 
			  tor browser가 있는 program1의 firefox.exe 실행 횟수를 찾아보니 동일함 + userassist(실행한 프로그램에 관련 횟수 등.. 정보가 담겨있음)

  ### Q24. There is a png photo of an iPhone in Grapes's files. Find it and provide the SHA-1 hash.
      1. miriam의 모든 png파일을 보았지만 iPhone사진은 발견 못함 -> 어딘가에 숨겨져 있을 것임
	  2. 유일하게 FTK imager로 사진이 안보이는 samplePhone.jpg 파일이 수상하여 HxD로 조사할 것임
	  3. jpg 파일들인데 png파일을 찾으라고 한 것이 의심스럽기도 하며 HxD로 분석결과 헤더 시그니처가 FF D8 FF E0(jpg 헤더 시그니처)가 아닌 것을 발견
	  4. 89 50 4E 47(png 헤더 시그니처)를 검색하니 발견함(offset 174A) -> 49 45 4E 44(png 푸터 시그니처 일부분)검색하니 또 발견(offset 2900A)
	  5. png부분을 추출해서 Hxd로 새로운 png파일을 만들어 보니 iPhone 뒷면 사진이 나옴 -> hascalc로 SHA-1값 추출
      -> 537fe19a560ba3578d2f9095dc2f591489ff2cde
			• 추가로 원래 맨 위에 있던 시그니처를 검색해보 webP시그니처로 있어서 steghide 방식으로 숨긴 것 같음

  ### Q25. When was the last time a docx file was opened on the device? (An apple a day keeps the docx away. Answer in UTC, YYYY-MM-DD HH:MM:SS)
      1. 질문에서 뜬금없이 Apple이야기를 하는걸 보니 tim.apple 이놈이랑 뭔가 연관된 것 같음
	  2. tim.apple의 NTUSER.dat파일을 분석해 보기로함(처음에는 뭔지 모르겠으니 regripper로 분석하기)
	  3. last라는 단어로 검색하니 odt파일(secret.odt)에 대한 시간값이 나옴 -> 정답이 아님
	  4. 잘 모르겠어서 힌트를 확인하니 과정은 맞지만 갑자기 jim의 폴더를 열라고 함
		(AI로 분석해보니 미국 속담을 이용한 건데 doctor대신 docx를 사용해 docx를 숨기고 멀리하는 사람을 나타내는 말이라고 함....즉 제일 의심스러운놈...)
	  -> 2020-04-11 23:23:36

			
  ### Q26. How many entries does the MFT of the filesystem have?
      1. FTK imager에서 $MFT파일의 개수인가 해서 225,181,696을 해보니 정답이 아니였음
	  2. 혹시나 size일까봐 219,904KB로 정답을 넣어보니 맞음
	  -> 219,904
        

  ### Q27. Tim wanted to fire an employee because they were ......?(Be careful what you wish for)
      1. 지금까지 문제를 풀면서 대부분 문서나 이미지 파일에 있거나 브라우저 기록이였음
	  2. 브라우저(tim은 chrome과 firefox를 보유) 기록을 분석해보니 chrome의 histroy에서 how do i nicely fire my stinky employee 문장 발견
	  		• 냄새나는 사람을 싫어하는 것 같음
	  -> stinky

  ### Q28. What cloud service was a Startup item for the user admin?
      1. FTK imager로 user중 admin분석하다 보니 뭐 발견 못함
	  2. startup이라는걸 보니 자동실행인 것 같아 SOFTWATR 레지스트리에서 자동실행 분석
	  3. software\microsoft\windows\currentversion\run에 하나 있음
	  -> onedrive
	
        
  ### Q29. Which Firefox prefetch file has the most runtimes? (Flag format is )
      1. winprefetchview로 prefetch폴더에서 firefox를 기준으로 분석함	
	  2. 4월 16일이 마지막 실행인 FIREFOX.EXE-20153F0F.pf를 정답으로 제출했지만 정답이 아님
	  3. 문제를 다시 봤는데 정답 양식이 / 문자가 있어서 실행횟수를 적는건가 해서 많이 실행한 횟수로 21회인 것을 정답으로 제출함
	  -> FIREFOX.EXE-A606B53C.pf/21

  ### Q30. What was the last IP address the machine was connected to?
      1. IP연결이라 SOFTWATR인가 싶어서 분석하다가 마지막이라는 정보랑 여러곳을 방문한 기록들이 아닌 연결에 관한 것 같아서 SYSTEM 레지스트리에서 분석함
	  2. 비슷한 21번 문제처럼 services 내부에서 IP랑 관련된 파일들을 찾다보니 \ControlSet001\Services\Tcpip\Parameters\Interfaces\파일에서 여러 GUID중 값이 있는 것을 분석함
	  -> 192.168.2.242(DhcpIPAddress) 
			• 혹시 몰라서 추후 힌트를 보니 이 과정이 맞았음

  ### Q31. Which user had the most items pinned to their taskbar?
      1. FTK imgaer로 user들의 microsoft\internet Exploer\quick launch\user pinned\taskbar의 개수들을 비교한 결과 admin이 제일 많았음
	  -> admin
        
  ### Q32. What was the last run date of the executable with an MFT record number of 164885? (Format: MM/DD/YYYY HH:MM:SS (UTC).)
      1. MFT를 MFTECmd로 분석한 csv 파일에서 164885에 관해서 찾아보면 됨(MFTEcmd는 윈도우에 깔아서 윈도우shell로 진행함)
	  2. 보니 파일은 7zG.exe이고 prefetch에서 이 파일을 찾아 분석하면 2020-04-12 오전 11:32:09 나옴  -> 근데 정답이 아님
	  3. 다른 해설들을 찾아보니까 winprefetch는 신기하게 분석 결과의 시간들이 분석 PC의 시간대에 맞게 변환해서 줌
	  4. 한군은 UTC+9라서 -9시간 하면
	  -> 04/12/2020 02:32:09

  ### Q33. What is the log file sequence number for the file "fruit_Assortment.jpg"?
      1. 32번 문제에서 뽑아낸 MFT의 csv파일에서 fruit_Assortment.jpg 검색하면
	  -> 1276820064
        
  ### Q34. Jim has some dirt on the company stored in a docx file. Find it, the flag is the fourth secret, 
           in the format of <"The flag is a sentence you put in quotes">. (Secrets, secrets are no fun)
      1. jim에게 있는 docx 파일은 Document1.docx가 유일해서 추출후 봐보니 딱히 이상한게 없음
	  2. Hxd로 보니 앞쪽에 50 4B 03 04가 2번이나 등장함 -> 내가 알기론 레더 시그니처는 1개만 있어야 하기에 헤더시그니처가 같은 zip파일과 관련있나 해서 변환해봄	
	  3. 모든 파일들을 Hxd로 보다보니 file.xml만 50 4B 03 04로 xml답지 않게 시작함 -> 확장자를 docx로 바꿔봄
  	  4. 4개의 문장들이 나오고 4번째 문장이 정답임
	  -> Customer data is not stored securely 
			• 사실 2번 단계는 얻어 걸린거고 추후에 AI에 물어보니 docx자체가 원래 zip파일이라 확장자를 zip으로 바꾸면 docx를 구성하던 xml이나 다른 파일들이 나오게 되는거였음
		

  ### Q35. In the company Slack, what is threatened to be deactivated if the user gets their email deactivated?
      1. 일단 slack이 뭔지 찾아보니 기업용 메신저라는걸 확인함
	  2. slack이 hansel만 Roaming 폴더 안에 존재함 -> 이걸 분석해보기
	  3. 근데 slack이 처음이라 도저히 뭘 분석할지 몰라서 힌트를 봐도 데이터 파일을 보라고만 함 -> 다른사람 write up을 살펴봄
	  4. IndexDB\http_app.....\000003.log파일을 분석함 -> 내용이 많아 파일 추출후 strings 사용해 봐도 도저히 모르겠음(영어로 이메일이 작동한다고 적혀있는거 말고는 이해안감...)
	  5. 정답을 보니 여러 문장들중 무릎이랑 관련된 미국 언어유희(?)를 찾으면 되는거였고 답은 무릎이였음...
	  -> kneecaps

		• AI에게 물어보니까 찾은 경로(\Slack\IndexedDB\https_app.slack.com_0.indexeddb.leveldb)는 Slack에서 사용사의 사용흔적(입력 내용 등..)을 볼 수 있는 중요 루트였음
	  
		

### 소감
