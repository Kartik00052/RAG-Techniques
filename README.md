<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>RAG Techniques — Implementation Repository</title>
<link href="https://fonts.googleapis.com/css2?family=DM+Serif+Display:ital@0;1&family=DM+Mono:wght@400;500&family=DM+Sans:ital,opsz,wght@0,9..40,300;0,9..40,400;0,9..40,500;0,9..40,600;1,9..40,300&display=swap" rel="stylesheet"/>
<style>
  :root {
    --bg: #f8f7f4;
    --surface: #ffffff;
    --surface-2: #f2f0ec;
    --border: #e4e0d8;
    --text: #1a1915;
    --text-muted: #6b6860;
    --text-light: #9b9890;
    --accent: #1e6b4a;
    --accent-light: #e8f3ee;
    --accent-2: #c84b16;
    --accent-2-light: #fdf0eb;
    --accent-3: #1a4f8a;
    --accent-3-light: #eaf0f9;
    --accent-4: #6b3fa0;
    --accent-4-light: #f3eefb;
    --yellow: #b8860b;
    --yellow-light: #fdf8e8;
    --tag-bg: #eee;
    --radius: 10px;
    --radius-lg: 16px;
    --shadow: 0 1px 3px rgba(0,0,0,0.06), 0 4px 16px rgba(0,0,0,0.04);
    --shadow-hover: 0 4px 12px rgba(0,0,0,0.08), 0 12px 32px rgba(0,0,0,0.06);
  }

  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  html { scroll-behavior: smooth; }

  body {
    font-family: 'DM Sans', sans-serif;
    background: var(--bg);
    color: var(--text);
    line-height: 1.7;
    font-size: 15px;
    min-height: 100vh;
  }

  /* ── LAYOUT ── */
  .page { max-width: 960px; margin: 0 auto; padding: 0 24px 80px; }

  /* ── HERO ── */
  .hero {
    padding: 72px 0 48px;
    border-bottom: 1px solid var(--border);
    margin-bottom: 56px;
    position: relative;
    overflow: hidden;
  }
  .hero::before {
    content: '';
    position: absolute;
    top: -40px; right: -60px;
    width: 320px; height: 320px;
    background: radial-gradient(circle, rgba(30,107,74,0.07) 0%, transparent 70%);
    border-radius: 50%;
    pointer-events: none;
  }
  .hero-badge {
    display: inline-flex;
    align-items: center;
    gap: 6px;
    background: var(--accent-light);
    color: var(--accent);
    border: 1px solid rgba(30,107,74,0.2);
    border-radius: 100px;
    padding: 4px 12px;
    font-size: 12px;
    font-weight: 600;
    letter-spacing: 0.04em;
    text-transform: uppercase;
    margin-bottom: 20px;
    animation: fadeUp 0.5s ease both;
  }
  .hero-badge span { font-size: 14px; }
  h1 {
    font-family: 'DM Serif Display', serif;
    font-size: clamp(2rem, 5vw, 3.2rem);
    line-height: 1.18;
    letter-spacing: -0.02em;
    color: var(--text);
    margin-bottom: 16px;
    animation: fadeUp 0.5s 0.1s ease both;
  }
  h1 em {
    font-style: italic;
    color: var(--accent);
  }
  .hero-desc {
    font-size: 17px;
    color: var(--text-muted);
    max-width: 580px;
    margin-bottom: 32px;
    font-weight: 300;
    animation: fadeUp 0.5s 0.2s ease both;
  }
  .hero-stats {
    display: flex;
    gap: 32px;
    flex-wrap: wrap;
    animation: fadeUp 0.5s 0.3s ease both;
  }
  .stat {
    display: flex;
    flex-direction: column;
    gap: 2px;
  }
  .stat-num {
    font-family: 'DM Serif Display', serif;
    font-size: 2rem;
    color: var(--text);
    line-height: 1;
  }
  .stat-label {
    font-size: 12px;
    color: var(--text-light);
    text-transform: uppercase;
    letter-spacing: 0.06em;
    font-weight: 500;
  }
  .hero-divider {
    width: 40px; height: 1px;
    background: var(--border);
    margin: 0 8px;
    align-self: center;
  }

  /* ── SECTION HEADERS ── */
  .section-header {
    margin-bottom: 28px;
    animation: fadeUp 0.5s ease both;
  }
  .section-label {
    font-size: 11px;
    font-weight: 600;
    letter-spacing: 0.1em;
    text-transform: uppercase;
    color: var(--text-light);
    margin-bottom: 6px;
  }
  h2 {
    font-family: 'DM Serif Display', serif;
    font-size: 1.6rem;
    letter-spacing: -0.01em;
    color: var(--text);
  }

  /* ── PIPELINE DIAGRAM ── */
  .pipeline-section { margin-bottom: 60px; }
  .pipeline-wrap {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: var(--radius-lg);
    padding: 32px 28px;
    box-shadow: var(--shadow);
    overflow-x: auto;
  }
  .pipeline {
    display: flex;
    align-items: center;
    gap: 0;
    min-width: 560px;
  }
  .p-step {
    flex: 1;
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 10px;
    cursor: default;
    position: relative;
  }
  .p-icon {
    width: 52px; height: 52px;
    border-radius: 12px;
    display: flex; align-items: center; justify-content: center;
    font-size: 20px;
    transition: transform 0.2s ease, box-shadow 0.2s ease;
  }
  .p-step:hover .p-icon {
    transform: translateY(-3px);
    box-shadow: 0 6px 20px rgba(0,0,0,0.1);
  }
  .p-step-label { font-size: 12px; font-weight: 600; text-align: center; color: var(--text); }
  .p-step-sub { font-size: 10.5px; text-align: center; color: var(--text-light); max-width: 88px; line-height: 1.4; }
  .p-arrow {
    flex-shrink: 0;
    width: 28px;
    display: flex; align-items: center; justify-content: center;
    color: var(--border);
    font-size: 18px;
    margin-bottom: 24px;
  }

  /* ── FILTER TABS ── */
  .filter-section { margin-bottom: 24px; }
  .filter-tabs {
    display: flex;
    gap: 8px;
    flex-wrap: wrap;
  }
  .filter-btn {
    padding: 6px 16px;
    border-radius: 100px;
    border: 1px solid var(--border);
    background: var(--surface);
    color: var(--text-muted);
    font-size: 13px;
    font-weight: 500;
    font-family: 'DM Sans', sans-serif;
    cursor: pointer;
    transition: all 0.15s ease;
  }
  .filter-btn:hover {
    border-color: var(--accent);
    color: var(--accent);
  }
  .filter-btn.active {
    background: var(--accent);
    border-color: var(--accent);
    color: #fff;
  }

  /* ── TECHNIQUE CARDS ── */
  .techniques-section { margin-bottom: 60px; }
  .cards-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
    gap: 16px;
  }
  .card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: var(--radius-lg);
    padding: 22px;
    cursor: pointer;
    transition: all 0.2s ease;
    position: relative;
    overflow: hidden;
    animation: fadeUp 0.4s ease both;
  }
  .card::before {
    content: '';
    position: absolute;
    top: 0; left: 0; right: 0;
    height: 3px;
    border-radius: var(--radius-lg) var(--radius-lg) 0 0;
    opacity: 0;
    transition: opacity 0.2s ease;
  }
  .card:hover {
    border-color: transparent;
    box-shadow: var(--shadow-hover);
    transform: translateY(-2px);
  }
  .card:hover::before { opacity: 1; }
  .card.cat-retrieval::before { background: var(--accent); }
  .card.cat-chunking::before { background: var(--accent-2); }
  .card.cat-query::before { background: var(--accent-3); }
  .card.cat-advanced::before { background: var(--accent-4); }
  .card.cat-eval::before { background: var(--yellow); }

  .card-header { display: flex; align-items: flex-start; justify-content: space-between; gap: 12px; margin-bottom: 12px; }
  .card-icon {
    width: 38px; height: 38px;
    border-radius: 9px;
    display: flex; align-items: center; justify-content: center;
    font-size: 17px;
    flex-shrink: 0;
  }
  .card-tag {
    font-size: 10px;
    font-weight: 600;
    letter-spacing: 0.05em;
    text-transform: uppercase;
    padding: 3px 8px;
    border-radius: 100px;
  }
  .card-title {
    font-family: 'DM Serif Display', serif;
    font-size: 1.05rem;
    margin-bottom: 6px;
    color: var(--text);
    line-height: 1.3;
  }
  .card-desc {
    font-size: 13px;
    color: var(--text-muted);
    line-height: 1.6;
    margin-bottom: 14px;
  }
  .card-footer {
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 8px;
    flex-wrap: wrap;
  }
  .card-file {
    font-family: 'DM Mono', monospace;
    font-size: 10.5px;
    color: var(--text-light);
    background: var(--surface-2);
    padding: 2px 8px;
    border-radius: 5px;
  }
  .card-complexity {
    display: flex;
    gap: 3px;
    align-items: center;
  }
  .dot {
    width: 6px; height: 6px;
    border-radius: 50%;
  }
  .dot.filled { background: var(--text-muted); }
  .dot.empty { background: var(--border); }

  /* ── EXPAND DETAIL ── */
  .card-detail {
    display: none;
    margin-top: 14px;
    padding-top: 14px;
    border-top: 1px solid var(--border);
    animation: fadeIn 0.2s ease;
  }
  .card-detail.open { display: block; }
  .detail-row {
    display: flex;
    gap: 8px;
    align-items: flex-start;
    margin-bottom: 8px;
    font-size: 12.5px;
  }
  .detail-icon { flex-shrink: 0; margin-top: 1px; }
  .detail-label { font-weight: 600; color: var(--text); margin-right: 4px; }
  .detail-val { color: var(--text-muted); }
  .card-expand-btn {
    background: none; border: none; cursor: pointer;
    font-size: 12px; color: var(--text-light);
    display: flex; align-items: center; gap: 4px;
    font-family: 'DM Sans', sans-serif;
    padding: 0;
    transition: color 0.15s;
    margin-top: 4px;
  }
  .card-expand-btn:hover { color: var(--accent); }
  .card-expand-btn .arrow { transition: transform 0.2s; display: inline-block; }
  .card-expand-btn.open .arrow { transform: rotate(180deg); }

  /* ── HIDDEN ── */
  .card.hidden { display: none; }

  /* ── QUICK START ── */
  .quickstart-section { margin-bottom: 60px; }
  .steps-wrap {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: var(--radius-lg);
    overflow: hidden;
    box-shadow: var(--shadow);
  }
  .qs-step {
    display: flex;
    align-items: flex-start;
    gap: 18px;
    padding: 22px 24px;
    border-bottom: 1px solid var(--border);
    transition: background 0.15s;
  }
  .qs-step:last-child { border-bottom: none; }
  .qs-step:hover { background: var(--surface-2); }
  .qs-num {
    width: 30px; height: 30px;
    border-radius: 50%;
    background: var(--text);
    color: #fff;
    display: flex; align-items: center; justify-content: center;
    font-size: 12px;
    font-weight: 700;
    flex-shrink: 0;
    margin-top: 2px;
  }
  .qs-title { font-weight: 600; font-size: 14px; margin-bottom: 4px; }
  .qs-code {
    font-family: 'DM Mono', monospace;
    font-size: 12.5px;
    background: var(--surface-2);
    border: 1px solid var(--border);
    padding: 8px 14px;
    border-radius: 7px;
    margin-top: 8px;
    color: var(--accent);
    display: block;
    overflow-x: auto;
    white-space: pre;
  }
  .qs-desc { font-size: 13px; color: var(--text-muted); }

  /* ── CATEGORY LEGEND ── */
  .legend {
    display: flex; gap: 16px; flex-wrap: wrap;
    margin-bottom: 20px;
  }
  .legend-item {
    display: flex; align-items: center; gap: 7px;
    font-size: 12px; color: var(--text-muted);
  }
  .legend-dot {
    width: 10px; height: 10px;
    border-radius: 3px;
  }

  /* ── FOOTER ── */
  .footer {
    border-top: 1px solid var(--border);
    padding-top: 32px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    flex-wrap: wrap;
    gap: 16px;
    color: var(--text-light);
    font-size: 13px;
  }
  .footer-logo {
    font-family: 'DM Serif Display', serif;
    font-size: 1.1rem;
    color: var(--text-muted);
  }
  .footer-links {
    display: flex; gap: 18px;
  }
  .footer-links a {
    color: var(--text-light);
    text-decoration: none;
    font-size: 13px;
    transition: color 0.15s;
  }
  .footer-links a:hover { color: var(--accent); }

  /* ── SEARCH ── */
  .search-wrap {
    position: relative;
    margin-bottom: 20px;
  }
  .search-input {
    width: 100%;
    padding: 11px 16px 11px 40px;
    border: 1px solid var(--border);
    border-radius: var(--radius);
    font-family: 'DM Sans', sans-serif;
    font-size: 14px;
    background: var(--surface);
    color: var(--text);
    outline: none;
    transition: border-color 0.15s, box-shadow 0.15s;
  }
  .search-input:focus {
    border-color: var(--accent);
    box-shadow: 0 0 0 3px rgba(30,107,74,0.08);
  }
  .search-icon {
    position: absolute;
    left: 14px; top: 50%;
    transform: translateY(-50%);
    color: var(--text-light);
    font-size: 15px;
    pointer-events: none;
  }
  .results-count {
    font-size: 12px;
    color: var(--text-light);
    margin-bottom: 16px;
    height: 16px;
  }

  /* ── ANIMATIONS ── */
  @keyframes fadeUp {
    from { opacity: 0; transform: translateY(14px); }
    to   { opacity: 1; transform: translateY(0); }
  }
  @keyframes fadeIn {
    from { opacity: 0; }
    to   { opacity: 1; }
  }

  /* ── RESPONSIVE ── */
  @media (max-width: 600px) {
    h1 { font-size: 1.8rem; }
    .hero { padding: 48px 0 32px; }
    .hero-stats { gap: 20px; }
    .pipeline { min-width: 480px; }
  }
