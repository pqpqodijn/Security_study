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
	  		* Oct  5 13:18:54 VulnOSv2 sshd[2825]: Accepted password for mail from 192.168.210.131 port 57704 ssh2
			* Oct  5 13:18:54 VulnOSv2 sshd[2825]: pam_unix(sshd:session): session opened for user mail by (uid=0)를 통해 ssh에 접속을 완료한 것을 확인
	  4. 근데 이전에 동일 IP가 다른 유저로 접속을 시도하려 한 것과 두 로그인 시간이 일치하지 않아서 이중 검증이 필요함. 더 찾아보니 wtmp가 모든 로그인, 아웃을 기록하는 것을 확인
	  5. wtmp파일을 추출 후 "last -f /mnt/d/71-Hacked/temp_extract_dir/wtmp"로 기록들을 전부 출력함
	  		* mail pts/1 192.168.210.131  Sat Oct  5 20:23 - 20:24  (00:00)로 mail이 제일 마지막에 로그인 한 것을 이중으로 확인함
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
	  		* Oct  5 12:42:38 VulnOSv2 sshd[1842]: pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=192.168.210.131  user=root
			* Oct  5 12:42:38 VulnOSv2 sshd[1837]: pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=192.168.210.131  user=root
			* Oct  5 12:42:38 VulnOSv2 sshd[1839]: pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=192.168.210.131  user=root
			* Oct  5 12:42:38 VulnOSv2 sshd[1840]: pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=192.168.210.131  user=root
			* Oct  5 12:42:38 VulnOSv2 sshd[1841]: pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=192.168.210.131  user=root
			이는 동일한 IP에서 동시에 ssh를 5개 열어서 시도한 것([]의 숫자들이 다르니 다 다른 PID임)으로 보임. (주의할 것은 PID 하나가 하나의 공격이라고 단정 지으면 안됨)
	 2. 이러한 형식의 공격을 검색해보니 정답같아 보이는 것을 발견
	 	-> Brute-force(무차별 공격)

  ### Q7. How many IP addresses are listed in the '/var/log/lastlog' file?
      1. 
	  
  ### Q8. How many users have a login shell?
      1. 

  ### Q9. What is the password of the mail user?
      1. 

  ### Q10. Which user account was created by the attacker?
      1.
        
  ### Q11. How many user groups exist on the machine?
      1. 

  ### Q12. How many users have sudo access?
      1. 
      
  ### Q13. What is the home directory of the PHP user?
      1. 

  ### Q14. What command did the attacker use to gain root privilege? (Answer contains two spaces).
      1. 

  ### Q15. Which file did the user 'root' delete?
      1.
      
  ### Q16. Recover the deleted file, open it and extract the exploit author name.
      1. 

  ### Q17. What is the content management system (CMS) installed on the machine?
      1. 
      
  ### Q18. The user Documents' directory contained a PDF file discussing data exfiltration techniques. What is the name of the file?
      1. 

  ### Q19. What is the version of the CMS installed on the machine?
       1. 

		

### 소감
1. Jumplist, shellbags와 같은 새로운 컴퓨터 내부 정보들도 새로 알게되는 방면에 outlook, skype, zenmap, bcwipe와 같은 처음 들어보는 어플에 관해 정보를 분석하는 방법을 배우게 되었다. 
2. 특히 25번 문제를 풀면서 프로그램이 실행한 파일에 대해서 분석하는 사고 과정은 처음이라 생소하고 신기했다.
3. 처음으로 autopsy를 이용하면서 문제를 푸니 생각보다 레지스트리를 분석하거나 시간이 많이 걸리는 부분들이 간단히 찾아지니 좋았고 이중 분석하기도 용이했다.
4. 다만 이렇게 문제를 풀면서 인터넷을 참고하며 푸는 비중이 아직 있고 단시간 내에 빨리 풀지 못한다는 점이 아직 많이 공부해야하는 것 같다.			         												 (참고로 2번 풀면서 write up은 2번째로 풀때 작성하는데도 잘 못푸는 문제도 몇개 있었다...)
5. 계속 문제만 풀다보니 무언가 아는 정보는 많아지지만 전문 지식이 채워지지 않는 것 같아서 디지털 포렌식 전문가 2급 자격증을 병행하며 공부해야겠다. (특히 복학하면 앞으로 배우는 내용과 겹처서 선행겸...)
       

