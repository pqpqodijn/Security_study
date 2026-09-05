# [분야/카테고리] 문제 이름 (Level / 난이도)

> **Platform / Source:** pwn.college (또는 Dreamhack, CTF명)
> 
> 
> **Category:** System / Forensic / Reversing / Web
> 
> **Target File:** `파일명` (또는 Target URL)
> 

### 1. Target Overview & Environment (대상 환경 및 보호기법)

*분석할 대상의 기본 정보, 파일 구조, 또는 메모리 보호 기법(Checksec)을 확인합니다.*

- **File Info / Arch:** `64-bit ELF` / `PNG Image` / `x86 PE` / `Web API`
- **Protection / Constraints:** *(해당 시 작성)*
    - **RELRO:** Partial RELRO
    - **Stack Canary:** Disabled
    - **NX / PIE:** NX Enabled / No PIE
    - **Format Constraint:** `steghide` 지원 포맷 여부, 필터링 문자열 등

### 2. Analysis & Static/Dynamic Check (정적/동적 분석 및 원리)

*소스 코드, 어셈블리, 디컴파일(Ghidra/IDA), GDB, Hex Editor, 헤더 정보 등을 통해 취약점이나 은닉 데이터의 위치를 기술적으로 입증합니다.*

#### 2-1. Static & Structural Analysis (정적 분석 및 구조 확인)

C

```
// 또는 Decompiled Code / CLI 명령어 / HTTP Request Log
void challenge(){
    char buf[32];
    read(0, buf, 0x100); // [!] Vulnerability: buf(32바이트)보다 큰 256바이트 입력 허용
}
```

- **분석 내용:** 파일 구조(Header/Chunk), 디컴파일 코드, 또는 설정값 분석 결과를 기술합니다.
- **취약점/은닉 원리:** 데이터가 어디서 leakage/overflow 되거나 숨겨져 있는지 발생 원인을 명시합니다.

#### 2-2. Dynamic Analysis & Value Calculation (동적 분석 및 계산)

*GDB 메모리 추적, 파일 해시/비밀번호 디코딩, 네트워크 패킷 추적 등의 실행 과정 및 근거 수치를 적습니다.*

- **Memory / Offset Calculation:**
    - `buf` 위치: `RBP - 0x20` (32 bytes) + `SFP` (8 bytes) = **Target Offset: 40 bytes**
- **Data Decoding / Identification:**
    - `echo "<encoded_str>" | base64 --decode` 명령을 이용해 추출용 Passphrase 획득
    - `sha256sum` 및 `file` 명령어로 무결성 및 실제 Mime-type 검증

### 3. Solution & Exploit Scenario (핵심 풀이 로직 및 시나리오)

*문제를 해결하기 위한 전체 흐름을 Step-by-Step으로 정리하고, 실행 코드나 명령어를 작성합니다.*

#### 3-1. Step-by-Step Scenario

1. **[초기 검증]** 대상 파일/시스템의 상태를 확인하고 필요 데이터를 확보한다.
2. **[취약점 우회/추출]** 오프셋 계산값 기반 페이로드 전송 또는 Steganography/Decode 명령어 실행
3. **[최종 획득]** 실행 흐름 변경(RIP Hijacking) 또는 데이터 추출 후 Flag 확인

#### 3-2. Exploit / Execution Code (Command & Script)

Python

```
# System/Reversing의 경우 Pwntools/angr 스크립트 작성
from pwn import *

p = process('./babyof_level2.0')
payload = b'A' * 40 + p64(0x40123b) # Dummy Offset + Target Address
p.sendline(payload)
p.interactive()
```

Bash

```
# Forensic/Web/CLI의 경우 사용한 핵심 명령어
steghide extract -sf image.jpg -p "decoded_passphrase"
cat flag.txt
```

### 4. Retrospective & Security Patch (배운 점, 실수, 보안 대책)


- **실수했던 부분 및 Key Takeaway:**
    - *(예시 1)* x86-64 환경에서 Stack Alignment(16바이트 정렬) 미준수로 인한 Crash 문제를 확인하고 `ret` 가젯 추가 필요성을 배움.
    - *(예시 2)* `steghide`는 PNG 포맷을 지원하지 않고 JPEG/BMP만 지원한다는 파일 포맷 특성상의 한계를 명확히 이해함.
- **보안 대책 (Security Remediation):**
    - **[코드 개선]** `read(0, buf, sizeof(buf) - 1)`로 입력 크기를 제한하여 오버플로우 근본 차단.
    - **[컴파일/설정 적용]** `fstack-protector-all` (Canary) 및 PIE 활성화, 주요 비밀번호의 하드코딩 금지.
 






+
echo "source /opt/pwndbg/gdbinit.py" >> ~/.gdbinit
echo "set print symbol-filename on" >> ~/.gdbinit
echo "set print asm-demangle on" >> ~/.gdbinit\


