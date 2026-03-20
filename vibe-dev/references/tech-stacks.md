# 프로젝트 유형별 추천 기술 스택

## 기술 스택 선택 원칙

1. **프로젝트 규모에 맞게**: 간단한 프로젝트에 복잡한 스택을 쓰지 않는다
2. **바이브코더 수준 고려**: 유지보수할 사람의 수준에 맞는 기술을 고른다
3. **생태계와 문서 충실도**: 커뮤니티가 크고 자료가 많은 기술을 우선한다
4. **비용 효율**: 초기에는 무료/저렴한 옵션을, 성장하면 확장 가능한 옵션을

---

## 간단한 랜딩페이지 / 포트폴리오

**복잡도: ⭐**
```
프론트엔드: HTML + CSS + 약간의 JS
    또는: Astro (정적 사이트)
스타일링:   Tailwind CSS
배포:       Vercel 또는 Netlify (무료)
```

**왜 이 조합인가:**
- React/Next.js는 과하다. 정적 페이지에 프레임워크가 필요 없음
- Tailwind은 디자인 시스템 없이도 일관된 스타일을 빠르게 만들 수 있음
- Vercel/Netlify는 깃 push만 하면 자동 배포

---

## 블로그 / 콘텐츠 사이트

**복잡도: ⭐⭐**
```
프레임워크: Next.js (App Router)
스타일링:   Tailwind CSS
CMS:        Contentful 또는 Notion API 또는 MDX
배포:       Vercel
```

**왜 이 조합인가:**
- Next.js의 SSG(Static Site Generation)로 빠른 로딩
- CMS 연동으로 비개발자도 콘텐츠 관리 가능
- Vercel + Next.js는 공식 조합이라 설정이 최소화

---

## 웹앱 (대시보드, SaaS, 관리 도구)

**복잡도: ⭐⭐⭐**
```
프론트엔드: React 18 + Vite (또는 Next.js)
스타일링:   Tailwind CSS + shadcn/ui
상태관리:   Zustand (간단) 또는 Tanstack Query (서버 상태)
백엔드:     Node.js + Express (또는 Hono)
DB:         PostgreSQL + Prisma ORM
인증:       JWT + bcrypt (또는 Auth.js)
배포:       Vercel (프론트) + Railway (백엔드) + Supabase (DB)
```

**왜 이 조합인가:**
- React + Vite는 개발 경험이 가장 빠름
- shadcn/ui는 커스터마이징 가능한 UI 컴포넌트 세트
- Prisma는 타입 안전한 DB 접근. 마이그레이션도 쉬움
- Zustand는 Redux보다 훨씬 간단한 상태관리

---

## 이커머스 / 쇼핑몰

**복잡도: ⭐⭐⭐⭐**
```
프론트엔드: Next.js (App Router + SSR)
스타일링:   Tailwind CSS + shadcn/ui
백엔드:     Node.js + Express
DB:         PostgreSQL + Prisma
결제:       토스페이먼츠 또는 아임포트 (한국) / Stripe (글로벌)
인증:       Auth.js (NextAuth)
이미지:     Cloudinary 또는 AWS S3
검색:       Algolia 또는 자체 구현
배포:       Vercel + Railway + Supabase
```

**왜 이 조합인가:**
- SSR로 SEO 최적화 (상품 페이지는 검색엔진 노출 필수)
- 토스페이먼츠는 한국 결제에서 가장 개발자 친화적
- Auth.js는 소셜 로그인 통합이 가장 쉬움

---

## 모바일 앱

**복잡도: ⭐⭐⭐**
```
프레임워크: React Native + Expo
스타일링:   NativeWind (Tailwind for RN)
상태관리:   Zustand
백엔드:     Node.js + Express (또는 Supabase BaaS)
DB:         PostgreSQL + Prisma
푸시알림:   Expo Notifications
배포:       EAS Build (Expo)
```

**왜 이 조합인가:**
- Expo가 React Native의 복잡한 네이티브 설정을 추상화
- 웹 개발자가 가장 빠르게 적응할 수 있는 모바일 개발 방법
- NativeWind으로 웹과 동일한 Tailwind 문법 사용

---

## 실시간 서비스 (채팅, 협업 도구)

**복잡도: ⭐⭐⭐⭐⭐**
```
프론트엔드: Next.js 또는 React + Vite
실시간:     Socket.io 또는 Supabase Realtime
백엔드:     Node.js + Express
DB:         PostgreSQL + Redis (캐시)
인증:       Auth.js
배포:       Railway (WebSocket 지원)
```

---

## 기술 스택 결정 질문

설계자가 기술 스택을 결정할 때 확인할 것:

1. 바이브코더가 나중에 유지보수할 수 있는 수준인가?
2. 무료 티어로 시작 가능한가?
3. 커뮤니티/문서가 충분한가? (에러 시 검색 가능)
4. 기획서의 모든 기능을 이 스택으로 구현 가능한가?
5. 향후 확장(사용자 증가, 기능 추가)에 대응 가능한가?
