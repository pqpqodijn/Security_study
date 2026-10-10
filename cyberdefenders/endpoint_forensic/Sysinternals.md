
# [endpoint] Sysinternals (medium)

* **Platform / Source**: cyberdefenders
* **Category**: endpoint
* **Target File / URL**: https://cyberdefenders.org/blueteam-ctf-challenges/sysinternals/

---

  ### Q1. 사용자가 다운로드한 악성 실행 파일의 이름은 무엇인가?
      * 일단 문제에서 **다운로드**라고 언급함 -> FTK imager로 분석 파일을 열고 public, 유저명, default 폴더 내부 download 파일들 비교
        * public에서 **sysinternals.exe**프로세스 발견 -> O
        
  ### Q2. 악성 실행 파일이 마지막으로 수정된 시점은 언제인가?
      * FTK imager에서 **sysinternals.exe**의 date Modified 날짜 발견 -> O

  ### Q3. 해당 악성코드의 SHA1 해시 값은 무엇인가?
      * 처음에는 해당 프로세스를 추출 후 **hashcalc**프로그램으로 구함 -> 그러나 정답이 아님
        * **virustotal**사이트에다가 푸풀한 파일을 올려 분석해서 얻은 값도 똑같음
      * 찾아보니 Amcache.hve(Application Management Cache)에서 파일 경로, sha1값 구할 수 있음을 확인
        * 파일 추출 후 **registry exploer**로 분석해 얻음
      -> Amcache는 최대 약 30MB크기만 이용해 sha1 해쉬 값을 만들기 때문에 전체 파일을 추출한 것과의 결과 값이 다른 것 같음(이 문제에서는 Amcache를 사용하길 원해서 이 값을 사용한 걸로 추측)

  ### Q4. 알리바바 판매업체 정보를 기준으로 볼 때, 해당 악성코드는 어떤 계열에 속하는가?
      * **virustotal**에 파일을 분석하고 이중에서  Security vendors' analysis 부분으로 확인

  ### Q5. 첫 번째 매핑된 도메인의 정규화된 도메인 이름(FQDN)은 무엇입니까?
      * **virustotal**에서 relation영역에 **Contacted URLs** 내용중 제일 먼저 연결된 URL확인
        * 방법을 찾다가 C:\root\system32\drivers\etc\hosts 파일에 ip 주소와 hostname이 연결되어있는 파일도 찾음
          **이를 통해 후에 악성프로그램이 이곳을 수정하면 사용자가 올바른 도메인을 이용해도 다른 ip address로 갈 수 있다는 것을 알게됨**

  ### Q6. 매핑된 도메인은 IP 주소와 연결되어 있습니다. 그 IP 주소는 무엇인가?
      * hosts파일에서 매핑된 ip주소 확인

  ### Q7. 1단계 실행 파일이 드롭하는 실행 파일의 이름은 무엇인가?
      * 프로세스가 새로 만들어 지는 것으로 오해함 -> **security.evtx** 의 4688이벤트(프로세스 생성) ID로 검색하나 못찾음
      * **virustotal**중 processtree에서 sysinternals.exe와 연괸되어서 **"C:\Windows\System32\cmd.exe" /C c:\Windows\vmtoolsIO.exe -install && net start VMwareIOHelperService && sc config VMwareIOHelperService start= auto** 발견
        * **vmtoolsIO.exe** 이 프로세스가 무언가 설치하는 것으로 드롭된 파일인 것으로 추측

  ### Q8. 2단계 실행 파일이 설치하는 서비스의 이름은 무엇인가?
      * 서비스 설치이다 보니 7번 문제에서 net start라는 서비스 실행 명령어와 **VMwareIOHelperService**확인함
      * 추가로 system.evtx에서 7045 ID(새 서비스 설치)로 검색해도 찾을 수 있음

        
       

