# 수행평가 순서 추첨

수행평가 시작 번호(또는 모둠 순서)를 효과음과 함께 추첨하고, 반별 평가 기록과 미응시 학생을 관리하는 교사용 웹앱입니다.

**바로 사용하기:** https://yemusict.github.io/suhaeng-draw/

## 주요 기능

- 학년도·학교명, 수업 들어가는 학년과 학급 수, 학년별 수행평가 주제 설정
- 번호순 평가 / 모둠별 평가 선택
- 결석생 번호와 사유(질병·인정·미인정) 표시 후 추첨 — 결석생은 순서에서 자동 제외
- 전학·위탁 등 없는 번호는 반별로 저장해 평가와 모둠 구성에서 계속 제외
- 드럼 소리, 전구 효과, 꽃가루가 있는 두근두근 추첨 화면
- 모둠마다 인원 조정, 랜덤 모둠 만들기
- 반별 평가일·시작 번호·결석생 기록과 반별 미응시 학생 목록
- 학생 명렬 등록(선택) 시 번호와 이름 함께 표시
- 백업 코드 복사·복원

## 데이터 저장

기본적으로 모든 기록은 사용하는 브라우저에만 저장되며 외부 서버로 전송되지 않습니다. 다른 기기로 옮길 때는 설정 탭의 백업 코드를 이용하거나, 아래 6자리 코드 연결을 켜세요.

## 여러 컴퓨터에서 같이 쓰기 (6자리 코드)

설정 탭의 ‘여러 컴퓨터에서 같이 쓰기’에서 6자리 숫자 코드를 정하면, 설정과 추첨 기록이 온라인(Firebase Firestore)에 저장됩니다. 다른 컴퓨터에서 같은 코드를 넣으면 같은 기록이 보입니다.

- 기록은 코드에서 만든 키로 브라우저에서 암호화(AES-GCM, PBKDF2)한 뒤 올라갑니다.
- 학생 명렬은 올라가지 않고 각 컴퓨터에만 남습니다.
- 기록 속 학생 이름은 가운데 글자를 가려서(김민지 → 김*지) 올라가고, 명렬을 등록한 컴퓨터에서는 원래 이름으로 보입니다.
- 6자리 숫자는 비밀번호보다 맞히기 쉬우니, 남이 알기 쉬운 숫자는 피하세요.

Firestore 보안 규칙:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /codes/{id} {
      allow get: if id.size() == 64;
      allow create, update: if id.size() == 64
        && request.resource.data.keys().hasOnly(['v','client','updatedAt','iv','ct'])
        && request.resource.data.ct is string
        && request.resource.data.ct.size() < 900000;
      allow list, delete: if false;
    }
  }
}
```

---

@yemusict
