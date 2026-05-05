# 🎨 Designer — 개발자가 제시한 API 스키마와 데이터 흐름도를 받아, 이 데이터를 시각적으로 표현할 수 있는 '재사용 가능한 비교 분석 컴포넌트(Component)'의 Figma 디자인 브리프를 작성하시오. 특히, 아키텍처 다이어그램을 삽입할 수 있는 구조와 컬러 팔레트를 정의해야 합니다.

🎨 Designer: 작업 시작합니다.

## 📐 Figma 디자인 브리프: 비교 분석 아키텍처 컴포넌트 (Component Blueprint)

본 브리프는 개발자가 설계한 `tech_comparison_schema.json` 데이터를 기반으로, 기술 블로그 및 Wiki 페이지에서 **재사용 가능**하고 **시각적 이해도가 극대화**된 아키텍처 비교 분석 컴포넌트를 정의합니다.

### 1. 컴포넌트 개요 및 목표

*   **컴포넌트명:** Tech Comparison Card (`/components/tech-compare-card`)
*   **사용 목표:** 복잡한 기술적 개념(예: ES vs. CQRS)의 장단점, 운영 책임, 비용 트레이드오프를 30~40대 직장인 독자가 직관적으로 이해할 수 있도록 시각화.
*   **핵심 원칙:** 텍스트 설명은 최소화하고, 시각적 구조와 비교 축(Comparison Axis)을 강조합니다.

### 2. 구조 및 레이아웃 (Layout & Structure)

**[전체 구조: Grid 기반의 모듈형 카드 묶음]**

1.  **최상위 컨테이너 (Container):** 가로로 확장되는 그리드 레이아웃 (12-column grid 기준).
2.  **비교 포인트 섹션 (Comparison Point Row):**
    *   **좌측 열 (Sticky):** `comparison_point` (핵심 비교 질문)이 위치합니다. 이 열은 페이지 스크롤 시 고정(Sticky)되어 독자가 항상 어떤 질문에 대한 답을 보고 있는지 인지하도록 합니다.
    *   **우측 영역 (Dynamic):** 비교 대상 아키텍처들이 가로로 나열되는 영역입니다.

**[개별 아키텍처 카드 (Architecture Card Component)]**

*   **구조:** 각 아키텍처(예: Event Sourcing)마다 독립적인 카드를 할당합니다.
*   **헤더:** 아키텍처 이름 (`name`)과 약어 (`acronym`)를 크게 배치합니다.
*   **핵심 요약 (Summary):** 짧은 한 문장의 핵심 설명 (`summary`)을 배치합니다.
*   **데이터 블록 (Data Block):**
    *   `storage_model` / `consistency_model` 등 API 스키마의 속성별로 작은 아이콘과 함께 텍스트를 표시합니다.
    *   **[핵심 개선점]** 단순 텍스트 대신, **아이콘과 태그**를 활용하여 시각적 구분을 명확히 합니다. (예: `Append-Only` $\rightarrow$ 💾 아이콘 + 태그)
*   **다이어그램 영역 (Diagram Placeholder):**
    *   각 아키텍처 카드 하단에 가장 넓은 공간을 할애합니다.
    *   **Placeholder:** `[아키텍처 흐름 다이어그램 삽입 영역 (Aspect Ratio 16:9)]` 텍스트를 넣어, 개발자가 여기에 Flowchart나 State Diagram을 삽입할 수 있도록 공간을 확보합니다. (이 부분이 콘텐츠의 깊이를 결정합니다.)

### 3. 비주얼 시스템 (Visual System)

#### A. 컬러 팔레트 (Color Palette)

| 역할 | 색상명 | HEX 코드 | 사용 목적 |
| :--- | :--- | :--- | :--- |
| **Primary (메인)** | Hooneyz Deep Blue | `#2C3E50` | 배경, 제목, 주요 텍스트, 고정(Sticky) 요소. (전문성/신뢰) |
| **Secondary (강조)** | Tech Accent Orange | `#FF8D00` | **경고/주의점**, 핵심 키워드, CTA 버튼, *최대 장점* 강조. (친근함/시선 집중) |
| **Tertiary (보조)** | Light Gray Background | `#F4F7FA` | 섹션 배경, 카드 배경, 가독성 확보. |
| **Neutral (텍스트)** | Dark Gray | `#333333` | 본문 텍스트. |

#### B. 타이포그래피 (Typography)

*   **메인 폰트:** Pretendard (가독성이 뛰어나고 모던한 느낌)
*   **제목 (H2, H3):** Pretendard Bold (24px ~ 32px)
*   **본문/라벨:** Pretendard Regular (16px ~ 18px)

### 4. 인터랙션 및 상호작용 (Interaction & UX)

1.  **호버 효과 (Hover State):** 마우스 커서가 아키텍처 카드 위에 올라가면, 해당 카드의 배경색이 `Light Gray Background`에서 `Secondary (Orange)`의 매우 연한 톤(e.g., `#FFF0E6`)으로 미묘하게 변하며, 카드의 테두리(Border)에 `Secondary` 색상의 얇은 라인이 생깁니다.
2.  **접근성 (Accessibility):** 모든 비교 포인트의 텍스트는 충분한 대비(Contrast)를 유지해야 합니다. (Primary Blue 텍스트 $\rightarrow$ Light Gray Background)

---
📊 평가: 완료 — API 스키마를 기반으로 실행 가능한 구체적인 디자인 시스템과 컴포넌트 구조가 정의되었습니다.
📝 다음 단계: Developer 에이전트 — 정의된 컴포넌트 구조에 맞춰, 실제 Figma 컴포넌트 파일 및 CSS/React 컴포넌트 코드를 작성해야 합니다.
