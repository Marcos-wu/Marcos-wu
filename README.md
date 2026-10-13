<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Marcos · 想法与创造并行</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@400;500;700&family=Noto+Sans+SC:wght@400;700;900&display=swap" rel="stylesheet">
<style>
  :root{
    --bg:#07060b;
    --magenta:#ff1f8f;
    --cyan:#12e8ff;
    --acid:#ccff00;
    --violet:#7b2bff;
    --txt:#f5f2fb;
    --muted:#a89fc0;
  }

  *{box-sizing:border-box;}
  html,body{margin:0;padding:0;}

  body{
    background:var(--bg);
    color:var(--txt);
    font-family:'Space Grotesk','Noto Sans SC',system-ui,-apple-system,'PingFang SC','Microsoft YaHei',sans-serif;
    min-height:100vh;
    overflow-x:hidden;
    position:relative;
    -webkit-font-smoothing:antialiased;
  }

  /* ===== 背景：网格 + 光斑 + 噪点 ===== */
  body::before{
    content:"";
    position:fixed; inset:0; z-index:0; pointer-events:none;
    background-image:
      linear-gradient(rgba(255,255,255,.035) 1px, transparent 1px),
      linear-gradient(90deg, rgba(255,255,255,.035) 1px, transparent 1px);
    background-size:64px 64px;
    -webkit-mask-image:radial-gradient(circle at 50% 25%, #000 0%, transparent 78%);
    mask-image:radial-gradient(circle at 50% 25%, #000 0%, transparent 78%);
  }

  .grain{
    position:fixed; inset:0; z-index:99; pointer-events:none; opacity:.055;
    background-image:url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='220' height='220'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.85' numOctaves='3'/%3E%3C/filter%3E%3Crect width='220' height='220' filter='url(%23n)'/%3E%3C/svg%3E");
  }

  .glow{
    position:fixed; border-radius:50%;
    filter:blur(100px); pointer-events:none; z-index:0;
  }
  .glow-a{width:540px;height:540px;background:var(--magenta);opacity:.40;top:-190px;left:-170px;animation:float1 15s ease-in-out infinite;}
  .glow-b{width:480px;height:480px;background:var(--violet);opacity:.42;bottom:-170px;right:-140px;animation:float2 18s ease-in-out infinite;}
  .glow-c{width:360px;height:360px;background:var(--cyan);opacity:.22;top:42%;left:58%;animation:float3 20s ease-in-out infinite;}

  @keyframes float1{0%,100%{transform:translate(0,0) scale(1)}50%{transform:translate(70px,50px) scale(1.12)}}
  @keyframes float2{0%,100%{transform:translate(0,0) scale(1)}50%{transform:translate(-80px,-55px) scale(1.15)}}
  @keyframes float3{0%,100%{transform:translate(0,0)}50%{transform:translate(-50px,70px)}}

  /* ===== 布局 ===== */
  .wrap{
    position:relative; z-index:1;
    max-width:1080px;
    margin:0 auto;
    padding:clamp(3rem,9vh,6.5rem) 1.5rem 3rem;
  }

  @keyframes rise{from{opacity:0;transform:translateY(26px)}to{opacity:1;transform:none}}

  /* ===== 打招呼 ===== */
  .hero{text-align:center;}

  .badge{
    display:inline-block;
    font-size:.72rem; font-weight:700;
    letter-spacing:.38em; text-transform:uppercase;
    padding:.55em 1.2em .55em 1.4em;
    border-radius:999px;
    color:#ffb3da;
    border:1px solid rgba(255,31,143,.55);
    background:rgba(255,31,143,.09);
    box-shadow:0 0 26px rgba(255,31,143,.3), inset 0 0 26px rgba(255,31,143,.1);
    margin-bottom:1.7rem;
    animation:rise .9s cubic-bezier(.2,.8,.2,1) both;
  }

  .hero h1{
    margin:0 0 .55em;
    font-size:clamp(2.5rem,8.5vw,5.4rem);
    line-height:1.03;
    font-weight:700;
    letter-spacing:-.02em;
    color:#fff;
    text-shadow:0 0 20px rgba(255,31,143,.5), 0 0 60px rgba(123,43,255,.45);
    animation:rise .9s cubic-bezier(.2,.8,.2,1) .08s both;
  }

  .name{
    background:linear-gradient(92deg,var(--magenta),var(--violet) 45%,var(--cyan));
    background-size:220% 100%;
    -webkit-background-clip:text; background-clip:text;
    color:transparent;
    animation:sheen 6s linear infinite;
  }
  @keyframes sheen{to{background-position:220% 0}}

  .wave{
    display:inline-block;
    animation:wave 2.4s ease-in-out infinite;
    transform-origin:70% 70%;
  }
  @keyframes wave{
    0%,60%,100%{transform:rotate(0)}
    70%{transform:rotate(16deg)}
    80%{transform:rotate(-10deg)}
    90%{transform:rotate(12deg)}
  }

  .sub{
    margin:.4em 0;
    font-size:clamp(.95rem,2.2vw,1.22rem);
    color:#d8d2e6;
    letter-spacing:.015em;
    animation:rise .9s cubic-bezier(.2,.8,.2,1) both;
  }
  .sub:nth-of-type(1){animation-delay:.16s}
  .sub:nth-of-type(2){animation-delay:.24s}
  .sub.accent{
    color:var(--cyan);
    text-shadow:0 0 24px rgba(18,232,255,.45);
    animation-delay:.32s;
  }

  /* ===== 跑马灯 ===== */
  .marquee{
    margin:clamp(2.6rem,7vh,4.5rem) 0;
    overflow:hidden;
    border-top:1px solid rgba(255,255,255,.12);
    border-bottom:1px solid rgba(255,255,255,.12);
    padding:.9rem 0;
    -webkit-mask-image:linear-gradient(90deg,transparent,#000 12%,#000 88%,transparent);
    mask-image:linear-gradient(90deg,transparent,#000 12%,#000 88%,transparent);
  }
  .marquee-track{
    display:flex; width:max-content;
    animation:scroll 26s linear infinite;
    font-size:.82rem; font-weight:700;
    letter-spacing:.28em; text-transform:uppercase;
    white-space:nowrap;
  }
  .marquee-track span{flex:0 0 auto;margin:0 1.1rem;color:rgba(255,255,255,.55);}
  .marquee-track b{flex:0 0 auto;margin:0 1.1rem;color:var(--magenta);text-shadow:0 0 16px rgba(255,31,143,.8);}
  @keyframes scroll{from{transform:translateX(0)}to{transform:translateX(-50%)}}

  /* ===== 想法 × 创造 ===== */
  .duo{text-align:center;}

  .duo-title{
    margin:0;
    font-size:clamp(1.9rem,6.2vw,3.9rem);
    font-weight:900;
    letter-spacing:.02em;
    line-height:1.2;
    animation:rise .9s cubic-bezier(.2,.8,.2,1) both;
  }
  .duo-title em{
    font-style:normal;
    color:var(--magenta);
    text-shadow:0 0 26px rgba(255,31,143,.75);
  }
  .duo-title b{
    color:transparent;
    -webkit-text-stroke:2px var(--acid);
    text-shadow:0 0 30px rgba(204,255,0,.5);
  }

  .duo-en{
    margin:.9rem 0 clamp(2.2rem,6vh,3.4rem);
    font-size:.78rem;
    font-weight:500;
    letter-spacing:.42em;
    text-transform:uppercase;
    color:var(--muted);
    animation:rise .9s cubic-bezier(.2,.8,.2,1) .1s both;
  }

  .grid{
    display:grid;
    grid-template-columns:1fr auto 1fr;
    gap:clamp(1rem,3vw,2.4rem);
    align-items:center;
    animation:rise .9s cubic-bezier(.2,.8,.2,1) .2s both;
  }

  .times{
    font-size:clamp(1.9rem,5vw,3.2rem);
    font-weight:700;
    color:var(--acid);
    text-shadow:0 0 30px rgba(204,255,0,.7);
    animation:pulse 3s ease-in-out infinite;
  }
  @keyframes pulse{0%,100%{opacity:1;transform:scale(1)}50%{opacity:.6;transform:scale(.9)}}

  .panel{
    position:relative;
    text-align:left;
    padding:clamp(1.5rem,3.4vw,2.6rem);
    border-radius:22px;
    border:1px solid rgba(255,255,255,.14);
    background:linear-gradient(160deg, rgba(255,255,255,.07), rgba(255,255,255,.015));
    backdrop-filter:blur(10px);
    -webkit-backdrop-filter:blur(10px);
    overflow:hidden;
    transition:transform .4s cubic-bezier(.2,.8,.2,1), box-shadow .4s;
  }
  .panel::after{
    content:"";
    position:absolute; inset:auto -30% -60% -30%;
    height:70%;
    border-radius:50%;
    filter:blur(50px);
    opacity:.55;
    pointer-events:none;
  }
  .panel-idea{box-shadow:0 0 70px -25px rgba(18,232,255,.9), inset 0 0 70px -50px rgba(18,232,255,1);}
  .panel-idea::after{background:var(--cyan);opacity:.28;}
  .panel-make{box-shadow:0 0 70px -25px rgba(255,31,143,.9), inset 0 0 70px -50px rgba(255,31,143,1);}
  .panel-make::after{background:var(--magenta);opacity:.3;}

  .panel:hover{transform:translateY(-7px) scale(1.018);}

  .panel .num{
    display:block;
    font-size:.72rem;
    font-weight:700;
    letter-spacing:.4em;
    color:rgba(255,255,255,.45);
  }
  .panel h3{
    margin:.35em 0 .12em;
    font-size:clamp(1.7rem,4.4vw,2.6rem);
    font-weight:900;
    letter-spacing:.04em;
  }
  .panel .en{
    display:block;
    font-size:.72rem;
    font-weight:700;
    letter-spacing:.44em;
    text-transform:uppercase;
  }
  .panel-idea .en{color:var(--cyan);text-shadow:0 0 18px rgba(18,232,255,.8);}
  .panel-make .en{color:var(--magenta);text-shadow:0 0 18px rgba(255,31,143,.8);}

  .panel p{
    position:relative; z-index:1;
    margin:1.3em 0 0;
    font-size:clamp(.9rem,2vw,1rem);
    line-height:1.8;
    color:#cdc6dc;
  }

  /* ===== 格言 ===== */
  .quotes{
    margin-top:clamp(4rem,11vh,7rem);
    text-align:center;
  }
  .quotes p{
    margin:.55em 0;
    font-size:clamp(.95rem,2.3vw,1.15rem);
    font-style:italic;
    color:#e6e0f4;
    text-shadow:0 0 26px rgba(123,43,255,.55);
  }
  .quotes p:last-child{
    color:var(--acid);
    text-shadow:0 0 26px rgba(204,255,0,.5);
  }

  /* ===== 响应式 ===== */
  @media (max-width:760px){
    .grid{grid-template-columns:1fr;gap:.9rem;}
    .times{justify-self:center;}
    .duo-title b{-webkit-text-stroke-width:1.5px;}
  }

  @media (prefers-reduced-motion:reduce){
    *{animation-duration:.01ms !important;animation-iteration-count:1 !important;transition:none !important;}
  }
</style>
</head>
<body>

  <div class="grain"></div>
  <div class="glow glow-a"></div>
  <div class="glow glow-b"></div>
  <div class="glow glow-c"></div>

  <main class="wrap">

    <!-- ===== 打招呼 ===== -->
    <header class="hero">
      <span class="badge">Hello · World</span>
      <h1>Hi, I'm <span class="name">Marcos</span> <span class="wave">👋</span></h1>
      <p class="sub">A Gen Z Coder Born in 2006 💻</p>
      <p class="sub">Bridging LLMs with Modern Web Tech 🚀</p>
      <p class="sub accent">AI 应用开发者 in training · Building with FastAPI + RAG + Agents 🎯</p>
    </header>

    <!-- ===== 跑马灯 ===== -->
    <div class="marquee" aria-hidden="true">
      <div class="marquee-track">
        <span>想法</span><b>×</b><span>创造</span><b>×</b><span>并行</span><b>×</b>
        <span>Ideas</span><b>×</b><span>Creation</span><b>×</b><span>Parallel</span><b>×</b>
        <span>想法</span><b>×</b><span>创造</span><b>×</b><span>并行</span><b>×</b>
        <span>Ideas</span><b>×</b><span>Creation</span><b>×</b><span>Parallel</span><b>×</b>
      </div>
    </div>

    <!-- ===== 想法与创造并行 ===== -->
    <section class="duo">
      <h2 class="duo-title">想法 <em>与</em> 创造 <b>并行</b></h2>
      <p class="duo-en">Ideas × Creation · Running in Parallel</p>

      <div class="grid">
        <article class="panel panel-idea">
          <span class="num">01</span>
          <h3>想法</h3>
          <span class="en">Idea</span>
          <p>先想到，再让世界看见。</p>
        </article>

        <div class="times">×</div>

        <article class="panel panel-make">
          <span class="num">02</span>
          <h3>创造</h3>
          <span class="en">Create</span>
          <p>先做出来，再想更多。</p>
        </article>
      </div>
    </section>

    <!-- ===== 格言 ===== -->
    <footer class="quotes">
      <p><i>"Share code, share ideas."</i></p>
      <p><i>"Progress over perfection."</i></p>
    </footer>

  </main>

</body>
</html>
