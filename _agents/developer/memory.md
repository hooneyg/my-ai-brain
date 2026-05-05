# 💻 Developer (Lead Engineer) 개인 메모리

_Developer 에이전트만 읽고 쓰는 개인 노트. 학습·교훈·자주 쓰는 패턴이 누적됩니다._

## 학습 기록

- [2026-05-03] Tech Portfolio Blog Website의 핵심 페이지 템플릿(Tech Report/Wiki 포스팅 페이지)의 기본 코드 구조를 설계하세요. 특히, 콘텐츠가 쉽게 업데이트될 수 있도록 CMS 연동이 가능한 형태의 기본 HTML/CSS 구조와, 기사 본문 데이터를 관리할 API 엔드포인트 초안을 정의해 주세요. → 산출물 sessions/2026-05-03T06-44/developer.md
- [2026-05-03] 이 오류를 해결하기 위한 구체적이고 순서가 명확한 명령어 시퀀스(Code Snippet)를 제공해 주세요. (예: git fetch --all, git checkout main, git pull 등). 각 명령어가 어떤 역할을 하는지 주석을 달고, 문제 해결의 로직 흐름을 단계별로 제시해야 합니다. → 산출물 sessions/2026-05-03T06-56/developer.md
- [2026-05-03] Tech Wiki의 기술적 정확성을 극대화하기 위해, 제시된 명령어 시퀀스(git branch -m, git pull origin master --force 등)에 실제 터미널 환경에서의 '실패 케이스'와 '성공 케이스'를 보여주는 코드 블록 예제 및 검증 코드를 추가로 작성해주세요. (실행 가능하도록 구체적으로) → 산출물 sessions/2026-05-03T07-12/developer.md
- [2026-05-03] 발굴된 새로운 기술 아키텍처 비교 콘텐츠가 웹사이트에 효과적으로 노출될 수 있도록, CMS (Content Management System)의 '아키텍처 비교 비교 차트' 섹션을 최적화해주세요. 특히, 운영 흐름(Workflow)을 시각화할 수 있는 인터랙티브 플로우 차트 컴포넌트를 구현할 자동화 스크립트(혹은 API 연동 계획)를 작성해주세요. → 산출물 sessions/2026-05-03T07-26/developer.md
- [2026-05-05] 기술 블로그의 핵심 데이터 구조(API)를 설계합니다. 최소한 다음 3가지 엔드포인트와 데이터 모델을 정의해주세요: 1. `/api/articles` (아티클 목록 및 필터링), 2. `/api/articles/{slug}` (개별 아티클 본문), 3. `/api/tech-wikis/{slug}` (기술 비교 위키 페이지). 특히 아키텍처 비교 콘텐츠를 위해 '운영 책임', '비용', '장단점'을 구조적으로 비교할 수 있는 JSON 필드를 포함시켜 주세요. → 산출물 sessions/2026-05-05T08-42/developer.
- [2026-05-05] 기술 위키(Tech Wiki) 페이지의 핵심 API 스키마를 확정하고, 비교 분석 데이터를 위한 프론트엔드 스캐폴드(Scaffold)를 구축하세요. 특히, '비용 트레이드오프'와 '운영 책임' 필드가 자동으로 강조되는 구조를 반영해야 합니다. → 산출물 sessions/2026-05-05T08-52/developer.md
- [2026-05-05] 작성된 콘텐츠 초안(Writer)을 실제로 구현할 수 있도록, 블로그 포스트의 핵심 비교 분석 컴포넌트(예: A vs B 비교 테이블)에 필요한 프론트엔드 컴포넌트 구조와, 이 데이터를 불러올 백엔드 API 스키마(JSON 예시 포함)를 설계하세요. → 산출물 sessions/2026-05-05T09-17/developer.md
- [2026-05-05] Figma API Key의 연결 상태를 즉시 테스트하고, 성공 여부(Success/Failure), 인증 토큰의 유효 기간, 그리고 현재 호출 제한(Rate Limit)에 대한 상세 보고서를 작성하여 secretary에게 전달하세요. → 산출물 sessions/2026-05-05T09-42/developer.md