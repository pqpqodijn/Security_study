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

*★ BoB 자소서/면접 시 기술적 깊이와 성찰 태도를 증명하는 가장 중요한 파트입니다.*

- **실수했던 부분 및 Key Takeaway:**
    - *(예시 1)* x86-64 환경에서 Stack Alignment(16바이트 정렬) 미준수로 인한 Crash 문제를 확인하고 `ret` 가젯 추가 필요성을 배움.
    - *(예시 2)* `steghide`는 PNG 포맷을 지원하지 않고 JPEG/BMP만 지원한다는 파일 포맷 특성상의 한계를 명확히 이해함.
- **보안 대책 (Security Remediation):**
    - **[코드 개선]** `read(0, buf, sizeof(buf) - 1)`로 입력 크기를 제한하여 오버플로우 근본 차단.
    - **[컴파일/설정 적용]** `fstack-protector-all` (Canary) 및 PIE 활성화, 주요 비밀번호의 하드코딩 금지.
