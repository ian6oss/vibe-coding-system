# 자주 발생하는 에러 패턴과 해결법

바이브코딩에서 반복적으로 나타나는 에러들의 패턴과 해결 가이드.
코드 검수관과 개발자가 에러 발생 시 참조한다.

---

## React / 프론트엔드

### 1. "Cannot read properties of undefined"
**원인:** 비동기 데이터가 로딩되기 전에 접근
```jsx
// ❌ 에러
{products.map(p => <ProductCard key={p.id} />)}

// ✅ 수정
{products?.map(p => <ProductCard key={p.id} />) || <Loading />}
```
**예방:** API 데이터를 쓰는 컴포넌트는 항상 로딩/에러 상태를 먼저 처리

### 2. 무한 리렌더링
**원인:** useEffect 의존성 배열에 매번 새로 생성되는 객체/함수
```jsx
// ❌ 무한루프
useEffect(() => { fetchData(filter) }, [filter])
// filter가 매 렌더마다 새 객체면 무한 호출

// ✅ 수정
const filterStr = JSON.stringify(filter)
useEffect(() => { fetchData(filter) }, [filterStr])
```
**예방:** 의존성에 객체를 넣을 때는 useMemo 또는 문자열 변환

### 3. CORS 에러
**원인:** 프론트(localhost:3000)에서 백엔드(localhost:8000)로 직접 요청
```
// ✅ 백엔드에 CORS 미들웨어 추가
app.use(cors({ origin: 'http://localhost:3000' }))
```
**예방:** 프로젝트 초기화 시 CORS 설정을 기본으로 포함

### 4. 상태 업데이트 후 이전 값 참조
**원인:** setState는 비동기. 바로 다음 줄에서 업데이트된 값이 아님
```jsx
// ❌ count는 아직 이전 값
setCount(count + 1)
console.log(count) // 이전 값 출력

// ✅ 콜백 형태 사용
setCount(prev => prev + 1)
```

---

## Node.js / 백엔드

### 5. "EADDRINUSE: port already in use"
**원인:** 이전 서버가 종료되지 않은 채 같은 포트로 재시작
```bash
# 해당 포트 사용 프로세스 찾기
lsof -i :3000
# 프로세스 종료
kill -9 <PID>
```
**예방:** 서버 시작 시 이미 사용 중인 포트 체크 로직 추가

### 6. "Cannot find module" (import 에러)
**원인:**
- 패키지 설치 안 됨 → `npm install <패키지명>`
- 상대 경로 오류 → 파일 위치 확인
- 확장자 누락 → `.js` 또는 `.jsx` 추가

### 7. 비동기 에러 미처리
**원인:** async/await에서 try-catch 누락
```javascript
// ❌ 에러 시 서버 크래시
app.get('/api/users', async (req, res) => {
  const users = await db.user.findMany()
  res.json(users)
})

// ✅ 에러 핸들링
app.get('/api/users', async (req, res) => {
  try {
    const users = await db.user.findMany()
    res.json(users)
  } catch (error) {
    res.status(500).json({ error: '서버 오류가 발생했습니다' })
  }
})
```
**예방:** 모든 API 라우트에 try-catch 적용 또는 에러 미들웨어 사용

---

## 데이터베이스

### 8. N+1 쿼리 문제
**원인:** 목록 조회 후 각 항목마다 추가 쿼리 실행
```javascript
// ❌ N+1: 게시물 100개면 101번 쿼리
const posts = await db.post.findMany()
for (const post of posts) {
  post.author = await db.user.findUnique({ where: { id: post.authorId }})
}

// ✅ include로 한 번에 조회
const posts = await db.post.findMany({
  include: { author: true }
})
```

### 9. 마이그레이션 충돌
**원인:** DB 스키마 변경 후 마이그레이션 미실행
```bash
npx prisma migrate dev --name "add_user_role"
```
**예방:** 스키마 변경 시 항상 마이그레이션 생성 및 적용

---

## 인증 / 보안

### 10. JWT 토큰 만료 미처리
**원인:** 토큰 만료 시 사용자에게 적절한 안내 없음
```javascript
// 프론트엔드: API 호출 시 401 체크
if (response.status === 401) {
  // 토큰 갱신 시도 또는 로그인 페이지로 리다이렉트
  refreshToken() || redirectToLogin()
}
```

### 11. 비밀번호 평문 저장
**절대 하면 안 되는 것.** 항상 해싱:
```javascript
import bcrypt from 'bcrypt'
const hashed = await bcrypt.hash(password, 12)
```

---

## CSS / 스타일링

### 12. 모바일 레이아웃 깨짐
**원인:** 고정 width 사용, viewport 미설정
```html
<!-- 필수 -->
<meta name="viewport" content="width=device-width, initial-scale=1">
```
```css
/* ❌ 고정 폭 */
.container { width: 1200px; }

/* ✅ 반응형 */
.container { max-width: 1200px; width: 100%; padding: 0 1rem; }
```

### 13. z-index 지옥
**예방:** z-index 체계를 미리 정의
```css
:root {
  --z-dropdown: 100;
  --z-modal: 200;
  --z-toast: 300;
  --z-tooltip: 400;
}
```

---

## 에러 대응 원칙

1. **에러 메시지를 정확히 읽는다** — 대부분의 에러는 메시지에 답이 있다
2. **최소 범위로 수정한다** — 에러 하나 고치려고 다른 곳을 건드리지 않는다
3. **수정 전에 원인을 확인한다** — "왜" 에러가 났는지 모르면 같은 에러가 반복된다
4. **수정 후 반드시 테스트한다** — 에러가 사라졌는지 + 새 에러가 생기지 않았는지
5. **같은 패턴의 에러는 일괄 수정한다** — 한 곳에서 발견되면 다른 곳도 확인