</style>
</head>
<body>
<div class="page">

  <!-- HERO -->
  <header class="hero">
    <div class="hero-badge"><span>📚</span> Open Source</div>
    <h1>RAG Techniques<br><em>Implementation Hub</em></h1>
    <p class="hero-desc">A comprehensive collection of Retrieval-Augmented Generation techniques — from simple vector search to self-reflective agents — implemented in Python with LangChain &amp; LlamaIndex.</p>
    <div class="hero-stats">
      <div class="stat">
        <span class="stat-num">19</span>
        <span class="stat-label">Techniques</span>
      </div>
      <div class="hero-divider"></div>
      <div class="stat">
        <span class="stat-num">5</span>
        <span class="stat-label">Categories</span>
      </div>
      <div class="hero-divider"></div>
      <div class="stat">
        <span class="stat-num">2</span>
        <span class="stat-label">Frameworks</span>
      </div>
      <div class="hero-divider"></div>
      <div class="stat">
        <span class="stat-num">GPT-4o</span>
        <span class="stat-label">Backbone</span>
      </div>
    </div>
  </header>

  <!-- RAG PIPELINE DIAGRAM -->
  <section class="pipeline-section">
    <div class="section-header">
      <div class="section-label">How It Works</div>
      <h2>The RAG Pipeline</h2>
    </div>
    <div class="pipeline-wrap">
      <div class="pipeline">
        <div class="p-step">
          <div class="p-icon" style="background:#eaf0f9;">📄</div>
          <div class="p-step-label">Documents</div>
          <div class="p-step-sub">PDFs, text, raw content</div>
        </div>
        <div class="p-arrow">→</div>
        <div class="p-step">
          <div class="p-icon" style="background:#fdf0eb;">✂️</div>
          <div class="p-step-label">Chunking</div>
          <div class="p-step-sub">Split into semantic pieces</div>
        </div>
        <div class="p-arrow">→</div>
        <div class="p-step">
          <div class="p-icon" style="background:#e8f3ee;">🔢</div>
          <div class="p-step-label">Embeddings</div>
          <div class="p-step-sub">Vector representations</div>
        </div>
        <div class="p-arrow">→</div>
        <div class="p-step">
          <div class="p-icon" style="background:#f3eefb;">🔍</div>
          <div class="p-step-label">Retrieval</div>
          <div class="p-step-sub">Find relevant context</div>
        </div>
        <div class="p-arrow">→</div>
        <div class="p-step">
          <div class="p-icon" style="background:#fdf8e8;">🤖</div>
          <div class="p-step-label">Generation</div>
          <div class="p-step-sub">LLM crafts the answer</div>
        </div>
      </div>
    </div>
  </section>

  <!-- TECHNIQUES -->
  <section class="techniques-section">
    <div class="section-header">
      <div class="section-label">Implementations</div>
      <h2>All Techniques</h2>
    </div>

    <div class="legend">
      <div class="legend-item"><div class="legend-dot" style="background:var(--accent)"></div>Retrieval</div>
      <div class="legend-item"><div class="legend-dot" style="background:var(--accent-2)"></div>Chunking</div>
      <div class="legend-item"><div class="legend-dot" style="background:var(--accent-3)"></div>Query</div>
      <div class="legend-item"><div class="legend-dot" style="background:var(--accent-4)"></div>Advanced RAG</div>
      <div class="legend-item"><div class="legend-dot" style="background:var(--yellow)"></div>Evaluation</div>
    </div>

    <div class="search-wrap">
      <span class="search-icon">🔎</span>
      <input class="search-input" id="searchInput" placeholder="Search techniques, keywords…" oninput="filterCards()" autocomplete="off"/>
    </div>

    <div class="filter-tabs" id="filterTabs">
      <button class="filter-btn active" onclick="setFilter('all', this)">All (19)</button>
      <button class="filter-btn" onclick="setFilter('retrieval', this)">Retrieval</button>
      <button class="filter-btn" onclick="setFilter('chunking', this)">Chunking</button>
      <button class="filter-btn" onclick="setFilter('query', this)">Query</button>
      <button class="filter-btn" onclick="setFilter('advanced', this)">Advanced RAG</button>
      <button class="filter-btn" onclick="setFilter('eval', this)">Evaluation</button>
    </div>

    <div class="results-count" id="resultsCount"></div>

    <div class="cards-grid" id="cardsGrid">

      <!-- 1 Simple RAG -->
      <div class="card cat-retrieval" data-cat="retrieval" data-keywords="simple basic rag vector faiss baseline">
        <div class="card-header">
          <div class="card-icon" style="background:var(--accent-light);">🗂️</div>
          <span class="card-tag" style="background:var(--accent-light);color:var(--accent);">Retrieval</span>
        </div>
        <div class="card-title">Simple RAG</div>
        <div class="card-desc">The foundational RAG pattern — chunk a PDF, embed with OpenAI, store in FAISS, and retrieve the top-k chunks for any query.</div>
        <div class="card-footer">
          <span class="card-file">simple_rag.py</span>
          <div class="card-complexity" title="Complexity: 1/3">
            <div class="dot filled"></div><div class="dot empty"></div><div class="dot empty"></div>
          </div>
        </div>
        <button class="card-expand-btn" onclick="toggleDetail(this)">Details <span class="arrow">▾</span></button>
        <div class="card-detail">
          <div class="detail-row"><span class="detail-icon">⚙️</span><span><span class="detail-label">Key idea:</span><span class="detail-val">Fixed-size chunking + semantic similarity search.</span></span></div>
          <div class="detail-row"><span class="detail-icon">📦</span><span><span class="detail-label">Libs:</span><span class="detail-val">LangChain, FAISS, OpenAIEmbeddings</span></span></div>
          <div class="detail-row"><span class="detail-icon">▶️</span><span><span class="detail-label">Run:</span></span></div>
          <code class="qs-code">python simple_rag.py --path doc.pdf --query "..."</code>
        </div>
      </div>

      <!-- 2 Semantic Chunking -->
      <div class="card cat-chunking" data-cat="chunking" data-keywords="semantic chunk split breakpoint percentile">
        <div class="card-header">
          <div class="card-icon" style="background:var(--accent-2-light);">✂️</div>
          <span class="card-tag" style="background:var(--accent-2-light);color:var(--accent-2);">Chunking</span>
        </div>
        <div class="card-title">Semantic Chunking</div>
        <div class="card-desc">Splits documents at semantically meaningful boundaries (percentile, std-dev, gradient) instead of arbitrary character counts.</div>
        <div class="card-footer">
          <span class="card-file">semantic_chunking.py</span>
          <div class="card-complexity" title="Complexity: 1/3">
            <div class="dot filled"></div><div class="dot empty"></div><div class="dot empty"></div>
          </div>
        </div>
        <button class="card-expand-btn" onclick="toggleDetail(this)">Details <span class="arrow">▾</span></button>
        <div class="card-detail">
          <div class="detail-row"><span class="detail-icon">⚙️</span><span><span class="detail-label">Key idea:</span><span class="detail-val">Embedding-based breakpoint detection for coherent chunks.</span></span></div>
          <div class="detail-row"><span class="detail-icon">📦</span><span><span class="detail-label">Libs:</span><span class="detail-val">LangChain SemanticChunker, FAISS</span></span></div>
          <div class="detail-row"><span class="detail-icon">▶️</span><span><span class="detail-label">Run:</span></span></div>
          <code class="qs-code">python semantic_chunking.py --breakpoint_threshold_type percentile</code>
        </div>
      </div>

      <!-- 3 Context Enrichment Window -->
      <div class="card cat-chunking" data-cat="chunking" data-keywords="context enrichment window neighbor chunk padding">
        <div class="card-header">
          <div class="card-icon" style="background:var(--accent-2-light);">🪟</div>
          <span class="card-tag" style="background:var(--accent-2-light);color:var(--accent-2);">Chunking</span>
        </div>
        <div class="card-title">Context Enrichment Window</div>
        <div class="card-desc">Retrieves a chunk then pads it with its neighboring chunks to provide richer surrounding context to the LLM.</div>
        <div class="card-footer">
          <span class="card-file">context_enrichment_window_around_chunk.py</span>
          <div class="card-complexity" title="Complexity: 2/3">
            <div class="dot filled"></div><div class="dot filled"></div><div class="dot empty"></div>
          </div>
        </div>
        <button class="card-expand-btn" onclick="toggleDetail(this)">Details <span class="arrow">▾</span></button>
        <div class="card-detail">
          <div class="detail-row"><span class="detail-icon">⚙️</span><span><span class="detail-label">Key idea:</span><span class="detail-val">Use chunk index metadata to fetch left/right neighbors.</span></span></div>
          <div class="detail-row"><span class="detail-icon">📦</span><span><span class="detail-label">Libs:</span><span class="detail-val">LangChain, FAISS</span></span></div>
          <div class="detail-row"><span class="detail-icon">▶️</span><span><span class="detail-label">Run:</span></span></div>
          <code class="qs-code">python context_enrichment_window_around_chunk.py --num_neighbors 2</code>
        </div>
      </div>

      <!-- 4 Contextual Compression -->
      <div class="card cat-retrieval" data-cat="retrieval" data-keywords="contextual compression llm extractor qa chain">
        <div class="card-header">
          <div class="card-icon" style="background:var(--accent-light);">🗜️</div>
          <span class="card-tag" style="background:var(--accent-light);color:var(--accent);">Retrieval</span>
        </div>
        <div class="card-title">Contextual Compression</div>
        <div class="card-desc">An LLM post-processes each retrieved chunk to strip irrelevant text, passing only the truly relevant fragments downstream.</div>
        <div class="card-footer">
          <span class="card-file">contextual_compression.py</span>
          <div class="card-complexity" title="Complexity: 2/3">
            <div class="dot filled"></div><div class="dot filled"></div><div class="dot empty"></div>
          </div>
        </div>
        <button class="card-expand-btn" onclick="toggleDetail(this)">Details <span class="arrow">▾</span></button>
        <div class="card-detail">
          <div class="detail-row"><span class="detail-icon">⚙️</span><span><span class="detail-label">Key idea:</span><span class="detail-val">LLMChainExtractor compresses chunks before generation.</span></span></div>
          <div class="detail-row"><span class="detail-icon">📦</span><span><span class="detail-label">Libs:</span><span class="detail-val">LangChain ContextualCompressionRetriever</span></span></div>
          <div class="detail-row"><span class="detail-icon">▶️</span><span><span class="detail-label">Run:</span></span></div>
          <code class="qs-code">python contextual_compression.py --path doc.pdf --query "..."</code>
        </div>
      </div>

      <!-- 5 Fusion Retrieval -->
      <div class="card cat-retrieval" data-cat="retrieval" data-keywords="fusion retrieval bm25 keyword vector hybrid rrf">
        <div class="card-header">
          <div class="card-icon" style="background:var(--accent-light);">⚗️</div>
          <span class="card-tag" style="background:var(--accent-light);color:var(--accent);">Retrieval</span>
        </div>
        <div class="card-title">Fusion Retrieval</div>
        <div class="card-desc">Combines BM25 keyword search and dense vector search via a weighted alpha score, capturing both lexical and semantic relevance.</div>
        <div class="card-footer">
          <span class="card-file">fusion_retrieval.py</span>
          <div class="card-complexity" title="Complexity: 2/3">
            <div class="dot filled"></div><div class="dot filled"></div><div class="dot empty"></div>
          </div>
        </div>
        <button class="card-expand-btn" onclick="toggleDetail(this)">Details <span class="arrow">▾</span></button>
        <div class="card-detail">
          <div class="detail-row"><span class="detail-icon">⚙️</span><span><span class="detail-label">Key idea:</span><span class="detail-val">score = α·vector + (1-α)·BM25. Best of both worlds.</span></span></div>
          <div class="detail-row"><span class="detail-icon">📦</span><span><span class="detail-label">Libs:</span><span class="detail-val">rank_bm25, FAISS, numpy</span></span></div>
          <div class="detail-row"><span class="detail-icon">▶️</span><span><span class="detail-label">Run:</span></span></div>
          <code class="qs-code">python fusion_retrieval.py --alpha 0.5 --k 5</code>
        </div>
      </div>

      <!-- 6 Reranking -->
      <div class="card cat-retrieval" data-cat="retrieval" data-keywords="reranking cross encoder relevance score sort">
        <div class="card-header">
          <div class="card-icon" style="background:var(--accent-light);">🏆</div>
          <span class="card-tag" style="background:var(--accent-light);color:var(--accent);">Retrieval</span>
        </div>
        <div class="card-title">Reranking</div>
        <div class="card-desc">Retrieves a large candidate set then re-scores each document with an LLM or cross-encoder to surface the most relevant results.</div>
        <div class="card-footer">
          <span class="card-file">reranking.py</span>
          <div class="card-complexity" title="Complexity: 2/3">
            <div class="dot filled"></div><div class="dot filled"></div><div class="dot empty"></div>
          </div>
        </div>
        <button class="card-expand-btn" onclick="toggleDetail(this)">Details <span class="arrow">▾</span></button>
        <div class="card-detail">
          <div class="detail-row"><span class="detail-icon">⚙️</span><span><span class="detail-label">Key idea:</span><span class="detail-val">Two-stage: broad retrieval → LLM/CrossEncoder rerank.</span></span></div>
          <div class="detail-row"><span class="detail-icon">📦</span><span><span class="detail-label">Libs:</span><span class="detail-val">sentence-transformers CrossEncoder, FAISS</span></span></div>
          <div class="detail-row"><span class="detail-icon">▶️</span><span><span class="detail-label">Run:</span></span></div>
          <code class="qs-code">python reranking.py --retriever_type cross_encoder</code>
        </div>
      </div>

      <!-- 7 Query Transformations -->
      <div class="card cat-query" data-cat="query" data-keywords="query rewrite step back decompose sub-query transformation">
        <div class="card-header">
          <div class="card-icon" style="background:var(--accent-3-light);">🔄</div>
          <span class="card-tag" style="background:var(--accent-3-light);color:var(--accent-3);">Query</span>
        </div>
        <div class="card-title">Query Transformations</div>
        <div class="card-desc">Three query strategies in one: query rewriting, step-back abstraction, and decomposition into targeted sub-queries.</div>
        <div class="card-footer">
          <span class="card-file">query_transformations.py</span>
          <div class="card-complexity" title="Complexity: 1/3">
            <div class="dot filled"></div><div class="dot empty"></div><div class="dot empty"></div>
          </div>
        </div>
        <button class="card-expand-btn" onclick="toggleDetail(this)">Details <span class="arrow">▾</span></button>
        <div class="card-detail">
          <div class="detail-row"><span class="detail-icon">⚙️</span><span><span class="detail-label">Key idea:</span><span class="detail-val">Transform ambiguous queries before retrieval for better recall.</span></span></div>
          <div class="detail-row"><span class="detail-icon">📦</span><span><span class="detail-label">Libs:</span><span class="detail-val">LangChain, ChatOpenAI GPT-4o</span></span></div>
          <div class="detail-row"><span class="detail-icon">▶️</span><span><span class="detail-label">Run:</span></span></div>
          <code class="qs-code">python query_transformations.py --query "Your question here"</code>
        </div>
      </div>

      <!-- 8 HyDE -->
      <div class="card cat-query" data-cat="query" data-keywords="hyde hypothetical document embedding query expansion">
        <div class="card-header">
          <div class="card-icon" style="background:var(--accent-3-light);">💭</div>
          <span class="card-tag" style="background:var(--accent-3-light);color:var(--accent-3);">Query</span>
        </div>
        <div class="card-title">HyDE</div>
        <div class="card-desc">Hypothetical Document Embeddings — generates a fake answer to the query, embeds it, and uses that embedding to find real similar documents.</div>
        <div class="card-footer">
          <span class="card-file">HyDe_Hypothetical_Document_Embedding.py</span>
          <div class="card-complexity" title="Complexity: 2/3">
            <div class="dot filled"></div><div class="dot filled"></div><div class="dot empty"></div>
          </div>
        </div>
        <button class="card-expand-btn" onclick="toggleDetail(this)">Details <span class="arrow">▾</span></button>
        <div class="card-detail">
          <div class="detail-row"><span class="detail-icon">⚙️</span><span><span class="detail-label">Key idea:</span><span class="detail-val">Bridge the query-document embedding gap via a synthetic document.</span></span></div>
          <div class="detail-row"><span class="detail-icon">📦</span><span><span class="detail-label">Libs:</span><span class="detail-val">LangChain, GPT-4o-mini, FAISS</span></span></div>
          <div class="detail-row"><span class="detail-icon">▶️</span><span><span class="detail-label">Run:</span></span></div>
          <code class="qs-code">python HyDe_Hypothetical_Document_Embedding.py --path doc.pdf</code>
        </div>
      </div>

      <!-- 9 HyPE -->
      <div class="card cat-query" data-cat="query" data-keywords="hype hypothetical prompt embeddings questions generation parallel">
        <div class="card-header">
          <div class="card-icon" style="background:var(--accent-3-light);">❓</div>
          <span class="card-tag" style="background:var(--accent-3-light);color:var(--accent-3);">Query</span>
        </div>
        <div class="card-title">HyPE</div>
        <div class="card-desc">Hypothetical Prompt Embeddings — augments each chunk with LLM-generated questions at index time, improving retrieval via question-to-question matching.</div>
        <div class="card-footer">
          <span class="card-file">HyPE_Hypothetical_Prompt_Embeddings.py</span>
          <div class="card-complexity" title="Complexity: 2/3">
            <div class="dot filled"></div><div class="dot filled"></div><div class="dot empty"></div>
          </div>
        </div>
        <button class="card-expand-btn" onclick="toggleDetail(this)">Details <span class="arrow">▾</span></button>
        <div class="card-detail">
          <div class="detail-row"><span class="detail-icon">⚙️</span><span><span class="detail-label">Key idea:</span><span class="detail-val">Parallel question generation per chunk → multi-vector FAISS index.</span></span></div>
          <div class="detail-row"><span class="detail-icon">📦</span><span><span class="detail-label">Libs:</span><span class="detail-val">ThreadPoolExecutor, faiss, tqdm, GPT-4o-mini</span></span></div>
          <div class="detail-row"><span class="detail-icon">▶️</span><span><span class="detail-label">Run:</span></span></div>
          <code class="qs-code">python HyPE_Hypothetical_Prompt_Embeddings.py --path doc.pdf</code>
        </div>
      </div>

      <!-- 10 Hierarchical Indices -->
      <div class="card cat-advanced" data-cat="advanced" data-keywords="hierarchical index summary page chunk two-level">
        <div class="card-header">
          <div class="card-icon" style="background:var(--accent-4-light);">🗼</div>
          <span class="card-tag" style="background:var(--accent-4-light);color:var(--accent-4);">Advanced RAG</span>
        </div>
        <div class="card-title">Hierarchical Indices</div>
        <div class="card-desc">Maintains two FAISS stores — one of page summaries, one of detailed chunks. Summary-level retrieval guides which detailed chunks to surface.</div>
        <div class="card-footer">
          <span class="card-file">hierarchical_indices.py</span>
          <div class="card-complexity" title="Complexity: 3/3">
            <div class="dot filled"></div><div class="dot filled"></div><div class="dot filled"></div>
          </div>
        </div>
        <button class="card-expand-btn" onclick="toggleDetail(this)">Details <span class="arrow">▾</span></button>
        <div class="card-detail">
          <div class="detail-row"><span class="detail-icon">⚙️</span><span><span class="detail-label">Key idea:</span><span class="detail-val">Coarse-to-fine retrieval: summary → page-filtered chunks.</span></span></div>
          <div class="detail-row"><span class="detail-icon">📦</span><span><span class="detail-label">Libs:</span><span class="detail-val">asyncio, LangChain MapReduce summarizer, FAISS</span></span></div>
          <div class="detail-row"><span class="detail-icon">▶️</span><span><span class="detail-label">Run:</span></span></div>
          <code class="qs-code">python hierarchical_indices.py --pdf_path doc.pdf</code>
        </div>
      </div>

      <!-- 11 Document Augmentation -->
      <div class="card cat-advanced" data-cat="advanced" data-keywords="document augmentation question generation qa pair index">
        <div class="card-header">
          <div class="card-icon" style="background:var(--accent-4-light);">📝</div>
          <span class="card-tag" style="background:var(--accent-4-light);color:var(--accent-4);">Advanced RAG</span>
        </div>
        <div class="card-title">Document Augmentation</div>
        <div class="card-desc">Enriches the vector index with LLM-generated Q&A pairs per document, enabling question-to-question retrieval alongside original text.</div>
        <div class="card-footer">
          <span class="card-file">document_augmentation.py</span>
          <div class="card-complexity" title="Complexity: 2/3">
            <div class="dot filled"></div><div class="dot filled"></div><div class="dot empty"></div>
          </div>
        </div>
        <button class="card-expand-btn" onclick="toggleDetail(this)">Details <span class="arrow">▾</span></button>
        <div class="card-detail">
          <div class="detail-row"><span class="detail-icon">⚙️</span><span><span class="detail-label">Key idea:</span><span class="detail-val">ORIGINAL + AUGMENTED docs share a FAISS index with type metadata.</span></span></div>
          <div class="detail-row"><span class="detail-icon">📦</span><span><span class="detail-label">Libs:</span><span class="detail-val">LangChain, GPT-4o-mini, FAISS, pydantic</span></span></div>
          <div class="detail-row"><span class="detail-icon">▶️</span><span><span class="detail-label">Run:</span></span></div>
          <code class="qs-code">python document_augmentation.py --path doc.pdf</code>
        </div>
      </div>

      <!-- 12 Graph RAG -->
      <div class="card cat-advanced" data-cat="advanced" data-keywords="graph rag knowledge graph entity relation neo4j network">
        <div class="card-header">
          <div class="card-icon" style="background:var(--accent-4-light);">🕸️</div>
          <span class="card-tag" style="background:var(--accent-4-light);color:var(--accent-4);">Advanced RAG</span>
        </div>
        <div class="card-title">Graph RAG</div>
        <div class="card-desc">Extracts entities and relationships from documents to build a knowledge graph, enabling graph-traversal-based retrieval beyond flat similarity.</div>
        <div class="card-footer">
          <span class="card-file">graph_rag.py</span>
          <div class="card-complexity" title="Complexity: 3/3">
            <div class="dot filled"></div><div class="dot filled"></div><div class="dot filled"></div>
          </div>
        </div>
        <button class="card-expand-btn" onclick="toggleDetail(this)">Details <span class="arrow">▾</span></button>
        <div class="card-detail">
          <div class="detail-row"><span class="detail-icon">⚙️</span><span><span class="detail-label">Key idea:</span><span class="detail-val">Structure knowledge as nodes/edges for multi-hop reasoning.</span></span></div>
          <div class="detail-row"><span class="detail-icon">📦</span><span><span class="detail-label">Libs:</span><span class="detail-val">LangChain, NetworkX, OpenAI</span></span></div>
          <div class="detail-row"><span class="detail-icon">▶️</span><span><span class="detail-label">Run:</span></span></div>
          <code class="qs-code">python graph_rag.py --path doc.pdf --query "..."</code>
        </div>
      </div>

      <!-- 13 RAPTOR -->
      <div class="card cat-advanced" data-cat="advanced" data-keywords="raptor tree cluster summarize recursive abstraction hierarchical">
        <div class="card-header">
          <div class="card-icon" style="background:var(--accent-4-light);">🦅</div>
          <span class="card-tag" style="background:var(--accent-4-light);color:var(--accent-4);">Advanced RAG</span>
        </div>
        <div class="card-title">RAPTOR</div>
        <div class="card-desc">Recursive Abstractive Processing — clusters documents, summarizes clusters, then re-clusters summaries across multiple tree levels for multi-granularity retrieval.</div>
        <div class="card-footer">
          <span class="card-file">raptor.py</span>
          <div class="card-complexity" title="Complexity: 3/3">
            <div class="dot filled"></div><div class="dot filled"></div><div class="dot filled"></div>
          </div>
        </div>
        <button class="card-expand-btn" onclick="toggleDetail(this)">Details <span class="arrow">▾</span></button>
        <div class="card-detail">
          <div class="detail-row"><span class="detail-icon">⚙️</span><span><span class="detail-label">Key idea:</span><span class="detail-val">Gaussian mixture clustering → recursive summarization tree.</span></span></div>
          <div class="detail-row"><span class="detail-icon">📦</span><span><span class="detail-label">Libs:</span><span class="detail-val">sklearn GaussianMixture, pandas, matplotlib, LangChain</span></span></div>
          <div class="detail-row"><span class="detail-icon">▶️</span><span><span class="detail-label">Run:</span></span></div>
          <code class="qs-code">python raptor.py --path doc.pdf --max_levels 3</code>
        </div>
      </div>

      <!-- 14 Self-RAG -->
      <div class="card cat-advanced" data-cat="advanced" data-keywords="self rag reflect critique support utility score agentic">
        <div class="card-header">
          <div class="card-icon" style="background:var(--accent-4-light);">🪞</div>
          <span class="card-tag" style="background:var(--accent-4-light);color:var(--accent-4);">Advanced RAG</span>
        </div>
        <div class="card-title">Self-RAG</div>
        <div class="card-desc">The LLM decides whether to retrieve, evaluates relevance, generates a response, then critiques its own output for support and utility before returning.</div>
        <div class="card-footer">
          <span class="card-file">self_rag.py</span>
          <div class="card-complexity" title="Complexity: 3/3">
            <div class="dot filled"></div><div class="dot filled"></div><div class="dot filled"></div>
          </div>
        </div>
        <button class="card-expand-btn" onclick="toggleDetail(this)">Details <span class="arrow">▾</span></button>
        <div class="card-detail">
          <div class="detail-row"><span class="detail-icon">⚙️</span><span><span class="detail-label">Key idea:</span><span class="detail-val">6-step self-reflective loop: retrieve → relevance → generate → support → utility → select best.</span></span></div>
          <div class="detail-row"><span class="detail-icon">📦</span><span><span class="detail-label">Libs:</span><span class="detail-val">LangChain, structured output, GPT-4o-mini</span></span></div>
          <div class="detail-row"><span class="detail-icon">▶️</span><span><span class="detail-label">Run:</span></span></div>
          <code class="qs-code">python self_rag.py --path doc.pdf --query "..."</code>
        </div>
      </div>

      <!-- 15 CRAG -->
      <div class="card cat-advanced" data-cat="advanced" data-keywords="crag corrective retrieval web search duckduckgo ambiguous evaluate">
        <div class="card-header">
          <div class="card-icon" style="background:var(--accent-4-light);">🔁</div>
          <span class="card-tag" style="background:var(--accent-4-light);color:var(--accent-4);">Advanced RAG</span>
        </div>
        <div class="card-title">CRAG</div>
        <div class="card-desc">Corrective RAG — scores retrieved docs; if confidence is too low it rewrites the query and falls back to web search (DuckDuckGo) automatically.</div>
        <div class="card-footer">
          <span class="card-file">crag.py</span>
          <div class="card-complexity" title="Complexity: 3/3">
            <div class="dot filled"></div><div class="dot filled"></div><div class="dot filled"></div>
          </div>
        </div>
        <button class="card-expand-btn" onclick="toggleDetail(this)">Details <span class="arrow">▾</span></button>
        <div class="card-detail">
          <div class="detail-row"><span class="detail-icon">⚙️</span><span><span class="detail-label">Key idea:</span><span class="detail-val">Correct → Ambiguous → Incorrect action routing with score thresholds.</span></span></div>
          <div class="detail-row"><span class="detail-icon">📦</span><span><span class="detail-label">Libs:</span><span class="detail-val">LangChain, DuckDuckGoSearchResults, FAISS</span></span></div>
          <div class="detail-row"><span class="detail-icon">▶️</span><span><span class="detail-label">Run:</span></span></div>
          <code class="qs-code">python crag.py --lower_threshold 0.3 --upper_threshold 0.7</code>
        </div>
      </div>

      <!-- 16 Adaptive Retrieval -->
      <div class="card cat-advanced" data-cat="advanced" data-keywords="adaptive retrieval factual analytical opinion contextual classify strategy">
        <div class="card-header">
          <div class="card-icon" style="background:var(--accent-4-light);">🎯</div>
          <span class="card-tag" style="background:var(--accent-4-light);color:var(--accent-4);">Advanced RAG</span>
        </div>
        <div class="card-title">Adaptive Retrieval</div>
        <div class="card-desc">Classifies the query as Factual, Analytical, Opinion, or Contextual, then routes it to a specialized retrieval strategy optimised for that type.</div>
        <div class="card-footer">
          <span class="card-file">adaptive_retrieval.py</span>
          <div class="card-complexity" title="Complexity: 3/3">
            <div class="dot filled"></div><div class="dot filled"></div><div class="dot filled"></div>
          </div>
        </div>
        <button class="card-expand-btn" onclick="toggleDetail(this)">Details <span class="arrow">▾</span></button>
        <div class="card-detail">
          <div class="detail-row"><span class="detail-icon">⚙️</span><span><span class="detail-label">Key idea:</span><span class="detail-val">QueryClassifier → strategy router (4 specialised retrievers).</span></span></div>
          <div class="detail-row"><span class="detail-icon">📦</span><span><span class="detail-label">Libs:</span><span class="detail-val">LangChain, FAISS, pydantic, GPT-4o</span></span></div>
          <div class="detail-row"><span class="detail-icon">▶️</span><span><span class="detail-label">Run:</span></span></div>
          <code class="qs-code">python adaptive_retrieval.py --texts "Your corpus here"</code>
        </div>
      </div>

      <!-- 17 Explainable Retrieval -->
      <div class="card cat-retrieval" data-cat="retrieval" data-keywords="explainable retrieval transparency explanation reasoning why relevant">
        <div class="card-header">
          <div class="card-icon" style="background:var(--accent-light);">🔬</div>
          <span class="card-tag" style="background:var(--accent-light);color:var(--accent);">Retrieval</span>
        </div>
        <div class="card-title">Explainable Retrieval</div>
        <div class="card-desc">For each retrieved chunk, an LLM produces a human-readable explanation of why it's relevant to the query — for transparent, trustable RAG.</div>
        <div class="card-footer">
          <span class="card-file">explainable_retrieval.py</span>
          <div class="card-complexity" title="Complexity: 1/3">
            <div class="dot filled"></div><div class="dot empty"></div><div class="dot empty"></div>
          </div>
        </div>
        <button class="card-expand-btn" onclick="toggleDetail(this)">Details <span class="arrow">▾</span></button>
        <div class="card-detail">
          <div class="detail-row"><span class="detail-icon">⚙️</span><span><span class="detail-label">Key idea:</span><span class="detail-val">Pair each doc with an LLM-generated relevance explanation.</span></span></div>
          <div class="detail-row"><span class="detail-icon">📦</span><span><span class="detail-label">Libs:</span><span class="detail-val">LangChain, GPT-4o-mini, FAISS</span></span></div>
          <div class="detail-row"><span class="detail-icon">▶️</span><span><span class="detail-label">Run:</span></span></div>
          <code class="qs-code">python explainable_retrieval.py --query "Why is the sky blue?"</code>
        </div>
      </div>

      <!-- 18 Feedback Loop -->
      <div class="card cat-retrieval" data-cat="retrieval" data-keywords="feedback loop fine tune relevance quality score iterative improve">
        <div class="card-header">
          <div class="card-icon" style="background:var(--accent-light);">🔃</div>
          <span class="card-tag" style="background:var(--accent-light);color:var(--accent);">Retrieval</span>
        </div>
        <div class="card-title">Retrieval with Feedback Loop</div>
        <div class="card-desc">Stores user relevance &amp; quality scores after each query, then adjusts retrieval weights and fine-tunes the vector index over time.</div>
        <div class="card-footer">
          <span class="card-file">retrieval_with_feedback_loop.py</span>
          <div class="card-complexity" title="Complexity: 2/3">
            <div class="dot filled"></div><div class="dot filled"></div><div class="dot empty"></div>
          </div>
        </div>
        <button class="card-expand-btn" onclick="toggleDetail(this)">Details <span class="arrow">▾</span></button>
        <div class="card-detail">
          <div class="detail-row"><span class="detail-icon">⚙️</span><span><span class="detail-label">Key idea:</span><span class="detail-val">JSON feedback → relevance-weighted re-scoring → periodic index re-build.</span></span></div>
          <div class="detail-row"><span class="detail-icon">📦</span><span><span class="detail-label">Libs:</span><span class="detail-val">LangChain RetrievalQA, GPT-4o, pydantic</span></span></div>
          <div class="detail-row"><span class="detail-icon">▶️</span><span><span class="detail-label">Run:</span></span></div>
          <code class="qs-code">python retrieval_with_feedback_loop.py --relevance 5 --quality 5</code>
        </div>
      </div>

      <!-- 19 Chunk Size Evaluation -->
      <div class="card cat-eval" data-cat="eval" data-keywords="chunk size evaluation faithfulness relevancy benchmarking llama index">
        <div class="card-header">
          <div class="card-icon" style="background:var(--yellow-light);">📊</div>
          <span class="card-tag" style="background:var(--yellow-light);color:var(--yellow);">Evaluation</span>
        </div>
        <div class="card-title">Chunk Size Evaluation</div>
        <div class="card-desc">Systematically benchmarks different chunk sizes across faithfulness, relevancy, and response-time metrics using LlamaIndex's evaluation framework.</div>
        <div class="card-footer">
          <span class="card-file">choose_chunk_size.py</span>
          <div class="card-complexity" title="Complexity: 2/3">
            <div class="dot filled"></div><div class="dot filled"></div><div class="dot empty"></div>
          </div>
        </div>
        <button class="card-expand-btn" onclick="toggleDetail(this)">Details <span class="arrow">▾</span></button>
        <div class="card-detail">
          <div class="detail-row"><span class="detail-icon">⚙️</span><span><span class="detail-label">Key idea:</span><span class="detail-val">DatasetGenerator → FaithfulnessEvaluator + RelevancyEvaluator per chunk size.</span></span></div>
          <div class="detail-row"><span class="detail-icon">📦</span><span><span class="detail-label">Libs:</span><span class="detail-val">LlamaIndex, OpenAI GPT-3.5/4o, nest_asyncio</span></span></div>
          <div class="detail-row"><span class="detail-icon">▶️</span><span><span class="detail-label">Run:</span></span></div>
          <code class="qs-code">python choose_chunk_size.py --chunk_sizes 128 256 512 1024</code>
        </div>
      </div>

    </div><!-- /cards-grid -->
  </section>

  <!-- QUICK START -->
  <section class="quickstart-section">
    <div class="section-header">
      <div class="section-label">Getting Started</div>
      <h2>Quick Start</h2>
    </div>
    <div class="steps-wrap">
      <div class="qs-step">
        <div class="qs-num">1</div>
        <div>
          <div class="qs-title">Clone the repository</div>
          <code class="qs-code">git clone https://github.com/your-username/rag-techniques.git
