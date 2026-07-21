# 8low8lowme Wargame

[이스트캠프] 가디언즈 정보보호 및 인프라 관리 10기 3차 팀 프로젝트
**팀 자체 제작 웹 해킹 워게임 플랫폼 · 침투 테스트 워크스루 모음**

**8low8lowme**(= "Follow Follow me") 팀이 직접 설계·구현한 10개의 웹 해킹 문제(problem01~10)에 대한 침투 테스트 워크스루 보고서 저장소입니다. 각 문제는 정찰부터 취약점 재현, 근본 원인 분석까지 정리한 PDF 보고서로 문서화되어 있습니다.

## 저장소 구성

| 문제 | 워크스루 |
|---|---|
| Problem 01 — Stopwatch | [`Problem01_Stopwatch_워크스루.pdf`](<./Problem01_Stopwatch_워크스루.pdf>) |
| Problem 02 — TimeLeak | [`Problem02_TimeLeak_워크스루.pdf`](<./Problem02_TimeLeak_워크스루.pdf>) |
| Problem 03 — JWT Forgery | [`Problem03_JWT_Forgery_워크스루.pdf`](<./Problem03_JWT_Forgery_워크스루.pdf>) |
| Problem 04 — Lost Schema | [`Problem04_Lost_Schema_워크스루.pdf`](<./Problem04_Lost_Schema_워크스루.pdf>) |
| Problem 05 — Android Pattern Lock | [`Problem05_AndroidPatternLock_워크스루.pdf`](<./Problem05_AndroidPatternLock_워크스루.pdf>) |
| Problem 06 — Guide NPC | [`Problem06_GuideNPC_워크스루.pdf`](<./Problem06_GuideNPC_워크스루.pdf>) |
| Problem 07 — ESTsoft ALZ 파일 검증기(File Upload) | [`Problem07_File_Upload_워크스루.pdf`](<./Problem07_File_Upload_워크스루.pdf>) |
| Problem 08 — Hidden Keys | [`problem08_HiddenKeys_워크스루.pdf`](<./problem08_HiddenKeys_워크스루.pdf>) |
| Problem 09 — Reflected XSS | [`Problem09_ReflectedXSS_워크스루.pdf`](<./Problem09_ReflectedXSS_워크스루.pdf>) |
| Problem 10 — Blind Administrator | [`Problem10_Blind_Administrator_워크스루.pdf`](<./Problem10_Blind_Administrator_워크스루.pdf>) |

## 문제 목록 요약

| 문제 | 카테고리 | 위험도 | CWE | 핵심 취약점 |
|---|---|---|---|---|
| Stopwatch | Client-Side Logic | Low | CWE-602/807 | 정답 판정 로직이 클라이언트 JS에 전역 노출 → 서버가 결과값만 신뢰 |
| TimeLeak | Auth Bypass | High | CWE-330/321 | 서버 시각 노출 + 짧은 salt(4자리) 브루트포스로 관리자 토큰 위조 |
| JWT Forgery | Backup Disclosure | High | CWE-347/798 | robots.txt→백업 zip→JWT_SECRET 평문 노출 → payload role 변조·재서명 |
| Lost Schema | Blind SQLi Chain | High | CWE-89/943/287 | 공백 필터 우회 + Stacked Query로 OTP 테이블 조작, 2단계 인증까지 우회 |
| Android Pattern Lock | Brute Force | Medium | CWE-307/319 | 평문 패턴 파라미터 전송 + 미흡한 잠금 정책 → 전수 조사로 크랙 |
| Guide NPC | Cookie Tampering | Low | CWE-565/284 | 서명 없는 base64 role 쿠키 변조만으로 권한 상승 |
| File Upload | Upload Bypass | Medium | CWE-434/646 | 확장자·매직넘버(첫 4바이트)만 검사 → 위조 파일로 이중 검증 우회 |
| Hidden Keys | Steganography | Medium | CWE-200/522 | 이미지 LSB 스테가노그래피로 계정 ID와 2FA 시크릿 키 추출 |
| Reflected XSS | XSS to Auth Bypass | Medium | CWE-79/602 | 반사형 XSS로 클라이언트 측 토큰 검증 함수(findFlag) 직접 호출 |
| Blind Administrator | Blind SQLi | High | CWE-89/284 | Boolean 기반 Blind SQLi로 관리자 비밀번호 전체 복원 |

## 설계 원칙

각 워크스루는 다음 구조를 공통으로 따릅니다.

1. **개요(Executive Summary)** — 취약점 한 줄 요약
2. **공격 시나리오(Attack Narrative)** — 단계별 기법과 확보 정보를 표로 정리
3. **상세 재현 절차(Proof of Concept)** — 실제 페이로드, 스크립트, 스크린샷을 포함한 재현 과정
4. **근본 원인 분석(Root Cause)** — 취약점이 발생한 설계상의 근본 원인

주로 다루는 근본 원인 패턴은 다음과 같습니다.

- **클라이언트 신뢰**: 판정/검증 로직을 서버가 아닌 브라우저 JS에 위임(Stopwatch, Guide NPC, Reflected XSS)
- **비밀정보 노출**: 백업 파일, 이미지 메타데이터, 응답 헤더를 통한 시크릿/설정 유출(JWT Forgery, TimeLeak, Hidden Keys)
- **입력 검증 미흡**: 블랙리스트 필터·부분 파일 검사로 인한 우회(Lost Schema, File Upload)
- **인증/인가 설계 결함**: 서명 없는 값 신뢰, Blind SQLi를 통한 자격증명 복원(Guide NPC, Blind Administrator, Lost Schema)

## 사용 도구

Burp Suite(Proxy/Repeater/Intruder), gobuster, nmap, Python(requests, hashlib, PyJWT, Pillow), exiftool, CyberChef, Google Authenticator, 브라우저 개발자 도구(DevTools)

## 팀 구성

8low8lowme(= "Follow Follow me") — 정슬기, 김혜미, 김유선, 이찬근 외 구현팀
