# [endpoint] Hacked (medium)

* **Platform / Source**: cyberdefenders
* **Category**: endpoint(Linux)
* **Target File / URL**: https://cyberdefenders.org/blueteam-ctf-challenges/hacked/

---


  ### Q1. What is the system timezone?
      1. timezone은 설정값이니 Linux에서 전체 설정값들이 담긴 etc 폴더를 분석해야함
	  2. 찾아보니 timezone이라는 파일 발견
	  	-> Europe/Brussels

  ### Q2. Who was the last user to log in to the system?
      1. 마지막 사용자에 관한 기록을 찾아야 해서 기록이랑 관련된 var폴더에서 관련 자료 분석해야함
	  2. 하위 폴더인 log폴더에서 lastlog파일 발견 192.168.210.131	2019-Oct-05 11:23:34 UTC -> 이 유저가 마지막으로 로그인함
	  3. IP 주인을 알기 위해 ssh 접속 기록인 authlog 파일을 열어 찾아봄 
	  		• Oct  5 13:18:54 VulnOSv2 sshd[2825]: Accepted password for mail from 192.168.210.131 port 57704 ssh2
			• Oct  5 13:18:54 VulnOSv2 sshd[2825]: pam_unix(sshd:session): session opened for user mail by (uid=0)를 통해 ssh에 접속을 완료한 것을 확인
	  4. 근데 이전에 동일 IP가 다른 유저로 접속을 시도하려 한 것과 두 로그인 시간이 일치하지 않아서 이중 검증이 필요함. 더 찾아보니 wtmp가 모든 로그인, 아웃을 기록하는 것을 확인
	  5. wtmp파일을 추출 후 "last -f /mnt/d/71-Hacked/temp_extract_dir/wtmp"로 기록들을 전부 출력함
	  		• mail pts/1 192.168.210.131  Sat Oct  5 20:23 - 20:24  (00:00)로 mail이 제일 마지막에 로그인 한 것을 이중으로 확인함
		-> mail

  ### Q3. What was the source port the user 'mail' connected from?
      1. 2번에서 발견한 port를 그대로 이용해도 될 것 같지만 로그들을 다시 보니 "Oct  5 13:13:53 VulnOSv2 sshd[2624]: Accepted password for mail from 192.168.210.131 port 57686 ssh2dd 'mail' to group 'sudo'" 라는 이전에 찍힌 로그를 발견함. (이후로도 여러개를 발견함)
	  2. 그러면 여러개의 port들 중에서 뭐가 정답인지 
      3. 전부 입력해보니 마지막에 찍힌 port가 정답임(왜 마지막이 정답인가 생각해보니 제작자가 오해없게 마지막을 정답으로 정한 것 같음)
	  	-> 57708
  ### Q4. How long was the last session for user 'mail'? (Minutes only)
      1. wtmp에서 "mail pts/1 192.168.210.131  Sat Oct  5 20:23 - 20:24  (00:00)"를 통해 1분 정도 로그인 한 것을 확인할 수 있음
	  2. authlog에서도 마지막 로그인(port 57708) 시간과 session이 종료되는 시간차가 1분인 것을 통해 이중으로 검증 완료
	  	-> 1

  ### Q5. Which server service did the last user use to log in to the system?
      1. 여기서의 server service는 sshd, apache, MYSQL이라는 것을 검색을 통해 찾음
	  2. 이전 문제들에서 찾은 결과를 통해 마지막에는 어떤 서비스에 접속한 것인지 발견할 수 있었음
	  	-> sshd

  ### Q6. What type of authentication attack was performed against the target machine?
      1. 로그를 찾다보니 아래와 같은 의심스러운 인증(authentication)기록들을 발견
	  		• Oct  5 12:42:38 VulnOSv2 sshd[1842]: pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=192.168.210.131  user=root
			• Oct  5 12:42:38 VulnOSv2 sshd[1837]: pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=192.168.210.131  user=root
			• Oct  5 12:42:38 VulnOSv2 sshd[1839]: pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=192.168.210.131  user=root
			• Oct  5 12:42:38 VulnOSv2 sshd[1840]: pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=192.168.210.131  user=root
			• Oct  5 12:42:38 VulnOSv2 sshd[1841]: pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=192.168.210.131  user=root
			이는 동일한 IP에서 동시에 ssh를 5개 열어서 시도한 것([]의 숫자들이 다르니 다 다른 PID임)으로 보임. (주의할 것은 PID 하나가 하나의 공격이라고 단정 지으면 안됨)
	 2. 이러한 형식의 공격을 검색해보니 정답같아 보이는 것을 발견
	 	-> Brute-force(무차별 공격)

  ### Q7. How many IP addresses are listed in the '/var/log/lastlog' file?
      1. 해당 경로를 따라가면 아래와 같은 data가 나옴(lastlog파일은 계정 한개당 마지막 로그인 하나만 기록하는 파일)
	  		• 0	    tty1		            2016-May-04 17:36:39 UTC  (tty = teletypewrite로 서버 앞에서 키보드나 모니터로 직접 로그인)
			• 8	    pts/1	192.168.210.131	2019-Oct-05 11:23:34 UTC  (pts = pesudo-terminal slave로 네트워크를 통해 원격으로 로그인) 
			• 1000	pts/0	192.168.56.101	2016-May-04 17:35:16 UTC
			• 1001	tty1		            2016-May-04 08:41:07 UTC
	  	-> 2

  ### Q8. How many users have a login shell?
      1. login "shell"에 관련된 것이니 login과 연관된 etc/passwd파일을 분석하기로 결정
	  2. data들의 구조는 "사용자이름:password:UID:GID:설명:home directory:로그인 할 때 실행되는 프로그램" 이고 주요로 봐야할 것은 맨 마지막 "로그인 할 때 실행되는 프로그램"이다.
	  3. 검색해보니 etc/shells에 있는 프로그램들이 여기서 인정하는 login shell이라는 것을 확인. -> /bin/sh, /bin/dash, /bin/bash, /bin/rbash, /usr/bin/tmux, /usr/bin/screen 발견
	  4. 이에 맞는 개수를 세면 정답이 나옴
	  	-> 5
 
  ### Q9. What is the password of the mail user?
      1. password는 etc/passwd파일이 아닌 etc/shadow파일에 암호화해서 저장됨.
	  2. 그러나 이를 디코딩하는 방법을 검색해보니 여기서는 크래킹이라는 방법을 사용해야 하는 것을 발견.
	  		• 크래킹 : 후보들을 같은 알고리즘으로 해시해서 결과값이 같이 나오는 것을 찾는 방법
	  3. 이를 위해서는 john프로그램과 rockyou.txt라는 외부에서 만든 자주 사용하는 비밀번호 모음집이 필요하여 ubuntu에서 다운받아서 진행함
	  		• 또한 passwd와 shadow를 이 순서대로 파일을 합치고(unshadow 사용) txt파일로 만들어서 이용해야함 -> passwd구조에 shadow에 있는 해시된 암호들을 넣는 작업
	  4. john --wordlist=~/rockyou.txt /mnt/d/71-Hacked/temp_extract_dir/output.txt랑 john --show /mnt/d/71-Hacked/temp_extract_dir/output.txt를 이용해 결과값을 확인
	  	-> forensics

  ### Q10. Which user account was created by the attacker?
      1. 유저가 새로 생성된 것은 추후 로그인하려 사용할 목적으로 추측하여 authlog파일에서 분석을 시도함
	  2. 아래와 같은 log 발견
	  		• Oct  5 13:06:38 VulnOSv2 useradd[2525]: new group: name=php, GID=999
		    • Oct  5 13:06:38 VulnOSv2 useradd[2525]: new user: name=php, UID=999, GID=999, home=/usr/php, shell=/bin/bash
		-> php
        
  ### Q11. How many user groups exist on the machine?
      1. groups와 관련된 설정은 etc/group에 있음
	  2. 해당 파일에서 group개수 확인
	  	-> 58

  ### Q12. How many users have sudo access?
      1. sudo access접근이니 sudo그룹에 포함되어 있을 것으로 추측
	  2. etc/group 파일에 가서 sudo 그룹에 포함된 users 확인 -> php,mail
	  	-> 2
      
  ### Q13. What is the home directory of the PHP user?
      1. etc/passwd에 user의 home directory가 있음
	  	-> /usr/php

  ### Q14. What command did the attacker use to gain root privilege? (Answer contains two spaces).
      1. root privilege에 관한 command이니 authlog 에서 command를 키워드로 찾거나 인터넷에 추가로 검색하니 유저의 bash_history파일에도 command기록이 남는 것을 확인 
			• authlog : sudo를 붙여 실행한 명령어 및 인증(로그인, 권한 전환 등..) 이벤트에 대해서만 기록함(추후 기록 수정 불가)
			• bash_histroy : 유저가 남긴 command에 대해서 기록함(추후 기록 수정 가능)
	 2. 더 확실한 정보를 확인하기 위해서 authlog에서 분석을 시작함
	 		• Oct  5 13:21:11 VulnOSv2 sudo:     mail : TTY=pts/1 ; PWD=/var/mail ; USER=root ; COMMAND=/bin/su - 로그 발견 검색해보니 아래와 같은 결과 얻음
	 		• su : 지금 로그인 한 유저에서 다른 유저로 바꾸는 명령어 
			• - : 그 환경 및 파일들까지 다른 유저 환경으로 바꾸기
			• su - [사용자명] : 사용자명을 입력하면 그 유저로 바꾸고 미입력시 root로 적용됨
	 3. 추가적으로 root/bash_histroy에 가면 이렇게 전환 후 입력한 명령어들을 볼 수 있다고 하길래 가보니 정말 명령어들이 있었음
	
  ### Q15. Which file did the user 'root' delete?
      1. 14번 문제에서 확인한 root/bash_histroy 파일에서 지우는 명령어인 rm을 기준으로 찾으면 발견할 수 있었음
	  	-> 37292.c 
      
  ### Q16. Recover the deleted file, open it and extract the exploit author name.
      1. 도저히 방법을 몰라서 write up을 보니 파일을 직접 다운 받아서(?) 클릭 해보고 cve번호를 보고 이를 특정 사이트에서 조회를 하고 푸는 방법으로 추측되는 것을 확인
	  	    • 파일을 다운받기에는 껄끄러워서 write up으로 보기만 함... 
	  2. R-Studio 프로그램을 이용해 복구하는 방식도 확인함 
	  		• 근데 유로인 것 같아서 pass...
	  	-> rebel

  ### Q17. What is the content management system (CMS) installed on the machine?
      1. 일단 cms란 notion, blog같이 코딩을 하지 않아도 사진이나 글을 올리면서 사용자가 쉽게 수정할 수 있도록 도와주는 system이라는 것을 확인
	  2. 그에 맞는 cms 종류들을 검색하니 아래와 같이 나옴
	  		• 워드프레스(WordPress.org), 그누보드, 라이믹스(XE), 드루팔(Drupal), 줌라(Joomla)등이 있음
	  3. 보통 우분투에서 다운받을 때 sudo를 이용하니 authlog에서 install 명령어를 기준으로 찾다보니 아래와 같이 발견
	  	    • Apr 16 15:10:37 VulnOSv2 sudo: vulnosadmin : TTY=tty1 ; PWD=/home/vulnosadmin ; USER=root ; COMMAND=/usr/bin/apt-get install drupal7
		-> drupal

  ### Q18. What is the version of the CMS installed on the machine?
      1. 마저 drupal에 대해서 로그를 분석해보니 아래와 같은 결과를 발견함
	  		• Apr 16 15:16:44 VulnOSv2 sudo: vulnosadmin : TTY=pts/0 ; PWD=/usr/share ; USER=root ; COMMAND=/bin/cp -r drupal7 /var/www/html
			• Apr 16 15:17:04 VulnOSv2 sudo: vulnosadmin : TTY=pts/0 ; PWD=/var/www/html ; USER=root ; COMMAND=/bin/cp -r drupal7 jabc
	  2. cp 명령어는 copy하는 명령어니 var/www/html/jabc에 drupal이 존재하는 것을 확인
	  3. 인터넷에 drupal 7버전에 대해서 검색해보니 bootstrap.inc가 drupal 코어 버전이 상수로 저장되어 있는 파일로 확인
	  4. var/www/html/includes/booystrap.inc에서 내용 확인
	  	-> 7.26
	 

  ### Q19. Which port was listening to receive the attacker's reverse shell?
       1. port랑 reverse shell이면 열어두고 피해자가 공격자에게 들어가는 행위인데 이에 관한 기록을 저장한 파일을 몰라서 검색해봄
	   		• access.log : 웹이나 애플리케이션 서버와의 받은 요청 및 응답을 기록하는 파일
	   2. var/log/apache2/access.log에서 분석을 시도함
	   3. 아까 의심스러운 IP는 192.168.210.131이니 이를 기준으로 분석하다가 이상하게 긴 POST method log 발견
	   		• 28base64_decode라고 적혀 있어서 base64로 디코딩 시도
	   	-> 4444
			

		

### 소감
1. 리눅스의 구조에 대해서 잘 몰랐는데 이번 기회로 기본 틀은 이해한 것 같다.
2. 리눅스의 간단하고 중요하게 봐야하는 명령어에 대해서 좀 더 찾아봐야 할 것 같다.(무작정 암기보다는 문제를 직접 풀면서...)
3. 이렇게 새로운 문제를 푸는건 어렵지만 새롭게 내용을 배운다는 점에서 재미를 느낀다.
       