cd rag-techniques</code>
        </div>
      </div>
      <div class="qs-step">
        <div class="qs-num">2</div>
        <div>
          <div class="qs-title">Install dependencies</div>
          <code class="qs-code">pip install -r requirements.txt</code>
        </div>
      </div>
      <div class="qs-step">
        <div class="qs-num">3</div>
        <div>
          <div class="qs-title">Configure your OpenAI key</div>
          <div class="qs-desc">Create a <code style="font-family:DM Mono;font-size:12px;background:var(--surface-2);padding:1px 6px;border-radius:4px;">.env</code> file in the root directory.</div>
          <code class="qs-code">OPENAI_API_KEY=sk-your-key-here</code>
        </div>
      </div>
      <div class="qs-step">
        <div class="qs-num">4</div>
        <div>
          <div class="qs-title">Run any technique</div>
          <div class="qs-desc">Each file is self-contained with argparse CLI support.</div>
          <code class="qs-code">python simple_rag.py --path data/your_doc.pdf --query "What is X?"
python self_rag.py  --path data/your_doc.pdf --query "Explain Y"
python fusion_retrieval.py --alpha 0.6 --k 5</code>
        </div>
      </div>
    </div>
  </section>

  <!-- FOOTER -->
  <footer class="footer">
    <div class="footer-logo">RAG Techniques</div>
    <div style="color:var(--text-light);font-size:13px;">Built with LangChain · LlamaIndex · OpenAI · FAISS</div>
    <div class="footer-links">
      <a href="#">GitHub</a>
      <a href="#">Issues</a>
      <a href="#">License</a>
    </div>
  </footer>

