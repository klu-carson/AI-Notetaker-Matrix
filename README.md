<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>AI Notetaker Competitor Matrix</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@400;500;600;700&family=Inter:wght@400;500;600&family=JetBrains+Mono:wght@400;500&display=swap" rel="stylesheet">
<style>
  :root{
    --paper:#FBFAF7;
    --panel:#FFFFFF;
    --ink:#17171F;
    --ink-soft:#3A3A45;
    --muted:#65656F;
    --line:#E7E4DC;
    --line-soft:#F0EDE6;
    --accent:#34378C;
    --free:#15805A;     /* bot-free / low friction */
    --bot:#B5611F;      /* visible bot joins */
    --hidden:#8A3FAE;   /* hidden overlay / contentious */
    --optional:#2A6F97; /* bot optional — works with or without */
    --display:'Space Grotesk', system-ui, sans-serif;
    --body:'Inter', system-ui, -apple-system, sans-serif;
    --mono:'JetBrains Mono', ui-monospace, SFMono-Regular, Menlo, monospace;
  }
  *{box-sizing:border-box}
  html{-webkit-text-size-adjust:100%}
  body{
    margin:0;background:var(--paper);color:var(--ink);
    font-family:var(--body);font-size:15px;line-height:1.55;
    -webkit-font-smoothing:antialiased;
  }
  .wrap{max-width:1180px;margin:0 auto;padding:48px 28px 80px}

  /* ---------- header ---------- */
  .eyebrow{
    font-family:var(--mono);font-size:11.5px;letter-spacing:.16em;
    text-transform:uppercase;color:var(--muted);margin:0 0 14px;
  }
  h1{
    font-family:var(--display);font-weight:700;letter-spacing:-.02em;
    font-size:clamp(30px,5vw,48px);line-height:1.02;margin:0 0 16px;
  }
  h1 .em{color:var(--accent)}
  .lede{font-size:17px;color:var(--ink-soft);max-width:62ch;margin:0 0 22px}
  .note{
    font-family:var(--mono);font-size:12px;color:var(--muted);
    border-left:2px solid var(--line);padding:8px 0 8px 14px;max-width:70ch;
    line-height:1.5;
  }

  /* ---------- legend ---------- */
  .legend{
    display:flex;flex-wrap:wrap;gap:8px 18px;margin:30px 0 6px;
    padding-top:24px;border-top:1px solid var(--line);
  }
  .legend .lab{font-family:var(--mono);font-size:11px;letter-spacing:.1em;
    text-transform:uppercase;color:var(--muted);align-self:center;margin-right:2px}
  .chip{
    display:inline-flex;align-items:center;gap:7px;font-size:12.5px;
    font-weight:500;color:var(--ink-soft);
  }
  .dot{width:9px;height:9px;border-radius:50%;flex:none}
  .dot.free{background:var(--free)} .dot.bot{background:var(--bot)} .dot.hidden{background:var(--hidden)} .dot.optional{background:var(--optional)}

  /* ---------- section heads ---------- */
  .sec-head{display:flex;align-items:baseline;gap:14px;margin:54px 0 20px}
  .sec-head h2{
    font-family:var(--display);font-weight:600;font-size:22px;
    letter-spacing:-.01em;margin:0;
  }
  .sec-head .idx{font-family:var(--mono);font-size:12px;color:var(--accent)}

  /* ---------- overview grid ---------- */
  .grid{display:grid;grid-template-columns:repeat(4,1fr);gap:14px}
  .card{
    background:var(--panel);border:1px solid var(--line);border-radius:14px;
    padding:18px 18px 16px;display:flex;flex-direction:column;gap:10px;
    position:relative;overflow:hidden;
  }
  .card::before{content:"";position:absolute;left:0;top:0;bottom:0;width:3px;background:var(--rail,var(--line))}
  .card[data-bot="free"]{--rail:var(--free)}
  .card[data-bot="bot"]{--rail:var(--bot)}
  .card[data-bot="hidden"]{--rail:var(--hidden)}
  .card[data-bot="optional"]{--rail:var(--optional)}
  .card .name{font-family:var(--display);font-weight:600;font-size:18px;letter-spacing:-.01em}
  .card .tag{font-size:13px;color:var(--muted);line-height:1.45;min-height:38px}
  .badge{
    display:inline-flex;align-items:center;gap:6px;align-self:flex-start;
    font-family:var(--mono);font-size:10.5px;letter-spacing:.06em;
    text-transform:uppercase;padding:4px 8px;border-radius:6px;font-weight:500;
  }
  .badge.free{color:var(--free);background:rgba(21,128,90,.09)}
  .badge.bot{color:var(--bot);background:rgba(181,97,31,.10)}
  .badge.hidden{color:var(--hidden);background:rgba(138,63,174,.10)}
  .badge.optional{color:var(--optional);background:rgba(42,111,151,.11)}
  .card .meta{display:flex;flex-direction:column;gap:6px;margin-top:auto;padding-top:10px;border-top:1px solid var(--line-soft)}
  .card .row{display:flex;gap:8px;font-size:12.5px;line-height:1.4}
  .card .row .k{font-family:var(--mono);font-size:10.5px;letter-spacing:.05em;text-transform:uppercase;color:var(--muted);flex:none;width:54px;padding-top:1px}
  .card .row .v{color:var(--ink-soft)}
  .card .row .v.price{font-family:var(--mono);font-size:12px}

  /* ---------- dimension tabs ---------- */
  .tabs{display:flex;flex-wrap:wrap;gap:7px;margin:20px 0 0}
  .tab{
    font-family:var(--body);font-size:13px;font-weight:500;color:var(--ink-soft);
    background:var(--panel);border:1px solid var(--line);border-radius:999px;
    padding:8px 15px;cursor:pointer;transition:background .14s,color .14s,border-color .14s;
  }
  .tab:hover{border-color:var(--accent)}
  .tab[aria-selected="true"]{background:var(--ink);color:#fff;border-color:var(--ink)}
  .tab:focus-visible{outline:2px solid var(--accent);outline-offset:2px}

  .panel{margin-top:20px;background:var(--panel);border:1px solid var(--line);border-radius:16px;overflow:hidden}
  .panel-intro{padding:18px 22px;border-bottom:1px solid var(--line-soft);font-size:14px;color:var(--ink-soft);background:linear-gradient(0deg,var(--paper),var(--panel))}
  .panel-intro b{color:var(--ink);font-weight:600}
  .matrix-row{display:grid;grid-template-columns:200px 1fr;gap:0;border-bottom:1px solid var(--line-soft)}
  .matrix-row:last-child{border-bottom:none}
  .matrix-row .tool{
    padding:18px 20px;display:flex;flex-direction:column;gap:8px;
    border-right:1px solid var(--line-soft);background:var(--paper)
  }
  .matrix-row .tool .nm{font-family:var(--display);font-weight:600;font-size:15.5px}
  .matrix-row .tool .badge{align-self:flex-start}
  .matrix-row .body{padding:18px 22px;font-size:14px;color:var(--ink-soft);line-height:1.6}

  /* ---------- closing analysis ---------- */
  .read{margin-top:18px;display:grid;grid-template-columns:1fr 1fr;gap:14px}
  .read .block{background:var(--panel);border:1px solid var(--line);border-radius:14px;padding:20px 22px}
  .read h3{font-family:var(--display);font-size:16px;font-weight:600;margin:0 0 10px;letter-spacing:-.01em}
  .read p{margin:0 0 10px;font-size:13.5px;color:var(--ink-soft);line-height:1.6}
  .read p:last-child{margin-bottom:0}
  .read strong{color:var(--ink);font-weight:600}
  .full{grid-column:1/-1}

  footer{margin-top:46px;padding-top:22px;border-top:1px solid var(--line);
    font-family:var(--mono);font-size:11.5px;color:var(--muted);line-height:1.6}

  @media (max-width:860px){
    .grid{grid-template-columns:repeat(2,1fr)}
    .read{grid-template-columns:1fr}
  }
  @media (max-width:560px){
    .wrap{padding:34px 18px 64px}
    .grid{grid-template-columns:1fr}
    .matrix-row{grid-template-columns:1fr}
    .matrix-row .tool{border-right:none;border-bottom:1px solid var(--line-soft)}
  }
  @media (prefers-reduced-motion:reduce){*{transition:none!important}}
</style>
</head>
<body>
<div class="wrap">

  <p class="eyebrow">Competitive analysis · AI meeting notetakers</p>
  <h1>The notetaker field, <span class="em">side by side</span></h1>
  <p class="lede">Eight tools compared across the seven things that actually decide adoption — what they do, where they break, who they're for, what they cost, how entrenched they are, how much they disrupt a meeting, and how they feel to use.</p>
  <p class="note">Compiled from product knowledge current to early 2026. Pricing and free-tier limits on these tools change often — treat every price below as "verify before quoting," especially against the source pages you linked. The browser connector wasn't responding, so live figures couldn't be pulled this pass.</p>

  <div class="legend">
    <span class="lab">Workflow footprint</span>
    <span class="chip"><span class="dot free"></span>Bot-free — captures audio locally, nothing joins the call</span>
    <span class="chip"><span class="dot bot"></span>Bot joins — a visible participant records the meeting</span>
    <span class="chip"><span class="dot optional"></span>Bot optional — works either way: visible bot or local capture</span>
    <span class="chip"><span class="dot hidden"></span>Hidden overlay — runs on your screen, unseen by others</span>
  </div>

  <!-- OVERVIEW -->
  <div class="sec-head"><span class="idx">01</span><h2>At a glance</h2></div>
  <div class="grid" id="overview"></div>

  <!-- DIMENSION MATRIX -->
  <div class="sec-head"><span class="idx">02</span><h2>The full matrix</h2></div>
  <div class="tabs" id="tabs" role="tablist" aria-label="Comparison dimensions"></div>
  <div class="panel">
    <div class="panel-intro" id="panelIntro"></div>
    <div id="panelBody"></div>
  </div>

  <!-- READING THE FIELD -->
  <div class="sec-head"><span class="idx">03</span><h2>Reading the field</h2></div>
  <div class="read">
    <div class="block">
      <h3>The one axis that splits the market</h3>
      <p><strong>Bot vs. bot-free is the real fault line</strong>, more than features or price. Otter and Fathom send a visible bot into the call, while Fireflies can do either — bot or local capture. Powerful and integration-rich, but when a bot is used every participant sees it and someone has to be comfortable with that.</p>
      <p>Jamie, Granola, Notion, and Gemini capture audio locally or natively, so the meeting looks normal. That single difference drives most of the "how invasive is it" answer.</p>
    </div>
    <div class="block">
      <h3>Cluely is a different animal</h3>
      <p>It's listed because people group it with notetakers, but it's a <strong>real-time, invisible copilot</strong> — built to feed you live answers during a call without others knowing. That makes it the most interpersonally invasive option and raises clear consent questions.</p>
      <p>As a durable notes archive it's weak; as a live assist for sales objection-handling it's novel. Judge it on that, not as a Granola substitute.</p>
    </div>
    <div class="block">
      <h3>"Industry standard" depends on the room</h3>
      <p><strong>Otter and Fireflies</strong> are the mature, widely-deployed defaults — especially in sales and enterprise. <strong>Fathom</strong> wins on review-site love and a famously generous free tier.</p>
      <p><strong>Gemini and Notion AI</strong> are standards by distribution — you likely already pay for them — but only if your org lives in Google Workspace or Notion respectively.</p>
    </div>
    <div class="block">
      <h3>Where Granola fits</h3>
      <p>Granola's pitch is narrower and sharper: you keep taking your own notes, it cleans them up with the transcript. It's the <strong>operator/founder favourite</strong> and the standout on pure UX, with zero footprint in the meeting.</p>
      <p>The trade-off is a lighter integration ecosystem and no conversation analytics — so it shines for individuals and small teams more than for RevOps reporting.</p>
    </div>
    <div class="block full">
      <h3>If you're choosing for a team</h3>
      <p>Roughly: pick <strong>Fathom</strong> or <strong>Otter</strong> for a low-cost, low-friction default; <strong>Fireflies</strong> when you need CRM sync and call analytics across a sales org; <strong>Granola</strong> or <strong>Jamie</strong> when meetings can't have a bot and individuals want clean personal notes (Jamie if EU/GDPR posture matters); and <strong>Gemini</strong> or <strong>Notion AI</strong> when the org is already all-in on that ecosystem and "good enough, zero setup" beats "best in class." The decisive questions are: is a bot acceptable on the call, do you need analytics/CRM or just clean notes, and which ecosystem do you already pay for.</p>
    </div>
  </div>

  <footer>
    8 tools · 7 dimensions · knowledge current to early 2026 · prices approximate, verify against source pages before citing.
  </footer>
</div>

<script>
const TOOLS = [
  {
    name:"Jamie", bot:"free", botLabel:"Bot-free",
    tag:"Privacy-first, EU-based. Records local audio on any platform — no bot.",
    start:"Free / ~€24/mo", bestFor:"No-bot, GDPR-bound work",
    features:"Captures system audio so it works on Zoom, Teams, Meet and in-person without joining the call. AI summaries, action items and transcripts; \"Ask Jamie\" chat over your notes; 20+ languages; custom vocabulary and templates.",
    limitations:"Desktop app required (Mac/Windows); no video recording; thinner third-party integration set than Fireflies or Otter; processes after the meeting rather than a rich live view; priced in EUR.",
    useCases:"Consultants, lawyers, and EU/GDPR-bound teams; anyone who can't have a bot on the call; cross-platform and in-person meetings where discretion matters.",
    pricing:"Free tier with limited meetings/month. Standard ~€24/mo, Pro ~€47/mo, Executive ~€99/mo, with annual discounts.",
    market:"Rising challenger with a strong foothold in the EU/privacy niche. Well-regarded but not yet a mass-market default.",
    friction:"Very low. No bot means meetings look completely normal; the only habit change is opening the desktop app and hitting record. Non-invasive to other participants.",
    uiux:"Clean, minimal, privacy-forward desktop interface with an uncluttered notes view. Functional rather than flashy."
  },
  {
    name:"Granola", bot:"free", botLabel:"Bot-free",
    tag:"You take notes, it augments them with the transcript. Operator favourite.",
    start:"Free trial / ~$18/mo", bestFor:"Personal clean notes, founders",
    features:"Blends your own typed notes with the meeting transcript into polished output. Custom templates per meeting type, ask-questions across all past meetings, folders and sharing. Works on any platform via local capture — no bot.",
    limitations:"Mac-first (Windows is newer and less mature); local-capture only, so you must join from that device; no video; lighter integration ecosystem; no conversation analytics or talk-time metrics.",
    useCases:"Founders, PMs, consultants and execs who already take notes and want them cleaned up. Strategy calls, 1:1s, and personal knowledge capture.",
    pricing:"Free trial (limited meetings). Individual ~$18/mo; Business ~$14/user/mo billed annually; Enterprise custom.",
    market:"Fast-growing favourite in the startup / VC / operator crowd, with near-cult product status. Not yet an enterprise standard.",
    friction:"Low, but a genuine behaviour shift — you still write notes yourself and it augments them. Bot-free, so it's invisible to others. Mac-centric habit.",
    uiux:"Widely cited as the best UX in the category: an elegant, minimalist notepad that stays out of your way. Fast and distraction-free."
  },
  {
    name:"Fireflies", bot:"optional", botLabel:"Bot optional",
    tag:'"Fred" can join the call or capture without a bot. Built for sales and large orgs.',
    start:"Free / ~$10 seat/mo", bestFor:"Sales, RevOps, big orgs",
    features:"Records via a bot that joins the call or, optionally, captures audio without one (browser extension / mobile / upload). Transcribes and summarises; \"AskFred\" assistant; conversation intelligence (talk-time, sentiment, topic trackers); CRM, Slack and dialer integrations; searchable knowledge base; soundbites; 60+ languages; AI workflows.",
    limitations:"Bot-free capture is more limited than the full bot flow; the visible bot still carries optics/consent friction when used; can feel feature-heavy; accuracy varies with audio quality; the most useful features sit on higher tiers; storage limits on the free plan.",
    useCases:"Sales and RevOps, customer success, recruiting, and large distributed orgs that need a searchable call library wired into the CRM.",
    pricing:"Free tier. Pro ~$10/seat/mo annual (~$18 monthly); Business ~$19/seat/mo; Enterprise ~$39/seat/mo.",
    market:"One of the most established players and a common enterprise/sales standard, with a large install base.",
    friction:"Variable. With the bot it visibly joins (participants notice and must consent); the bot-free capture mode avoids that. Either way there's calendar auto-join and integration setup, plus a rich UI to learn.",
    uiux:"Dense, feature-rich dashboard — powerful but can feel busy and enterprise-y. Lots of surface area to navigate."
  },
  {
    name:"Otter", bot:"bot", botLabel:"Bot joins",
    tag:"The original transcription name. Real-time captions and OtterPilot.",
    start:"Free 300 min / ~$8+/mo", bestFor:"Live captions, accessibility",
    features:"Real-time live transcription with speaker ID; OtterPilot auto-joins calls; live summaries and action items; Otter AI Chat; automatic slide capture; integrations with Zoom, Meet, Teams, Salesforce, HubSpot and Slack; newer AI agents.",
    limitations:"Bot joins; struggles with heavy accents and cross-talk; summaries less polished than newer rivals; strongest in English; free-tier minutes are tight.",
    useCases:"Live captioning and accessibility, education, journalism, and general business meetings where a real-time transcript is the point.",
    pricing:"Free (300 min/mo, 30 min/conversation). Pro ~$8.33/mo annual (~$16.99 monthly); Business ~$20/user/mo; Enterprise custom.",
    market:"The original category leader and a household name in transcription; very widely adopted, especially in the US. Mature.",
    friction:"Low-to-moderate. OtterPilot joins visibly and the live transcript can distract, but the product is intuitive. Moderate invasiveness.",
    uiux:"Familiar and functional, centred on the live transcript. Solid mobile app, though some find the desktop experience dated next to newer entrants."
  },
  {
    name:"Fathom", bot:"bot", botLabel:"Bot joins",
    tag:"Famously generous free tier, fast clean recaps, slick highlight clips.",
    start:"Free (generous) / ~$15/mo", bestFor:"SMB sales/CS, free users",
    features:"Bot records Zoom, Meet and Teams; fast AI summaries and action items; highlight and clip moments; \"Ask Fathom\"; CRM sync (HubSpot, Salesforce, Close); shared team call libraries; multi-language summaries.",
    limitations:"Bot joins; fewer deep analytics than Fireflies; historically English-centric (expanding); less control over output format; lighter enterprise governance on lower tiers.",
    useCases:"SMB sales and customer-success teams and individuals; founders wanting a free, no-fuss recorder; quick post-call recaps and shareable clips.",
    pricing:"Free plan is unusually generous (unlimited recording for individuals). Premium ~$15/mo; Team Edition ~$19/user/mo; Team Pro ~$29/user/mo.",
    market:"Very popular and consistently top-rated on review sites like G2, with strong SMB momentum driven by the free tier.",
    friction:"Low. The bot joins, but the flow is smooth and repeatedly praised for simplicity — close to zero learning curve. Bot is still visible.",
    uiux:"Clean, modern and friendly; the highlights-and-clips experience is a standout. Consistently rated as easy and pleasant to use."
  },
  {
    name:"Cluely", bot:"hidden", botLabel:"Hidden overlay",
    tag:"Real-time invisible copilot that feeds you live answers mid-call.",
    start:"Free / ~$20/mo", bestFor:"Live in-call assistance",
    features:"An invisible desktop overlay listens to your audio and screen and supplies live AI suggestions and answers during calls. Can produce meeting notes, but the core is in-the-moment assistance — it runs on your machine and stays hidden from screen-share.",
    limitations:"Ethically and consent-contentious by design (\"undetectable\"); not a structured knowledge base; live answers carry hallucination risk; real privacy concerns; weak as a long-term notes archive.",
    useCases:"Live sales objection-handling, real-time coaching, support calls, and (contested) interview prep — situations that want answers in the moment.",
    pricing:"Free tier; Pro ~$20/mo; Enterprise custom.",
    market:"New, buzzy and deliberately controversial, with viral marketing. Not an enterprise standard and reputationally polarising.",
    friction:"Low to operate, but high interpersonal invasiveness — it's hidden from everyone else on the call, which raises clear consent and ethics issues. Not transparent.",
    uiux:"Sleek, minimal \"invisible\" overlay designed to stay unobtrusive on screen. A novel interaction model rather than a notes app."
  },
  {
    name:"Notion AI", bot:"free", botLabel:"Bot-free",
    tag:"Meeting notes that live inside your Notion workspace and docs.",
    start:"Bundled ~$20–30/user", bestFor:"Notion-centric teams",
    features:"Records and transcribes meetings inside Notion, with summaries that drop straight into your pages and databases. Workspace-wide AI Q&A across docs and wikis; templates; notes sit alongside your projects.",
    limitations:"Only valuable if you're already on Notion; the notetaking is newer and less specialised; few meeting-specific integrations; no conversation intelligence; capture quality trails dedicated tools.",
    useCases:"Teams standardising on Notion that want meeting notes feeding directly into project docs, wikis and databases.",
    pricing:"AI is bundled into Notion Business (~$20–30/user/mo depending on billing); Enterprise custom; limited on free and Plus tiers.",
    market:"A standard within Notion-centric orgs, but not a standalone meeting-tool leader.",
    friction:"Low if you already live in Notion; otherwise high, since it means adopting Notion itself. Bot-free, local capture — non-invasive in the meeting.",
    uiux:"Inherits Notion's clean, block-based design. Great if you like Notion, but the meeting view is secondary to the broader workspace."
  },
  {
    name:"Google Gemini", bot:"free", botLabel:"Bot-free",
    tag:'"Take notes for me" — native Meet summaries, zero setup, Workspace-wide.',
    start:"Bundled in Workspace", bestFor:"Google Meet / Workspace orgs",
    features:"\"Take notes for me\" auto-generates Meet summaries and action items into Google Docs; live captions and translation; the Gemini assistant works across Gmail, Docs, Drive and Sheets; recordings saved to Drive.",
    limitations:"Google Meet only — no Zoom or Teams; requires Workspace plus a Gemini entitlement; notes can read generic; limited templating; tied to the Google ecosystem.",
    useCases:"Organisations standardised on Google Workspace and Meet that want zero-setup, native notes without another vendor.",
    pricing:"Bundled into Google Workspace Business Standard (~$14/user/mo) and higher tiers; standalone Gemini add-ons exist for some plans.",
    market:"A default by distribution across Google Workspace's enormous base — increasingly a de facto choice simply because it's built in.",
    friction:"Lowest setup friction for Workspace users — a native toggle in Meet, no bot, nothing to install. The catch is it's locked to Meet. Non-invasive.",
    uiux:"Native Google look — tidy and integrated, with notes landing in Docs and no separate app to learn. Minimal but generic."
  }
];

const DIMS = [
  {key:"features",    label:"Features",          intro:"What each tool actually <b>does</b> — capture method, AI output, and standout capabilities."},
  {key:"limitations", label:"Limitations",        intro:"Where each one <b>breaks down</b> or hits a ceiling."},
  {key:"useCases",    label:"Use cases",          intro:"Who each tool is <b>built for</b> and the jobs it does best."},
  {key:"pricing",     label:"Pricing",            intro:"Approximate tiers — <b>verify before quoting</b>, these move often."},
  {key:"market",      label:"Market position",    intro:'How established each is — the "<b>industry standard</b>" question.'},
  {key:"friction",    label:"Workflow friction",  intro:"The <b>learning curve and invasiveness</b> — how much it disrupts a meeting."},
  {key:"uiux",        label:"UI / UX",            intro:"How each tool <b>looks and feels</b> to use day to day."}
];

// ---- render overview ----
const ov = document.getElementById('overview');
ov.innerHTML = TOOLS.map(t => `
  <div class="card" data-bot="${t.bot}">
    <span class="badge ${t.bot}"><span class="dot ${t.bot}"></span>${t.botLabel}</span>
    <div class="name">${t.name}</div>
    <div class="tag">${t.tag}</div>
    <div class="meta">
      <div class="row"><span class="k">Start</span><span class="v price">${t.start}</span></div>
      <div class="row"><span class="k">Best for</span><span class="v">${t.bestFor}</span></div>
    </div>
  </div>`).join('');

// ---- render tabs ----
const tabsEl = document.getElementById('tabs');
const introEl = document.getElementById('panelIntro');
const bodyEl  = document.getElementById('panelBody');

DIMS.forEach((d,i) => {
  const b = document.createElement('button');
  b.className='tab'; b.textContent=d.label; b.role='tab';
  b.id='tab-'+d.key; b.setAttribute('aria-selected', i===0?'true':'false');
  b.addEventListener('click', ()=>select(d.key));
  b.addEventListener('keydown', e=>{
    const order=DIMS.map(x=>x.key); let idx=order.indexOf(d.key);
    if(e.key==='ArrowRight'){idx=(idx+1)%order.length;e.preventDefault();select(order[idx]);document.getElementById('tab-'+order[idx]).focus();}
    if(e.key==='ArrowLeft'){idx=(idx-1+order.length)%order.length;e.preventDefault();select(order[idx]);document.getElementById('tab-'+order[idx]).focus();}
  });
  tabsEl.appendChild(b);
});

function select(key){
  const dim = DIMS.find(d=>d.key===key);
  document.querySelectorAll('.tab').forEach(t=>t.setAttribute('aria-selected', t.id==='tab-'+key?'true':'false'));
  introEl.innerHTML = dim.intro;
  bodyEl.innerHTML = TOOLS.map(t=>`
    <div class="matrix-row">
      <div class="tool">
        <span class="nm">${t.name}</span>
        <span class="badge ${t.bot}"><span class="dot ${t.bot}"></span>${t.botLabel}</span>
      </div>
      <div class="body">${t[key]}</div>
    </div>`).join('');
}
select('features');
</script>
</body>
</html>
