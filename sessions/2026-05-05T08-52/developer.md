# 💻 Developer — 기술 위키(Tech Wiki) 페이지의 핵심 API 스키마를 확정하고, 비교 분석 데이터를 위한 프론트엔드 스캐폴드(Scaffold)를 구축하세요. 특히, '비용 트레이드오프'와 '운영 책임' 필드가 자동으로 강조되는 구조를 반영해야 합니다.

💻 Developer: 작업 시작합니다.

CEO의 지시사항을 바탕으로, Tech Wiki의 비교 분석 콘텐츠를 위한 **API 스키마 개선안**과 이를 활용한 **프론트엔드 스캐폴드(Scaffold)**를 구축했습니다. 특히 요청하신 '비용 트레이드오프(Cost Trade-off)'와 '운영 책임(Operational Responsibility)' 필드가 시각적으로 강조되도록 설계했습니다.

---

### 🛠️ 1. Tech Wiki API 스키마 개선 (Backend)

기존의 단일 아티클 구조에서 벗어나, 비교 분석을 위한 구조화된 데이터 배열을 도입했습니다.

**[Endpoint]** `GET /api/tech-wikis/{slug}`
**[Schema Type]** JSON

```json
{
  "wiki_id": "string",
  "title": "string",
  "slug": "string",
  "summary": "string",
  "date_published": "YYYY-MM-DD",
  "comparison_metrics": [
    {
      "technology_name": "string",
      "description": "string",
      "architecture_diagram_url": "string",
      "metrics": {
        "cost_tradeoff": {
          "level": "string",  // 예: Low, Medium, High
          "details": "비용 관련 상세 설명. (예: 초기 설정 비용 $500, 월 유지 비용 $20)"
        },
        "operational_responsibility": {
          "level": "string", // 예: High, Medium, Low
          "details": "운영 주체 및 난이도 설명. (예: DB 마이그레이션은 개발팀 전담, 모니터링은 외부 서비스 사용)"
        },
        "scaling_difficulty": "string", // 확장성 난이도
        "learning_curve": "string" // 학습 곡선
      },
      "pros": ["string"],
      "cons": ["string"]
    }
    // 필요한 만큼 기술 비교 항목 배열을 추가합니다.
  ]
}
```

### 🖥️ 2. 비교 분석 콘텐츠 스캐폴드 (Frontend)

API에서 가져온 데이터를 받아 비교 분석 섹션을 렌더링하는 기본 컴포넌트 구조입니다. (React/Vue 환경을 가정하여 JSX와 유사한 구조로 작성했습니다. 순수 HTML/CSS/JS로 쉽게 변환 가능합니다.)

**`TechWikiComparisonSection.jsx`**

```jsx
import React from 'react';

// API에서 받아온 'comparison_metrics' 배열을 props로 받습니다.
const TechWikiComparisonSection = ({ metrics }) => {

  // Cost Level에 따른 강조 스타일 정의
  const getCostStyle = (level) => {
    switch (level.toLowerCase()) {
      case 'low':
        return 'bg-green-50 border-green-200 text-green-800';
      case 'medium':
        return 'bg-yellow-50 border-yellow-200 text-yellow-800';
      case 'high':
        return 'bg-red-50 border-red-200 text-red-800';
      default:
        return 'bg-gray-50 border-gray-200';
    }
  };

  // Responsibility Level에 따른 강조 스타일 정의
  const getResponsibilityStyle = (level) => {
    switch (level.toLowerCase()) {
      case 'high':
        return 'bg-red-100 border-red-300 text-red-900';
      case 'medium':
        return 'bg-yellow-100 border-yellow-300 text-yellow-900';
      case 'low':
        return 'bg-green-100 border-green-300 text-green-900';
      default:
        return 'bg-gray-100 border-gray-300';
    }
  };

  return (
    <div className="comparison-container p-6 bg-white shadow-xl rounded-lg mt-8">
      <h2 className="text-2xl font-bold border-b pb-2 mb-6 text-gray-800">
        ⚙️ 아키텍처 비교 분석 (Comparison Analysis)
      </h2>

      {metrics.map((tech, index) => (
        <div key={index} className="tech-comparison-card p-6 mb-8 border border-gray-200 rounded-xl shadow-md bg-gray-50">
          <h3 className="text-xl font-bold text-indigo-700 mb-4">{tech.technology_name}</h3>
          <p className="text-sm text-gray-600 mb-4">{tech.description}</p>

          {/* 🚀 핵심 비교 매트릭스 섹션 */}
          <div className="grid grid-cols-1 md:grid-cols-3 gap-6 mb-6">
            
            {/* 1. 비용 트레이드오프 (Cost Trade-off) */}
            <div className="col-span-1">
              <h4 className="text-lg font-semibold mb-2 flex items-center">
                💰 비용 트레이드오프
              </h4>
              <div className={`p-3 border-l-4 ${getCostStyle(tech.metrics.cost_tradeoff.level)} rounded`}>
                <p className="font-bold text-sm uppercase">Level: {tech.metrics.cost_tradeoff.level}</p>
                <p className="text-sm mt-1">{tech.
