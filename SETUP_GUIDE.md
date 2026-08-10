# 🛠️ 선교대회 디자인팀 허브 - 설정 가이드

## 전체 순서 요약
1. Google Cloud 프로젝트 생성
2. API 활성화 (Sheets, Drive)
3. OAuth 동의 화면 설정
4. OAuth 클라이언트 ID 생성
5. API 키 생성
6. Google Sheets 준비
7. Google Drive 폴더 준비
8. index.html에 값 입력
9. 배포

---

## Step 1. Google Cloud 프로젝트 생성

1. https://console.cloud.google.com/ 접속
2. 상단 프로젝트 선택 드롭다운 클릭 → **"새 프로젝트"**
3. 프로젝트 이름: `선교대회-디자인팀` (아무거나 OK)
4. **만들기** 클릭
5. 생성 후 해당 프로젝트가 선택된 상태인지 확인

---

## Step 2. API 활성화

1. 좌측 메뉴 → **API 및 서비스** → **라이브러리**
2. 검색창에 아래 API를 각각 검색 후 **사용** 클릭:
   - `Google Sheets API` ← 검색 → 사용
   - `Google Drive API` ← 검색 → 사용
   - `Google Picker API` ← 검색 → 사용 (파일 선택용)

---

## Step 3. OAuth 동의 화면 설정

1. 좌측 메뉴 → **API 및 서비스** → **OAuth 동의 화면**
2. **User Type**: `외부` 선택 → 만들기
   - (Google Workspace 조직이 있으면 `내부`도 가능)
3. 앱 정보 입력:
   - 앱 이름: `선교대회 디자인팀 허브`
   - 사용자 지원 이메일: 본인 이메일
   - 개발자 연락처: 본인 이메일
4. **저장 후 계속**
5. 범위(Scopes) 단계:
   - **범위 추가 또는 삭제** 클릭
   - 검색: `spreadsheets` → `Google Sheets API .../auth/spreadsheets` 체크
   - 검색: `drive.file` → `Google Drive API .../auth/drive.file` 체크
   - **업데이트** 클릭 → **저장 후 계속**
6. 테스트 사용자 단계:
   - **+ ADD USERS** 클릭
   - 팀원 5명의 Gmail 주소 모두 추가:
     - (강민구, 김하늘, 정서현, 정서린, 유윤서의 Google 계정)
   - **저장 후 계속**
7. 요약 확인 → **대시보드로 돌아가기**

> ⚠️ 테스트 사용자에 추가하지 않은 계정은 로그인 불가!
> 나중에 팀원이 추가되면 여기서 이메일 추가하면 됩니다.

---

## Step 4. OAuth 클라이언트 ID 생성

1. 좌측 메뉴 → **API 및 서비스** → **사용자 인증 정보**
2. 상단 **+ 사용자 인증 정보 만들기** → **OAuth 클라이언트 ID**
3. 애플리케이션 유형: **웹 애플리케이션**
4. 이름: `디자인팀 허브 웹`
5. **승인된 JavaScript 원본** 에 추가 (배포할 주소):
   - `http://localhost` (로컬 테스트용)
   - `http://127.0.0.1:5500` (VS Code Live Server 사용 시)
   - `https://your-username.github.io` (GitHub Pages 사용 시)
   - 또는 배포할 도메인
6. **만들기** 클릭
7. 팝업에 나타나는 **클라이언트 ID** 복사 → 메모장에 저장

```
예시: 123456789-abcdefgh.apps.googleusercontent.com
```

---

## Step 5. API 키 생성

1. 같은 페이지 (사용자 인증 정보)에서
2. **+ 사용자 인증 정보 만들기** → **API 키**
3. 생성된 API 키 복사 → 메모장에 저장
4. (선택) **키 제한** 클릭:
   - 애플리케이션 제한: HTTP 리퍼러 → 배포 도메인 추가
   - API 제한: Google Sheets API, Google Drive API, Google Picker API만 선택

```
예시: AIzaSyB1234567890abcdefghijklmno
```

---

## Step 6. Google Sheets 준비

1. https://sheets.google.com 에서 **새 스프레드시트** 생성
2. 이름: `선교대회 디자인팀 DB`
3. 하단 시트 탭에서 **4개 시트**를 만들어야 합니다:

### 시트 1: `projects` (탭 이름을 이렇게 변경)
| A | B | C | D | E | F | G |
|---|---|---|---|---|---|---|
| id | name | status | assignee | deadline | notes | category |

