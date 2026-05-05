# 💻 Developer — 연구자에게 받은 핵심 비교 아키텍처 주제 2가지를 기반으로, 이 주제들을 비교 분석할 수 있는 'API 스키마 초안(JSON Schema)'을 설계하고, 프론트엔드에서 이 스키마를 소비할 수 있는 데이터 파이프라인의 기본 구조(흐름도)를 작성하시오.

💻 Developer: 작업 시작합니다.

CEO님의 지시에 따라, Researcher가 제시한 'Event Sourcing vs. CQRS' 비교 주제를 위한 재사용 가능한 API 스키마 초안과, 이를 프론트엔드가 소비할 수 있는 데이터 파이프라인의 기본 흐름도를 설계했습니다. 이 구조는 콘텐츠 관리 시스템(CMS)의 API 레이어에서 직접 데이터를 가져와 비교 컴포넌트로 전송하는 것을 목표로 합니다.

---

### 1. 💾 API 스키마 초안 (JSON Schema Draft)

이 스키마는 비교 분석 콘텐츠를 위한 핵심 구조를 정의하며, 모든 비교 아키텍처(A, B, C...)에 공통적으로 적용할 수 있도록 설계되었습니다.

**파일명:** `tech_comparison_schema.json`

```json
{
  "title": "Tech Architecture Comparison Data",
  "type": "array",
  "items": {
    "type": "object",
    "properties": {
      "comparison_point": {
        "description": "비교하는 핵심 주제 (예: 데이터 모델, 일관성 모델)",
        "type": "string"
      },
      "architectures": {
        "type": "array",
        "description": "비교 대상 아키텍처들의 데이터 묶음",
        "items": {
          "type": "object",
          "properties": {
            "name": {
              "description": "아키텍처 이름 (예: Event Sourcing)",
              "type": "string"
            },
            "acronym": {
              "description": "약어 (예: ES)",
              "type": "string"
            },
            "summary": {
              "description": "핵심 요약 설명",
              "type": "string"
            },
            "data": {
              "type": "object",
              "properties": {
                "storage_model": {
                  "description": "데이터 저장 방식 (예: Append-Only, Read/Write DB)",
                  "type": "string"
                },
                "consistency_model": {
                  "description": "일관성 모델 (예: Eventual Consistency, Strong Consistency)",
                  "type": "string"
                },
                "op_responsibility": {
                  "description": "운영 책임/복잡도 (Operational Overhead)",
                  "type": "string"
                },
                "cost_tradeoff": {
                  "description": "비용 트레이드오프 (Cost implication)",
                  "type": "string"
                },
                "pros": {
                  "description": "장점 목록",
                  "type": "array",
                  "items": {"type": "string"}
                },
                "cons": {
                  "description": "단점 목록",
                  "type": "array",
                  "items": {"type": "string"}
                }
              },
              "required": ["storage_model", "consistency_model", "op_responsibility", "cost_tradeoff"]
            }
          },
          "required": ["name", "acronym", "data"]
        }
      }
    },
    "required": ["comparison_point", "architectures"]
  }
}
```

#### 💡 JSON 데이터 예시 (샘플 데이터)

```json
[
  {
    "comparison_point": "데이터 저장 방식",
    "architectures": [
      {
        "name": "Event Sourcing",
        "acronym": "ES",
        "summary": "모든 상태 변화를 이벤트 로그로 저장하며, 데이터 불변성을 유지함.",
        "data": {
          "storage_model": "Append-Only Event Log",
          "consistency_model": "Eventual Consistency",
          "op_responsibility": "높음 (이벤트 재처리/뷰 관리 필요)",
          "cost_tradeoff": "중간 (이벤트 스트림 처리 시스템 비용)",
          "pros": ["높은 감사(Audit) 기능", "시간 여행 디버깅 가능"],
          "cons": ["쿼리 로직 복잡도 증가", "실시간 읽기 접근 어려움"]
        }
      },
      {
        "name": "CQRS",
        "acronym": "CQRS",
        "summary": "명령(Command)과 조회(Query)를 분리하여, 각각에 최적화된 데이터 모델을 사용.",
        "data": {
          "storage_model": "Write Model (DB A) + Read Model (DB B)",
          "consistency_model": "선택 가능 (제어된 일관성)",
          "op_responsibility": "중간 (동기화 로직 구현 필요)",
          "cost_tradeoff": "높음 (다중 DB 및 동기화 레이어 비용)",
          "pros": ["읽기 성능 최적화 용이", "기술 스택 분리 용이"],
          "cons": ["데이터 동기화 메커니즘 복잡", "초기 설계 복잡도 높음"]
        }
      }
    ]
  }
]
```

---

### 2. 🌊 데이터 파이프라인 기본 구조 (Flowchart/Architecture Diagram)

이 구조는 API 게이트웨이를 통해 데이터를 요청하고, 백엔드가 복잡한 비교 데이터를 필요한 스키마로 가공하여 프론트엔드에 제공하는 흐름을 보여줍니다.

**Flow Diagram (Mermaid 형식 - 개발 문서화 용도)**

```mermaid
graph TD
    A[CMS/Source Data: Articles DB] --> B(Backend Service: Tech Comparison Service);
    B --> C{Data Aggregator & Processor};
    C --> D[1. Schema Mapping & Transformation];
    D --> E[2. API Endpoint: /api/tech-compare/{slug}];
    E --> F{JSON Schema Output};
    F --> G[Frontend Component: ComparisonTable.jsx];
    
    subgraph Data Flow
        A -- raw data (raw JSON/DB records) --> B;
        B --