</div><!-- /page -->

<script>
  let activeFilter = 'all';

  function setFilter(cat, btn) {
    activeFilter = cat;
    document.querySelectorAll('.filter-btn').forEach(b => b.classList.remove('active'));
    btn.classList.add('active');
    applyFilters();
  }

  function filterCards() { applyFilters(); }

  function applyFilters() {
    const q = document.getElementById('searchInput').value.toLowerCase().trim();
    const cards = document.querySelectorAll('.card');
    let visible = 0;

    cards.forEach(card => {
      const cat = card.dataset.cat;
      const kw  = (card.dataset.keywords + ' ' + card.querySelector('.card-title').textContent + ' ' + card.querySelector('.card-desc').textContent).toLowerCase();

      const catMatch  = (activeFilter === 'all') || (cat === activeFilter);
      const termMatch = !q || kw.includes(q);

      if (catMatch && termMatch) {
        card.classList.remove('hidden');
        visible++;
      } else {
        card.classList.add('hidden');
      }
    });

    const rc = document.getElementById('resultsCount');
    rc.textContent = q || activeFilter !== 'all' ? `Showing ${visible} of 19 techniques` : '';
  }

  function toggleDetail(btn) {
    const detail = btn.parentElement.querySelector('.card-detail');
    const isOpen = detail.classList.toggle('open');
    btn.classList.toggle('open', isOpen);
    btn.querySelector('.arrow').textContent = isOpen ? '▴' : '▾';
  }

  // Stagger card animations on load
  document.addEventListener('DOMContentLoaded', () => {
    document.querySelectorAll('.card').forEach((card, i) => {
      card.style.animationDelay = `${0.05 * i}s`;
    });
  });
</script>
</body>
</html>
