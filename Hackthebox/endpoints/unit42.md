# [endpoint] unit43 (easy)

* **Platform / Source**: hackthebox
* **Category**: endpoint/Event log
* **Target File / URL**: https://app.hackthebox.com/sherlocks/Unit42

  ### Q1. How many Event logs are there with Event ID 11?
      1. Event viwer로 log파일을 열고 ID가 11인 것만 필터링함
        -> 56

  ### Q2. Whenever a process is created in memory, an event with Event ID 1 is recorded with details such as command line, hashes, process path, parent process path, etc. This information is very useful for an analyst because it allows us to see all programs executed on a system, which means we can spot any malicious processes being executed. What is the malicious process that infected the victim's system?
      1. 문제는 길지만 "언제 process가 실행되어서 감염시킨건가"를 확인해야함
      2. process가 실행 또는 생길 때 ID 1로 기록되니 이를 기준으로 필터링
      3. 여러개가 나오는데 다른건 경로가 기본적으로 실행되는 것으로 보이고 하나만 downloas폴더에서 실행됨
        -> C:\Users\CyberJunkie\Downloads\Preventivo24.02.14.exe.exe

  ### Q3. Which Cloud drive was used to distribute the malware?
      1. 2번에서 찾은 log에서 downloads폴더에 악성 프로세스가 있다는 점을 의식하고 그 이전 시간대를 분석함
      2. ID 11, 22, 26이 있었음. 이중에서 ID가 22면 뭔지 검색하니 쿼리 성공 여부에 관계없이 프로세스에서 발급한 DNS 쿼리를 기록하는 것을 확인
      3. ID가 22인 log를 분석하니 DNS가 나옴
        -> dropbox
		
  ### Q4. For many of the files it wrote to disk, the initial malicious file used a defense evasion technique called Time Stomping, where the file creation date is changed to make it appear older and blend in with other files. What was the timestamp changed to for the PDF file?
      1. time stomping에 대해서 검색하니 파일의 metadata를 인위적으로 수정하는 기술로 확인함
      2. 주제에서 준 pdf라는 힌트로 pdf파일 관련된 것을 확인하니 뭔가 내용이 다른 2개 log를 발견
      3. 하나만 technique_id=T1070.006,technique_name=Timestomp라는 문구 발견했고 log id가 2(파일의 생성시간이 변경되면 id 2로 기록됨)인점을 이용해 이 파이링 바뀐 것으로 추측
          (technique_id=T1070.006,technique_name=Timestomp는 MITRE ATT&CK 라벨로 공격자의 공격 기술을 고유번호로 분류한 것임)
      4. 나머지 하나는 id가 23인데 이는 session이 종료시 뜨는 것으로 검색하여 확인
        -> 2024-01-14 08:10:06(경로 밑에 바뀐 metadata랑 그 밑에는 원래 metadata가 있음)

  ### Q5. The malicious file dropped a few files on disk. Where was "once.cmd" created on disk? Please answer with the full path along with the filename.
      1. 처음에는 once.md가 created된 것을 보고 id를 1로 필터링 하여 분석해보니 once.md랑 관련된 것이 없었음
      2. 그래서 다른 ID 찾아보니 id가 11이면 파일이 생성될 때 기록되는 것을 배움
      3. id를 11로 필터링 하고 once.md를 검색하니 2개를 발견함 -> 이중 악성 프로세스가 있던 경로랑 비슷한 경로를 발견
        -> C:\Users\CyberJunkie\AppData\Roaming\Photo and Fax Vn\Photo and vn 1.1.2\install\F97891C\WindowsVolume\Games\once.cmd
	
  ### Q6. The malicious file attempted to reach a dummy domain, most likely to check the internet connection status. What domain name did it try to connect to?
      1. event log들을 살펴보다 유일하게 domain이 있는 걸 발견
        -> www.example.com
	  
  ### Q7. Which IP address did the malicious process try to reach out to?
      1. 네트워크 연결을 시도할 때 id가 뭔지 검색하니 3으로 나오는 것을 찾음
      2. 기록중에 실패한 걸 빼면 성공한 ip가 나옴
        -> 93.184.216.34(6번 문제에서 나온 정답 www.example.com의 ip랑 같음. 이곳을 접속한 것으로 이중 확인)
	  
  ### Q8. The malicious process terminated itself after infecting the PC with a backdoored variant of UltraVNC. When did the process terminate itself?
      1.  인터넷에 검색해 프로그램 종료시 id가 5로 기록되는 것을 확인 -> 필터링 하여 검색
        -> 2024-02-14 03:41:58


### 소감
1. 처음에 log id의 숫자가 다 기존에 알던 것보다 작아서 확인해보니 sysmon(System Monitor)이 로그를 남기는 주체라 다르다는 것을 배움
2. sysmon에 대해서 확인해 보니 내가 알던 windows 기본 로그랑 다르게 내가 사용한다고 설정해야 생기는 하나의 부가 event log프로그램이라는 것을 알게됨


        
       

