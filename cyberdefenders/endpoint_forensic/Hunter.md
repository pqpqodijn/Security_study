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
      1. 
	  
  ### Q8. When was the last login time for the discovered account? Format: one-space between date and time
      1. 

  ### Q9. There was a “Network Scanner” running on this computer, what was it? And when was the last time the suspect used it? Format: program.exe,YYYY-MM-DD HH:MM:SS UTC
      1. 

  ### Q10. When did the port scan end? (Example: Sat Jan 23 hh:mm:ss 2016)
      1. 
        
  ### Q11. How many ports were scanned?
      1. 

  ### Q12. What ports were found "open"?(comma-separated, ascending)
      1. 
        
  ### Q13. What was the version of the network scanner running on this computer?
      1. 

  ### Q14. The employee engaged in a Skype conversation with someone. What is the skype username of the other party?
      1. 

  ### Q15. What is the name of the application both parties agreed to use to exfiltrate data and provide remote access for the external attacker in their Skype conversation?
      1. 
	  
  ### Q16. What is the Gmail email address of the suspect employee?
      1. 

  ### Q17. It looks like the suspect user deleted an important diagram after his conversation with the external attacker. What is the file name of the deleted diagram?
      1. 
        
  ### Q18. The user Documents' directory contained a PDF file discussing data exfiltration techniques. What is the name of the file?
      1. 

  ### Q19. What was the name of the Disk Encryption application Installed on the victim system? (two words space separated)
       1. 

  ### Q20. What are the serial numbers of the two identified USB storage?
      1. 
        

  ### Q21. One of the installed applications is a file shredder. What is the name of the application? (two words space separated)
      1. 
				

  ### Q22. How many prefetch files were discovered on the system?
      1. 
        

  ### Q23. How many times was the file shredder application executed?
      1. 
	  
  ### Q24. Using prefetch, determine when was the last time ZENMAP.EXE-56B17C4C.pf was executed?
      1. 

  ### Q25. A JAR file for an offensive traffic manipulation tool was executed. What is the absolute path of the file?
      1. 

			
  ### Q26. The suspect employee tried to exfiltrate data by sending it as an email attachment. What is the name of the suspected attachment?
      1. 
        

  ### Q27. Shellbags shows that the employee created a folder to include all the data he will exfiltrate. What is the full path of that folder?
      1. 
	  
  ### Q28. The user deleted two JPG files from the system and moved them to $Recycle-Bin. What is the file name that has the resolution of 1920x1200?
      1. 
	
        
  ### Q29. Provide the name of the directory where information about jump lists items (created automatically by the system) is stored?
      1. 

  ### Q30. Using JUMP LIST analysis, provide the full path of the application with the AppID of "aa28770954eaeaaa" used to bypass network security monitoring controls.
      1. 
		

### 소감

       

