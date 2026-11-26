# [endpoint] Ann’s Bad AIM (?)

* **Platform / Source**: forensicscontest
* **Category**: endpoint(network)
* **Target File / URL**: https://forensicscontest.com/2009/09/25/puzzle-1-anns-bad-aim

---


  ### Q1. What is the name of Ann’s IM buddy?
      1. 주어진 Ann의 IP는 192.168.1.158이니 이와 대화를 나눈 IP를 통계에서 찾아봄 -> 62.12.24.50 발견
      2. 이 둘이 연락한 내용만 나오게 필터링 후 확인해봄
      3. stream을 확인해보니 내용이 정리되지는 않고 추측되는 이름을 발견 -> Sec558user1
      4. 확실하지 않으니 추가 검증이 필요 -> 이야기하는 상대 port가 443이면 대부분 HTTPS = HTTP + TLS인 것을 검색해서 확인함. ssl은 tls이전 버전이니 문제 없음
      5. 그런데 문제에서 메신저 친구에 관해서 물어본 점이랑, 첫 패킷이 continuation data라는 점(처음부터 이어지는 data?)이 이상해 claude에게 물어봄
          • wireshark는 "Ethernet 디섹터 → IP 디섹터 → TCP 디섹터 → ??? 디섹터" 이런 계층별 디섹터에 데이터를 넘기며 해석하는데 TCP 443 port면 SSL/TSL로 해석한다는 점을 배움
          • SSL로 해석했는데 내용이 SSL이 아니라 continuation data로 해석함
          • DECODE as(디코딩 바꾸기)로 디섹터(해석 규칙)을 맞게 수정해서 다시 해석해봐야함
      6. 무슨 디섹터로 바꿀지는 도저히 모르겠어서 write up을 찾아보니 AIM을 사용함
      7. AIM messiging으로 대화 관련된 내용들이 나옴!(Buddy에서 이름 확인 가능)
        -> Sec558user1
      
  ### Q2. What was the first comment in the captured IM conversation?
      1. 이름을 찾은 처음 메세지칸에 내용도 들어있어서 확인할 수 있음(ValueMessage)
        ->  Here's the secret recipe... I just downloaded it from the file server. Just copy to a thumb drive and you're good to go &gt;:-) 

  ### Q3. What is the name of the file Ann transferred?
      1. 원래 보던 stream에서 62.12.24.50이 보낸 recipe.docx라는 파일명 발견
      2. recipe.docx를 기준으로 192.16.1.158이 보내는 바이트 내용중에 있는 다름 패킷을 검색해봄
      3. 192.168.1.159 IP address에 보내는 stream에서 또 발견 -> 여기에는 PK도 있어서 파일로 확정
        -> recipe.docx
      
  ### Q4. What is the magic number of the file you want to extract (first four bytes)?
      1. 3번에서 찾은 패킷에서 PK부터 4 bytes 추출(1칸이 1 byte)
        -> 50 4B 03 04

  ### Q5. What was the MD5sum of the file?
      1. stream에서 RAW형식으로 추출 후 hxd에서 열고 magic number이전 내용 지우기
      2. footer signature가 50 4B 05 06(docx)이니 이를 검색했는데 맨 밑줄이 나와서 더 이상 자를 것은 없음
      3. docx 파일로 저장하고 파일 MD5계산하기
        -> 8350582774e1d4dbe1d61d64c89e0ea1
  
  ### Q6. What is the secret recipe?
      1. docx파일 열면 확인할 수 있음
        -> 1 serving
           Ingredients:
           4 cups sugar
           2 cups water
           In a medium saucepan, bring the water to a boil. Add sugar. Stir gently over low heat until sugar is fully dissolved. Remove  the  saucepan from heat.  Allow to cool completely. Pour into gas tank. Repeat as necessary. <img width="851" height="297" alt="image" src="https://github.com/user-attachments/assets/947c0420-6b9a-4fae-a210-ccc2c6feaad7" />




		

### 소감
1. 사실 pcap관련된 문제들은 나만의 문제 해결 가이드를 만들지 못했는데 이번 기회에 만듬
     • 기간, 패킷 수, 링크 타입은?	Statistics → Capture File Properties
     • 어떤 프로토콜이 섞여 있나?	Statistics → Protocol Hierarchy
     • 누가 누구와, 얼마나 주고받나?	Statistics → Conversations (TCP/UDP 탭, Bytes 기준 정렬)
     • 포트와 내용이 맞는가? 내부↔내부 직접 통신이 있는가?	Conversations 결과를 보고 가설 세우기
                                                        (port 마다 정해진 사용법이 있는데 port와 내용이 다른지 확인)
                                                        (ex) 443port → TLS
     • 실제로 오간 데이터는?	우클릭 → Follow → TCP Stream
     • 파일이 오갔다면 꺼내서 해시 계산	Export Objects / Follow Stream → Raw 저장 → md5sum
2. wireshark가 pacp에 대해서 계층별 해석하는 방식을 이해했고 이 과정에서 오류로 해석될 수 있다는 점도 배움
3. port 마다 대부분 대응되는 protocol이 있다는 점을 배움

• 

