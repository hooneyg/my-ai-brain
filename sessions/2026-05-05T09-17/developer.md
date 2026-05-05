# 💻 Developer — 작성된 콘텐츠 초안(Writer)을 실제로 구현할 수 있도록, 블로그 포스트의 핵심 비교 분석 컴포넌트(예: A vs B 비교 테이블)에 필요한 프론트엔드 컴포넌트 구조와, 이 데이터를 불러올 백엔드 API 스키마(JSON 예시 포함)를 설계하세요.

💻 Developer: 작업 시작합니다.

CEO의 지시에 따라, Writer가 작성한 콘텐츠 초안을 실제 블로그 포스트에 구현할 수 있도록, 핵심 비교 분석 컴포넌트의 **프론트엔드 구조(React/JSX 기반)**와 필요한 **백엔드 API 스키마(JSON)**를 설계했습니다.

이 구조는 콘텐츠의 깊이(운영 책임, 비용 트레이드오프)를 시각적으로 강조하는 데 초점을 맞추었습니다.

---

### ⚙️ 1. 백엔드 API 스키마 설계 (JSON)

**API 엔드포인트:** `/api/tech-wikis/graphql-vs-rest`
**목적:** 기술 비교 위키 페이지의 모든 구조적 데이터를 한 번에 제공합니다.
**스키마 버전:** v1.1 (Comparison Data Structure 추가)

```json
{
  "articleId": "graphql-vs-rest-optimization",
  "title": "GraphQL vs REST: 데이터 요청 최적화, 어떤 아키텍처를 선택해야 할까요?",
  "slug": "graphql-vs-rest",
  "metadata": {
    "primaryTopic": "API Design",
    "targetAudience": "30-40대 백엔드/풀스택 개발자",
    "keywords": ["GraphQL", "REST", "Over-fetching", "Under-fetching", "API 설계"]
  },
  "comparisonData": {
    "title": "핵심 비교 분석: REST vs GraphQL",
    "description": "데이터를 요청하고 소비하는 과정에서의 근본적인 차이점을 비교합니다.",
    "comparisonCriteria": [
      {
        "criterion": "데이터 요청 방식 (Flexibility)",
        "description": "클라이언트가 필요한 필드만 지정할 수 있는가?",
        "restValue": {
          "score": "낮음",
          "detail": "고정된 엔드포인트 구조로, 필요한 필드와 관계없이 전체 리소스를 가져옴 (Over-fetching 위험)."
        },
        "graphqlValue": {
          "score": "높음",
          "detail": "스키마를 통해 원하는 필드만 지정 가능하여 데이터 전송 효율성이 극대화됨."
        }
      },
      {
        "criterion": "개발 속도 및 구현 복잡도",
        "description": "새로운 리소스가 추가되거나 변경될 때의 대응 용이성.",
        "restValue": {
          "score": "직관적",
          "detail": "리소스(Resource) 중심의 설계로 개념 이해가 쉽고, 초기 구현은 단순함."
        },
        "graphqlValue": {
          "score": "복잡함",
          "detail": "강력한 타입 시스템과 스키마를 유지해야 하므로, 초기 설계와 서버 측 복잡도가 높음."
        }
      },
      {
        "criterion": "운영 책임 (Operational Burden)",
        "description": "시스템 운영 및 유지보수 단계에서 발생하는 부하.",
        "restValue": {
          "score": "낮음",
          "detail": "캐싱 전략(HTTP Cache)이 직관적이고 표준화되어 운영 부하가 적음."
        },
        "graphqlValue": {
          "score": "중간",
          "detail": "단일 엔드포인트로 모든 것을 처리하므로, 캐싱 전략이 복잡해지거나 자체 캐싱 레이어가 필요할 수 있음."
        }
      },
      {
        "criterion": "비용 트레이드오프 (Performance)",
        "description": "네트워크 대역폭 및 처리 속도 관점의 효율성.",
        "restValue": {
          "score": "변동성 높음",
          "detail": "Over-fetching으로 인해 불필요한 데이터 전송량 증가 시 네트워크 비용 상승 가능성이 높음."
        },
        "graphqlValue": {
          "score": "최적화됨",
          "detail": "정확한 데이터 전송으로 네트워크 부하를 최소화하며, 성능 관점에서 유리함."
        }
      }
    ]
  }
}
```

### 🎨 2. 프론트엔드 컴포넌트 구조 (React/JSX)

**컴포넌트 이름:** `ComparisonMatrix`
**설명:** API로부터 받은 `comparisonCriteria` 배열을 순회하며, 구조화된 비교 테이블을 렌더링합니다.

```jsx
// src/components/tech-wiki/ComparisonMatrix.jsx
import React from 'react';
import './ComparisonMatrix.css';

/**
 * 기술 아키텍처 비교 분석 매트릭스 컴포넌트
 * @param {object} props - API로부터 받은 비교 데이터 객체
 */
const ComparisonMatrix = ({ data }) => {
  if (!data || !data.comparisonCriteria) return null;

  const { title, description, comparisonCriteria } = data;

  return (
    <section className="comparison-matrix-container">
      <header className="matrix-header">
        <h2>{title}</h2>
        <p className="matrix-description">{description}</p>
      </header>

      <div className="comparison-grid">
        {/* 1. 기준 컬럼 (Criteria) */}
        <div className="criterion-column">
          <h3>비교 기준</h3>
          <ul className="criteria-list">
            {comparisonCriteria.map((item, index) => (
              <li key={index}>
                <strong style={{ color: '#007bff' }}>{item.criterion}</strong>
                <p className="criterion-desc">{item.description}</p>
              </li>
            ))}
          </ul>
        </div>

        {/* 2. 비교 결과 컬럼 (REST vs GraphQL) */}
        <div className="comparison-result-columns">
          {/* REST Column */}
          <div className="comparison-column rest-column">
            <h4>REST</h4>
            <div className="rest-card">
              {comparisonCriteria.map((item, index) => (
                <div key={index} className="comparison-row">
                  <span className="score-badge" style={{ backgroundColor: '#ffc107' }}>{item.restValue.score}</span>
                  <p className="score-detail">{item.restValue.detail}</p>
                </div>
              ))}
            </div>
          </div>

          {/* GraphQL Column */}
          <div className="comparison-column graphql-column">
            <h4>GraphQL</h4>
