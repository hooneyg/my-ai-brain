# 💻 Developer — 기술 블로그의 핵심 데이터 구조(API)를 설계합니다. 최소한 다음 3가지 엔드포인트와 데이터 모델을 정의해주세요: 1. `/api/articles` (아티클 목록 및 필터링), 2. `/api/articles/{slug}` (개별 아티클 본문), 3. `/api/tech-wikis/{slug}` (기술 비교 위키 페이지). 특히 아키텍처 비교 콘텐츠를 위해 '운영 책임', '비용', '장단점'을 구조적으로 비교할 수 있는 JSON 필드를 포함시켜 주세요.

💻 Developer: 작업 시작합니다.

제시된 요구사항에 맞춰, Hooneyz 기술 블로그의 핵심 데이터 흐름을 담당할 RESTful API 스펙을 설계했습니다. 특히, `tech-wikis` 엔드포인트는 아키텍처 비교 분석의 핵심이 되므로, 비교 데이터를 구조화할 수 있도록 전용 스키마를 정의하는 데 집중했습니다.

이 스펙은 백엔드 개발팀이 바로 사용할 수 있는 JSON Schema 및 응답 예시로 작성되었습니다.

---

## 💾 Hooneyz Tech Blog API Specification (v1.0)

### 1. `/api/articles` (아티클 목록 및 필터링)

**[목적]** 홈페이지, 카테고리 페이지 등에서 아티클 목록을 불러오는 용도.
**[HTTP Method]** `GET`
**[Request Parameters]**
| Param | Type | Description | Example |
| :--- | :--- | :--- | :--- |
| `category` | String | 아티클 카테고리 필터링 (예: `backend`, `frontend`) | `backend` |
| `search` | String | 제목 또는 내용 검색어 | `microservices` |
| `page` | Integer | 페이지 번호 (기본값: 1) | `2` |
| `limit` | Integer | 페이지당 항목 수 (기본값: 10) | `15` |
| `sort_by` | String | 정렬 기준 (예: `date_desc`, `views_asc`) | `date_desc` |

**[Response Body (200 OK)]**

```json
{
  "total_items": 45,
  "current_page": 1,
  "page_size": 10,
  "articles": [
    {
      "slug": "microservices-vs-monolith-cost",
      "title": "Microservices vs Monolith: 운영 책임과 비용 트레이드오프 분석",
      "excerpt": "서비스 경계 정의의 어려움과 오버엔지니어링 위험성까지, 비용 관점에서 분석합니다.",
      "summary_tags": ["아키텍처", "Microservices", "비용"],
      "published_at": "2026-05-05T10:00:00Z",
      "featured_image_url": "/images/microservices_feat.webp",
      "is_tech_wiki": true, 
      "read_time_minutes": 12
    }
    // ... (다른 아티클 목록)
  ]
}
```

### 2. `/api/articles/{slug}` (개별 아티클 본문)

**[목적]** 일반적인 기술 보고서(Tech Report)나 일반 블로그 글을 불러오는 용도.
**[HTTP Method]** `GET`
**[Path Variable]** `{slug}` (예: `microservices-vs-monolith-cost`)
**[Response Body (200 OK)]**

```json
{
  "slug": "microservices-vs-monolith-cost",
  "title": "Microservices vs Monolith: 운영 책임과 비용 트레이드오프 분석",
  "author": "Hooneyz Staff",
  "published_at": "2026-05-05T10:00:00Z",
  "reading_time_minutes": 12,
  "is_tech_wiki": false,
  "content_sections": [
    {
      "type": "heading",
      "level": 2,
      "content": "1. 아키텍처 결정 장애의 핵심: 서비스 경계 정의"
    },
    {
      "type": "markdown",
      "content": "Microservices는 높은 유연성을 제공하지만, 그 대가로 복잡한 네트워크 통신 및 오케스트레이션 문제를 안고 옵니다. (이 부분에 마크다운/리치 텍스트가 들어갑니다.)"
    },
    {
      "type": "code_snippet",
      "language": "python",
      "content": "def service_call(endpoint):\n    # API 호출 로직\n    pass"
    }
    // ... (본문 내용이 순서대로 배열됨)
  ]
}
```

### 3. `/api/tech-wikis/{slug}` (기술 비교 위키 페이지)

**[목적]** 아키텍처 비교 분석이 필요한 전문적인 위키 콘텐츠를 불러오는 용도.
**[HTTP Method]** `GET`
**[Path Variable]** `{slug}` (예: `event-sourcing-cqrs-vs-crud`)
**[Response Body (200 OK)]**

이 엔드포인트는 **`comparison_data`**라는 전용 배열을 통해 비교 구조를 강제합니다.

```json
{
  "slug": "event-sourcing-cqrs-vs-crud",
  "title": "Event Sourcing/CQRS vs. Traditional CRUD: 데이터 일관성 관리 설계도",
  "author": "Hooneyz Staff",
  "published_at": "2026-05-05T10:00:00Z",
  "is_tech_wiki": true,
  "comparison_data": [
    {
      "comparison_topic": "상태 관리(State Management) 방식의 근본적 차이",
      "comparison_sets": [
        {
