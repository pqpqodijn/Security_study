# [endpoint] BlackEnergy (medium)

* **Platform / Source**: cyberdefenders
* **Category**: endpoint
* **Target File / URL**: https://cyberdefenders.org/blueteam-ctf-challenges/blackenergy/

---

  ### Q1. 이 기계에 가장 적합한 volatility 프로필은 무엇인가?
      * OS에 관하여 묻는 것이라 windows.info를 사용했는데 원하는 정답은 얻지 못함
        -> 현재 내가 가지고 있는 volatility가 버전 3이라 imageinfo 명령어를 사용 불가능, 그래서 정답(WinXPSP2x86) 확인후 windows.info를 내용들로 최대한 구현해봄.
      * **OS는 윈도우로 확인** + **Is64Bit False -> 32비트 즉 x86** 
        단 SP(service pack)가 2인지 3인지는 확인못함
        
        
  ### Q2. 이미지를 획득할 당시 실행 중이던 프로세스는 몇 개인가?
      * windows.pslist사용해 확인하니 프로세스는 총 25개, 종료된 것이 6개라 정답은 19개로 확인

  ### Q3. cmd.exe 의 프로세스 ID는 무엇인가?
      * 이전 문제에 사용한 pslist에서 cmd의 PID는 1960으로 확인
        -> 갑자기 cmd.exe를 물어보니 해킹 관련해서 뭔가 연관된 것으로 추측됨.

  ### Q4. 가장 의심스러운 프로세스의 이름은 무엇인가?
      * 이전 문제에서 언급된 cmd.exe의 부모 PID를 찾아보니 rootkit.exe 프로세스를 확인
        -> 뭔가 이상해 프로세스를 인터넷에 검색해보니 일반 프로세스가 아닌 걸로 확인

  ### Q5. 어떤 프로세스가 코드 삽입 가능성이 가장 높은가?
      * 처음에는 pslist, psscan, pstree를 통해 rootkit.exe 와 관련되거나 또 이상한 프로세스가 있는건 아닌지 확인함.
        -> 유의미한 결과는 못찾음. 그래서 인터넷에 찾아보니 windows.malfind라는 plugin을 알게됨.
           windows.malfind : 프로세스들을 훑어보면서 프로세스 내부에 다른 코드들이 있는 것 같은 부분들을 추측해서 보여줌.(시작 주소, 권한, 발견한 코드에 대한 설명 등..)
      * PID 880인 svchost.exe에 읽고 쓰기 권한이며 유일하게 어떤 실행파일이 있는 것을 확인함.(MZ로 header가 시작됨)
        

  ### Q6. 최근 프로세스에서 이상한 파일이 참조되었습니다. 해당 파일의 전체 경로는 무엇인가?
      * 파일이 참조 되었길래 앞서 찾은 svchost.exe에 대하여 windows.handles로 찾으면 될 것 같다고 생각함.
        -> windows.handles로 만든 csv파일을 분석하다가 str.sys라는 이상한 파일을 찾아 검색해보니 실제 사용하는 파일이 아닌 것 같음.
           이에 대한 경로를 정답으로 확신함.

  ### Q7. 최근 실행된 프로세스에서 로드된 삽입된 DLL 파일의 이름은 무엇인가?
      * 처음에는 svchost.exe에다가 windows.dlllist 결과들을 분석했지만 찾지 못함. (악성코드가 프로세스의 DLL 목록들에서 지워버려서 못 찾은걸로 추측)
        -> 인터넷에서 관련 plugin을 찾다가 windows.ldrmodules(Loader Modules)을 찾음.
        * loader은 프로세스가 자기안에 가지고 있는 모듈(DLL)목록들(Inload, InInt, inMem)이며
          이들과 실제 메모리에 매핑된 모듈들(이 기록들은 수정할 수 없음)이랑 비교하여 진짜로 장부에 있는 사람인지 확인함.
      * 메모리에만 기록이 있고 svchost.exe에는 없는 msxml3r.dll 발견함.
        

  ### Q8. 주입된 DLL의 기본 주소는 무엇인가?
      * 문제 5번에서 svchost.exe 내부에 생긴 악성코드의 시작주소를 말하기에 이를 다시 확인하면 됨.
      

### 해킹된 과정(내 추측)

1. explorer.exe에서 rootkit.exe가 실행됨
2. rootkit.exe는 cmd.exe를 실행하였음
3. 이후 svchost.exe 내부에 악성코드가 생겼고 str.sys를 이용해 악성코드 실행에 도움을 준 것으로 추측
4. 또한 msxml3r.dll을 사용하였으나 이에 대한 기록은 지워버림
이렇게 해킹이 진행된 것 같다.
        
       