### 시트 2: `meetings` (+ 버튼으로 새 시트 추가 후 이름 변경)
| A | B | C | D | E | F |
|---|---|---|---|---|---|
| id | date | title | content | author | createdAt |

### 시트 3: `budget`
| A | B | C | D | E | F | G |
|---|---|---|---|---|---|---|
| id | category | item | amount | date | note | createdBy |

### 시트 4: `files`
| A | B | C | D | E | F | G |
|---|---|---|---|---|---|---|
| id | name | driveId | url | uploadedBy | uploadedAt | category |

4. 각 시트의 **1행에 위 헤더를 입력**해주세요 (데이터는 2행부터 들어갑니다)
5. **스프레드시트 ID 복사**: URL에서 `/d/` 와 `/edit` 사이의 문자열

```
URL: https://docs.google.com/spreadsheets/d/1AbCdEfGhIjKlMnOpQrStUvWxYz/edit
                                             ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
                                             이 부분이 SHEET_ID
```

6. **공유** 클릭 → 팀원들의 Gmail에 편집 권한 부여

---

## Step 7. Google Drive 폴더 준비

1. https://drive.google.com 에서 새 폴더 생성
2. 폴더 이름: `선교대회 디자인팀 작업물`
3. 폴더 안에 들어가서 URL에서 **폴더 ID** 복사:

```
URL: https://drive.google.com/drive/folders/1AbCdEfGhIjKlMnOpQrStUvWxYz
                                             ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
                                             이 부분이 DRIVE_FOLDER_ID
```

4. 폴더 우클릭 → **공유** → 팀원들에게 편집 권한 부여

---

## Step 8. index.html에 값 입력

`index.html` 파일에서 아래 부분을 찾아 실제 값으로 변경:

```javascript
const CONFIG = {
  CLIENT_ID: '여기에_Step4에서_복사한_클라이언트ID.apps.googleusercontent.com',
  API_KEY: '여기에_Step5에서_복사한_API키',
  SHEET_ID: '여기에_Step6에서_복사한_스프레드시트ID',
  DRIVE_FOLDER_ID: '여기에_Step7에서_복사한_폴더ID',
  ...
};
```

---

## Step 9. 배포

### 옵션 A: GitHub Pages (무료, 추천)

1. GitHub 가입 (https://github.com)
2. 새 Repository 생성 (Private 권장 - 팀 내부용이니까)
3. `index.html` 파일 업로드
4. Settings → Pages → Source: `main` 브랜치 → Save
5. 몇 분 후 `https://your-username.github.io/repo-name` 에서 접속 가능
6. 이 URL을 Step 4의 승인된 JavaScript 원본에도 추가!

### 옵션 B: Netlify (드래그앤드롭)

1. https://netlify.com 가입
2. Sites → 파일 드래그앤드롭으로 배포
3. 생성된 URL을 Step 4의 승인된 JavaScript 원본에 추가

### 옵션 C: 로컬 테스트

VS Code에서 Live Server 확장 설치 후 index.html 우클릭 → "Open with Live Server"

---

## ✅ 체크리스트

- [ ] Google Cloud 프로젝트 생성됨
- [ ] Sheets API 활성화됨
- [ ] Drive API 활성화됨
- [ ] Picker API 활성화됨
- [ ] OAuth 동의 화면 설정 + 테스트 사용자 추가됨
- [ ] OAuth 클라이언트 ID 생성됨
- [ ] API 키 생성됨
- [ ] Google Sheets 4개 시트 + 헤더 입력됨
- [ ] Google Drive 공유 폴더 생성됨
- [ ] index.html CONFIG에 4개 값 입력됨
- [ ] 배포 완료
- [ ] 배포 URL이 OAuth 승인된 원본에 추가됨
- [ ] 팀원들 로그인 테스트 완료

---

## 🔧 트러블슈팅

### "이 앱은 확인되지 않았습니다" 경고
- 정상입니다! 테스트 모드이기 때문
- "고급" → "안전하지 않은 페이지로 이동" 클릭하면 됨
- 팀원들에게 미리 안내해두세요

### "redirect_uri_mismatch" 오류
- Step 4에서 승인된 JavaScript 원본에 현재 접속 URL이 정확히 포함되어있는지 확인
- `http`와 `https` 구분, 포트번호 확인, 끝에 `/` 없어야 함

### 로그인 후 데이터가 안 보임
- Google Sheets 시트 탭 이름이 정확히 `projects`, `meetings`, `budget`, `files`인지 확인
- 대소문자 구분됨!

### 파일 업로드 실패
- Drive 폴더 ID가 정확한지 확인
- 해당 폴더에 편집 권한이 있는지 확인