추후 완성된 통합본
```
# [[카테고리]] 문제 이름 (Level / 난이도)

* **Platform / Source**: [Dreamhack / TryHackMe / HTB / CTF명]
* **Category**: [Forensic / DFIR / Reversing / System / Web / Misc]
* **Target File / URL**: [파일명 (e.g., memory.raw, sample.exe) / URL]

---

### 1. Target Overview & Environment (대상 환경 및 제약 조건)
분석 대상의 기본 정보, 파일 구조, 시스템 환경, 보안 제약 조건을 확인합니다.

* **File Info / Arch**: [e.g., Windows 10 x64 Memory Dump / 64-bit ELF / PNG Image]
* **Protections / Constraints**:
  * **System / Reversing**: RELRO (Partial/Full) | Canary (Enable/Disable) | NX | PIE
  * **Forensics / DFIR**: OS (Win10/Linux) | File System (NTFS/EXT4) | 도구 제약
  * **Web**: WAF 적용 여부 | SQLi/XSS 필터링 키워드

---

### 2. Analysis & Investigation (분석 및 원리 입증)
정적/동적 분석, 아티팩트 추적, 디컴파일 코드, 패킷 로그 등을 통해 기술적 근거를 입증합니다.

#### 2-1. Static & Structural Analysis (정적/구조 분석)
```text
# [예시 1 - Forensics/DFIR] 아티팩트 / CLI 명령어 / 로그
$ volatility -f memory.raw --profile=Win10x64_19041 pstree
-> PID 4412 (cmd.exe) 하위로 PID 5104 (powershell.exe) 이상 실행 정황 포착

# [예시 2 - System/Reversing/Web] 소스코드 / 디컴파일 / HTTP Request
void vuln_func() {
    char buf[64];
    read(0, buf, 0x200); // [!] Vulnerability: Buffer Overflow (64bytes < 512bytes)
}
```

- **분석 내용**: [구조 분석 결과, 레지스트리/이벤트 로그 경로, 디컴파일 로직 설명]
- **취약점 / 은닉 원리**: [데이터가 은닉된 위치(LSB, MFT, Alternate Data Stream) 또는 취약점 발생 원인 명시]

#### 2-2. Dynamic Analysis & Trace (동적 분석 및 계산)

Plaintext

```
# [예시 1 - Offset & Memory Trace]
buf 위치: RBP - 0x40 (64 bytes) + SFP (8 bytes) -> Target Offset: 72 bytes

# [예시 2 - Decryption & Packet Trace]
Wireshark Filter: http.request.method == "POST" && ip.addr == 192.168.1.10
추출 데이터: Base64 Encrypted String -> "S3JjcmV0S2V5MTIz" (Decoded: SecretKey123)
```

- **추적 및 계산 과정**: [오프셋 계산, 디코딩 방식, 패킷 흐름, 타임라인 재구성 근거 기재]

---

### 3. Solution & Scenario (핵심 풀이 로직 및 시나리오)

#### 3-1. Step-by-Step Scenario

1. **[초기 검증 및 단서 확보]**: [예: Volatility로 메모리 덤프 내 악성 프로세스 PID 확인 및 파일 추출]
2. **[데이터 추적 및 연계 분석]**: [예: 추출한 파일의 난독화 해제 및 C2 통신 패킷 내 복호화 키 확보]
3. **[최종 획득 및 행위 입증]**: [예: C2 통신 데이터를 복호화하여 깃발(Flag) 및 유출 데이터 확인]

#### 3-2. Exploit / Command Script (실행 코드 & 명령어)

Python

```
# [Python 스크립트예시 - Pwntools / Forensics Decrypter / Automate Script]
from pwn import *
import base64

# Exploit or Forensic Extraction Logic
p = remote('host.dreamhack.games', 12345)
payload = b"A" * 72 + p64(0x40123b)
p.sendline(payload)
p.interactive()
```

Bash

```
# [CLI 명령어 예시 - Volatility / Steghide / Wireshark / Forensics Tools]
volatility -f memory.raw --profile=Win10x64_19041 dumpfiles -Q 0x000000007fe1230 -D ./
steghide extract -sf hidden_image.jpg -p "SecretKey123"
```

---

### 4. Key Takeaways & Defense Measures (배운 점 및 보안 대책)

- **실수 및 배운 점 (Key Takeaway)**:
    - [기술적 레슨]: (예시) Volatility 프로필 불일치 시 KDBG 주소를 직접 지정해야 함을 배움.
    - [실수했던 부분]: (예시) LNK 파일 분석 시 UTC 타임스탬프와 Local Time 간의 시차(9시간) 계산 오류 수정.
- **보안 대책 및 방어 방안 (Defensive Remediation - BoB 포트폴리오 핵심)**:
    - **[코드/설정 개선]**: (예시) 입력 크기 제한 적용(`fgets` 사용), 하드코딩된 암호키 제거.
    - **[로그 및 모니터링 방안]**: (예시) Windows Event ID 4688(프로세스 생성) 모니터링 강화 및 PowerShell Script Block Logging(Event ID 4104) 활성화.
    - **[탐지 룰 작성]**: (예시) 추출된 악성코드의 YARA Rule / Sigma Rule 패턴 등록.
