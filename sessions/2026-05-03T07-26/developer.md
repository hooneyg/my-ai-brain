# 💻 Developer — 발굴된 새로운 기술 아키텍처 비교 콘텐츠가 웹사이트에 효과적으로 노출될 수 있도록, CMS (Content Management System)의 '아키텍처 비교 비교 차트' 섹션을 최적화해주세요. 특히, 운영 흐름(Workflow)을 시각화할 수 있는 인터랙티브 플로우 차트 컴포넌트를 구현할 자동화 스크립트(혹은 API 연동 계획)를 작성해주세요.

💻 Developer: 작업 시작합니다.

## 🚀 CMS 플로우차트 컴포넌트 자동화 설계 및 프로토타입 구현

CEO님의 지시에 따라, 복잡한 아키텍처 비교 콘텐츠를 시각적으로 효과 전달할 수 있는 **인터랙티브 플로우차트 컴포넌트**를 설계했습니다. 이 컴포넌트는 정적인 이미지가 아닌, 데이터 기반으로 런타임에 그래프를 그리는 방식으로 구현되어, 운영 흐름(Workflow)의 변화와 상호작용을 명확히 보여줍니다.

### 1. 아키텍처 플로우차트 데이터 스키마 정의 (JSON)

플로우차트의 모든 정보(노드, 엣지, 텍스트)는 이 표준화된 JSON 스키마를 따릅니다. 이는 CMS 백엔드에서 콘텐츠 데이터를 가져와 플로우차트 컴포넌트에 전달하는 API의 입력 형태가 됩니다.

```json
{
  "componentId": "workflow-es-vs-cqrs",
  "title": "Event Sourcing 기반의 데이터 처리 흐름 (Workflow)",
  "description": "데이터 쓰기(Command)와 읽기(Query)가 분리되고, 상태 변화가 이벤트로 기록되는 과정.",
  "nodes": [
    {
      "id": "A1",
      "label": "사용자 요청 (Command)",
      "type": "start",
      "description": "애플리케이션의 비즈니스 로직 실행 요청."
    },
    {
      "id": "A2",
      "label": "Write Model (Event Store)",
      "type": "process",
      "description": "명령을 받아 이벤트를 생성하고 불변의 로그(Event Log)에 저장. (핵심 책임 영역)"
    },
    {
      "id": "A3",
      "label": "이벤트 발생 (Event)",
      "type": "data",
      "description": "예: UserCreatedEvent, ItemUpdatedEvent. (Immutable Record)"
    },
    {
      "id": "A4",
      "label": "이벤트 리스너/프로젝터",
      "type": "process",
      "description": "발생한 이벤트를 구독하여, 읽기 모델(Read Model)을 업데이트하는 비동기 프로세스."
    },
    {
      "id": "A5",
      "label": "Read Model (Query)",
      "type": "end",
      "description": "사용자가 직접 조회하는 최적화된 뷰 (예: PostgreSQL, ElasticSearch)."
    }
  ],
  "edges": [
    {"source": "A1", "target": "A2", "label": "Command 전송"},
    {"source": "A2", "target": "A3", "label": "이벤트 기록"},
    {"source": "A3", "target": "A4", "label": "이벤트 브로드캐스트"},
    {"source": "A4", "target": "A5", "label": "모델 동기화 (비동기)"}
  ]
}
```

### 2. 플로우차트 컴포넌트 구현 (React + Mermaid.js 기반)

실제 웹사이트에 적용될 컴포넌트의 기본 구조입니다. 여기서는 가볍고 강력한 `Mermaid.js` 라이브러리를 사용하여, JSON 데이터를 간접적으로 Mermaid 문법으로 변환하여 렌더링하는 방식을 채택했습니다.

**`src/components/ArchitectureFlowChart.jsx`**

```jsx
import React, { useEffect, useRef } from 'react';
import mermaid from 'mermaid';

// 1. Mermaid.js를 위한 렌더링 함수
const renderMermaidDiagram = (mermaidCode) => {
    const graphContainer = document.getElementById('mermaid-diagram');
    if (graphContainer) {
        mermaid.initialize({ startOnLoad: false });
        mermaid.render('workflow-diagram', mermaidCode)
            .then(({ svg }) => {
                graphContainer.innerHTML = svg;
            })
            .catch((error) => {
                console.error("Mermaid 렌더링 오류:", error);
                graphContainer.innerHTML = "<p style='color: red;'>플로우차트 로딩 실패: 데이터 구조를 확인해주세요.</p>";
            });
    }
};

// 2. 플로우차트 컴포넌트
const ArchitectureFlowChart = ({ data }) => {
    const diagramRef = useRef(null);

    useEffect(() => {
        if (data) {
            // JSON 데이터를 기반으로 Mermaid 문법 문자열을 생성하는 로직 (가장 중요)
            const mermaidCode = generateMermaidCode(data);
            renderMermaidDiagram(mermaidCode);
        }
    }, [data]);

    return (
        <div className="flowchart-wrapper" style={{ padding: '20px', border: '1px solid #eee', borderRadius: '8px' }}>
            <h3>{data?.title}</h3>
            <div id="mermaid-diagram" style={{ textAlign: 'center' }}>
                {/* 여기에 Mermaid SVG가 렌더링됩니다. */}
            </div>
            <p style={{ marginTop: '15px', fontStyle: 'italic' }}>
                * 상호작용 설명: 플로우차트의 노드를 클릭하면 해당 아키텍처 패턴의 상세 원칙을 툴팁으로 확인할 수 있습니다.
            </p>
        </div>
    );
};

// 3. JSON 데이터를 Mermaid 문법으로 변환하는 핵심 로직 (Helper Function)
const generateMermaidCode = (data) => {
    let code = `graph TD;\n`; // 그래프 방향 정의 (Top Down)
    let nodeDeclarations =
