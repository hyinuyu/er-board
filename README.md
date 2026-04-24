# 응급의학과 담벼락 🏥

응급의학과 전문의들이 과별 협진 정보, 프로토콜, 공지사항을 실시간으로 공유하는 게시판입니다.  
Firebase Firestore를 사용해 **실시간 저장 및 동기화**가 됩니다.

---

## 🚀 배포 방법 (처음 한 번만)

### 1단계 — GitHub Pages 켜기

1. 이 저장소(repo) 페이지에서 **Settings** 탭 클릭
2. 왼쪽 메뉴에서 **Pages** 클릭
3. Source를 **Deploy from a branch** 선택
4. Branch를 **main**, 폴더를 **/ (root)** 로 설정 후 **Save**
5. 1~2분 후 `https://[내아이디].github.io/[저장소이름]/` 으로 접속 가능

---

### 2단계 — Firebase 프로젝트 만들기

1. [Firebase Console](https://console.firebase.google.com/) 접속 (구글 계정 필요)
2. **프로젝트 만들기** 클릭 → 이름 입력 (예: `er-board`) → 계속
3. Google Analytics는 **사용 안 함** 선택 → 프로젝트 만들기

---

### 3단계 — Firestore 데이터베이스 만들기

1. 왼쪽 메뉴 **Firestore Database** 클릭
2. **데이터베이스 만들기** 클릭
3. **테스트 모드**로 시작 (30일간 누구나 읽기/쓰기 가능)
4. 위치: `asia-northeast3 (서울)` 선택 → 완료

> ⚠️ 30일 후에는 보안 규칙을 수정해야 합니다. 아래 **보안 규칙** 섹션 참고.

---

### 4단계 — 웹 앱 등록 및 설정값 복사

1. Firebase Console 홈에서 **`</>`** (웹) 아이콘 클릭
2. 앱 닉네임 입력 (예: `er-board-web`) → **앱 등록**
3. 아래처럼 생긴 설정값이 나타납니다:

```js
const firebaseConfig = {
  apiKey: "AIzaSy...",
  authDomain: "er-board.firebaseapp.com",
  projectId: "er-board",
  appId: "1:123456789:web:abcdef..."
};
```

4. 이 값들을 메모해 두세요 (다음 단계에서 사용)

---

### 5단계 — 앱에 Firebase 연결하기

1. GitHub Pages 주소로 접속
2. 처음 접속하면 **Firebase 연결 설정** 화면이 나타납니다
3. 4단계에서 복사한 값들을 각 칸에 입력
4. **저장하고 시작하기** 클릭
5. 끝! 이제 실시간으로 게시물이 저장됩니다 🎉

> 설정값은 내 브라우저에만 저장됩니다. 다른 선생님들도 각자 브라우저에서 같은 설정값을 입력하면 됩니다.

---

## 🔒 보안 규칙 (30일 후)

Firebase Console → Firestore → **규칙** 탭에서 아래 내용으로 교체하세요.  
(병원 내부망에서만 쓴다면 테스트 모드 연장도 괜찮습니다)

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if true;
    }
  }
}
```

---

## 📋 기능

| 기능 | 설명 |
|------|------|
| **과별 탭** | 정형외과, 신경외과, 내과 등 탭 전환 |
| **과 추가** | `+ 과 추가` 버튼으로 새 탭 추가 (Firestore에 저장) |
| **게시물 유형** | 정보공유 / 프로토콜 / 공지 / 일반 |
| **공지 자동 고정** | 유형을 `공지`로 선택하면 자동으로 상단 고정 |
| **실시간 동기화** | 누군가 글을 올리면 모든 접속자 화면에 즉시 반영 |
| **이름/닉네임** | 게시 시 이름 입력 (입력 안 하면 `익명`) |

---

## 🛠 문제 해결

**"연결 오류" 메시지가 뜰 때**
- Firebase Console → Firestore → 규칙 탭에서 읽기/쓰기가 허용되어 있는지 확인
- 설정값(apiKey, projectId 등)이 정확한지 확인

**설정값 다시 입력하고 싶을 때**
- 브라우저 개발자 도구(F12) → Application → localStorage → `er_board_firebase_config` 삭제 후 새로고침
