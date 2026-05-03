# 💻 Developer — Tech Portfolio Blog Website의 핵심 페이지 템플릿(Tech Report/Wiki 포스팅 페이지)의 기본 코드 구조를 설계하세요. 특히, 콘텐츠가 쉽게 업데이트될 수 있도록 CMS 연동이 가능한 형태의 기본 HTML/CSS 구조와, 기사 본문 데이터를 관리할 API 엔드포인트 초안을 정의해 주세요.

💻 Developer: 작업 시작합니다.

CEO 지시에 따라, Tech Portfolio Blog Website의 핵심 콘텐츠 템플릿과 이를 지원하는 API 엔드포인트 초안을 설계했습니다. 이 구조는 CMS(Content Management System) 연동을 염두에 두고, 콘텐츠 블록 단위로 데이터를 관리할 수 있도록 설계되었습니다.

---

## 🌐 1. Tech Report 포스팅 페이지 기본 구조 (HTML/CSS Skeleton)

가장 가독성이 높고, 콘텐츠가 블록 단위로 분리되어 편집기에서 수정하기 용이하도록 구조화했습니다. (CSS는 가독성과 레이아웃을 위한 최소한의 스타일링만 포함합니다.)

**`templates/tech-report-post.html`**

```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{{ article.title }} | 후니즈닷컴 Tech Report</title>
    <link rel="stylesheet" href="/css/main.css">
    <!-- 가독성 향상을 위한 구글 폰트 등 외부 리소스 연결 위치 -->
</head>
<body>

    <header class="site-header">
        <div class="container">
            <h1 class="site-logo">후니즈닷컴</h1>
            <nav>
                <a href="/">Home</a>
                <a href="/report">Tech Report</a>
                <a href="/wiki">Tech Wiki</a>
                <a href="/about">About</a>
            </nav>
        </div>
    </header>

    <main class="container content-layout">
        <!-- ⭐️ 메인 콘텐츠 영역 (가장 중요) -->
        <article class="tech-article">
            
            <!-- 1. 기사 헤더 및 메타데이터 -->
            <header class="article-header">
                <h1 class="article-title">{{ article.title }}</h1>
                <p class="article-subtitle">{{ article.subtitle }}</p>
                <div class="article-meta">
                    <span class="meta-item">📅 작성일: {{ article.date }}</span>
                    <span class="meta-item">🧑‍💻 작성자: {{ article.author }}</span>
                    <span class="meta-item">🏷️ 카테고리: {{ article.category }}</span>
                </div>
                <hr>
            </header>

            <!-- 2. 본문 콘텐츠 영역 (CMS가 데이터를 주입하는 곳) -->
            <section class="article-body" id="main-content">
                <!-- 
                [핵심] 이 영역은 API로부터 받은 'contentBlocks' 배열을 순회하며 
                <h2 class="block-h2">, <p class="block-p">, <pre class="block-code"> 등의 태그로 렌더링합니다.
                -->
                {% for block in article.contentBlocks %}
                <div class="content-block {{ block.type }}">
                    {% if block.type == 'h2' %}
                        <h2 class="block-h2">{{ block.content }}</h2>
                    {% elif block.type == 'h3' %}
                        <h3 class="block-h3">{{ block.content }}</h3>
                    {% elif block.type == 'paragraph' %}
                        <p class="block-p">{{ block.content }}</p>
                    {% elif block.type == 'list' %}
                        <ul class="block-list">{{ block.content | safe }}</ul>
                    {% elif block.type == 'code' %}
                        <pre class="block-code"><code>{{ block.content }}</code></pre>
                    {% else %}
                        <p class="block-p">{{ block.content }}</p>
                    {% endif %}
                </div>
                {% endfor %}
            </section>
        </article>

        <!-- ⭐️ 사이드바 영역 (관련 정보, 목차, CTA) -->
        <aside class="sidebar">
            <div class="widget related-posts">
                <h3>🔗 관련 주제</h3>
                <!-- API 호출로 관련 포스트 목록을 가져와 표시 -->
                <ul>
                    <li><a href="/report/graphql-vs-rest">GraphQL vs REST: 데이터 계약의 명확성</a></li>
                    <li><a href="/report/microservices-vs-serverless">Microservices vs Serverless: 운영 트레이드오프</a></li>
                </ul>
            </div>
            <div class="widget call-to-action">
                <h4>💡 다음 스텝을 놓치지 마세요</h4>
                <p>Tech Wiki에 기여하여 회사의 지식을 확장해 보세요.</p>
                <button>📝 Wiki 기여 가이드 보기</button>
            </div>
        </aside>
    </main>

    <footer class="site-footer">
        <div class="container">
            <p>&copy; 2024 후니즈닷컴. All rights reserved.</p>
        </div>
    </footer>

    <!-- 클라이언트 측 스크립트 로딩 위치 -->
    <script src="/js/main.js"></script>
</body>
</html>
```

### 🎨 2. 핵심 CSS 스타일링 컨셉 (가독성 중심)

`main.css`에 적용되어야 할 핵심 스타일링 지침입니다.

```css
/* --- Global & Typography --- */
:root {
    --color-primary: #007bff; /* JAY CORP 메인 컬러 */
    --color-secondary: #
