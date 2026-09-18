---
theme: default
title: Automate your software delivery with AI Factories
info: |
  ## Automate your software delivery with AI Factories
  How a delivery pipeline where every stage is an agent actually works, what
  holds it together, and how to build one on your own machine.

  Speaker: Dani Akash, Founding Engineer at BrowserOS.
fonts:
  sans: Inter
  mono: IBM Plex Mono
  weights: '200,400,600,700'
css: unocss
colorSchema: dark
canvasWidth: 1920
aspectRatio: '16/9'
drawings:
  persist: false
mdc: true
transition: fade
layout: default
class: 'dark'
htmlAttrs:
  lang: en
---

<style>
@import './styles/index.css';
</style>

<div class="slide-shell">
  <ParticleField variant="cloud" :opacity="0.9" />
  <div class="chrome"><div>Dani Akash</div><ChromeCounter /></div>
  <div class="frame">
    <div style="max-width:1180px">
      <div class="kicker">AI FACTORIES</div>
      <h1 class="t-lg">Automate your<br/>software delivery<br/>with <span class="accent">AI factories.</span></h1>
      <p class="body" style="margin-top:54px">A delivery pipeline where every stage is an agent. One triages, one writes the spec, one implements, one reviews. A human still owns the merge.</p>
      <div class="meta-row" style="margin-top:54px"><span>The pattern</span><span>The anatomy</span><span>Build one yourself</span></div>
    </div>
  </div>
  <div class="foot"><div class="title">AI Factories</div><div>Opening</div></div>
</div>


---
class: 'dark'
---

<div class="slide-shell">
  <div class="chrome"><div>Speaker</div><ChromeCounter /></div>
  <div class="frame">
    <div class="split">
      <div>
        <div class="kicker">INTRODUCTIONS</div>
        <h2 class="t-md">Hi, I'm Dani.</h2>
        <p class="body" style="margin-top:36px">Founding Engineer at BrowserOS, where I build browsers and tooling for AI agents.</p>
        <p class="body muted">I maintain an open source toolkit for the Agent Client Protocol, which is how any coding agent plugs into anything else.</p>
      </div>
      <div class="stack">
        <div class="meta-row" style="flex-direction:column;gap:18px;align-items:flex-start">
          <span class="mist">Work</span>
          <span style="font-size:27px;text-transform:none;letter-spacing:normal;color:var(--color-bone-white)">browseros.com</span>
        </div>
        <div class="meta-row" style="flex-direction:column;gap:18px;align-items:flex-start;margin-top:18px">
          <span class="mist">Open source</span>
          <span class="accent" style="font-size:27px;text-transform:none;letter-spacing:normal">github.com/DaniAkash</span>
        </div>
        <div class="meta-row" style="flex-direction:column;gap:18px;align-items:flex-start;margin-top:18px">
          <span class="mist">Elsewhere</span>
          <span class="spark" style="font-size:27px;text-transform:none;letter-spacing:normal">@dani_akash_</span>
        </div>
      </div>
    </div>
  </div>
  <div class="foot"><div class="title">AI Factories</div><div>Opening</div></div>
</div>

<!--
Switch to the browser here. File the issue live on the demo repo, read the title
and the expected-versus-actual out loud, and submit. Do not explain the system yet.
-->

---
class: 'dark'
---

<div class="slide-shell">
  <div class="chrome"><div>Demo</div><ChromeCounter /></div>
  <div class="frame">
    <div style="max-width:1320px">
      <div class="kicker">A REAL BUG</div>
      <h2 class="t-lg">One real bug,<br/>filed right now.</h2>
      <p class="body" style="margin-top:45px">A small defect in a working repository, with a test that already fails because of it.</p>
    </div>
  </div>
  <div class="foot"><div class="title">AI Factories</div><div>Opening</div></div>
</div>

<!--
Switch to the terminal. Start the run, wait for the worktree and conventions lines
to appear, then shrink it to a corner pane and leave it there. Say one sentence:
we come back to this at the end. Then move on and do not look at it again.
-->

---
class: 'dark'
---

<div class="slide-shell">
  <div class="chrome"><div>Demo</div><ChromeCounter /></div>
  <div class="frame">
    <div class="split">
      <div>
        <div class="kicker">THE LINE STARTS</div>
        <h2 class="t-md">The line<br/>is running.</h2>
        <p class="body" style="margin-top:36px">An agent now has the issue, a clean checkout of the repository, and no ability to push anything.</p>
      </div>
      <div>
        <pre class="code sm"><span class="vio">$</span> <span class="hi">aalai run &lt;repo&gt; &lt;issue&gt;</span>
&nbsp;
<span class="off">pipeline </span>  run starting
<span class="off">workspace</span>  worktree ready
<span class="off">pipeline </span>  conventions detected
<span class="off">agent    </span>  <span class="amb">working…</span></pre>
      </div>
    </div>
  </div>
  <div class="foot"><div class="title">AI Factories</div><div>Opening</div></div>
</div>


---
class: 'dark'
---

<div class="slide-shell">
  <div class="chrome"><div>The pattern</div><ChromeCounter /></div>
  <div class="frame">
    <div style="max-width:1500px">
      <div class="kicker">DEFINITION</div>
      <h2 class="t-sm" style="line-height:1.25">An AI software factory is a delivery pipeline where <span class="accent">every stage is an agent</span>: one triages the issue, one writes the spec, one implements it, one reviews it, and <span class="spark">a human still owns the merge.</span></h2>
    </div>
  </div>
  <div class="foot"><div class="title">AI Factories</div><div>The pattern</div></div>
</div>

---
class: 'dark'
---

<div class="slide-shell">
  <div class="chrome"><div>The pattern</div><ChromeCounter /></div>
  <div class="frame">
    <div>
      <div class="kicker">THE DIAGRAM EVERYONE DRAWS</div>
      <pre class="code lg" style="margin-top:45px">issue <span class="off">──▶</span> <span class="hi">classify</span> <span class="off">──▶</span> <span class="hi">analyze</span> <span class="off">──▶</span> <span class="hi">implement</span> <span class="off">──▶</span> <span class="hi">review</span> <span class="off">──▶</span> <span class="amb">human merge</span></pre>
      <p class="body" style="margin-top:60px">This diagram is accurate. It is also about a third of what a working factory contains.</p>
    </div>
  </div>
  <div class="foot"><div class="title">AI Factories</div><div>The pattern</div></div>
</div>

---
class: 'dark'
---

<div class="slide-shell">
  <div class="chrome"><div>The pattern</div><ChromeCounter /></div>
  <div class="frame">
    <div>
      <div class="kicker">ONE JOB EACH</div>
      <div class="numbered" style="margin-top:36px">
        <div class="row"><div class="n">01</div><div><div class="t">Classifier</div><div class="d">Type, priority, complexity. Is this even actionable, or does someone need to ask a question first?</div></div></div>
        <div class="row"><div class="n">02</div><div><div class="t">Analyst</div><div class="d">Reads the real repository. Produces a plan and, critically, the acceptance criteria.</div></div></div>
        <div class="row"><div class="n">03</div><div><div class="t">Implementer</div><div class="d">Writes the code. Runs the repository's own checks. Records what it ran and what came back.</div></div></div>
        <div class="row"><div class="n">04</div><div><div class="t">Reviewer</div><div class="d">Judges the real diff against those acceptance criteria, one by one, with evidence.</div></div></div>
        <div class="row"><div class="n">05</div><div><div class="t spark">A person</div><div class="d">Merges. This is the only station nobody automated.</div></div></div>
      </div>
    </div>
  </div>
  <div class="foot"><div class="title">AI Factories</div><div>The pattern</div></div>
</div>

---
class: 'dark'
---

<div class="slide-shell">
  <div class="chrome"><div>Evidence</div><ChromeCounter /></div>
  <div class="frame">
    <div style="max-width:1320px">
      <div class="kicker">THIS IS NOT A THOUGHT EXPERIMENT</div>
      <h2 class="t-lg">It is already<br/>running in<br/>the open.</h2>
      <p class="body" style="margin-top:45px">Vercel operates one on the AI SDK repository. The framework underneath it and the factory built on it are both open source, so everything that follows is quoted from code rather than from a write-up.</p>
    </div>
  </div>
  <div class="foot"><div class="title">AI Factories</div><div>The pattern</div></div>
</div>

---
class: 'dark'
---

<div class="slide-shell">
  <div class="chrome"><div>Evidence</div><ChromeCounter /></div>
  <div class="frame">
    <div class="split even">
      <div>
        <div class="kicker">FOUR WEEKS IN</div>
        <h2 class="t-md">Roughly a third<br/>of merged pull<br/>requests.</h2>
        <p class="body muted" style="margin-top:36px">And a maintainer approved every single one of them. The factory proposes. It does not decide.</p>
      </div>
      <div class="stack loose">
        <div class="stat"><div class="v accent">~⅓</div><div class="l">Of weekly merges, authored by the line</div></div>
        <div class="stat"><div class="v spark">1,022 → 844</div><div class="l">Open issues over the same window</div></div>
        <div class="caption" style="max-width:480px">Figures are an August 2026 snapshot from the published write-up. Treat them as a direction of travel, not a live dashboard.</div>
      </div>
    </div>
  </div>
  <div class="foot"><div class="title">AI Factories</div><div>The pattern</div></div>
</div>

---
class: 'dark'
---

<div class="slide-shell">
  <div class="chrome"><div>Evidence</div><ChromeCounter /></div>
  <div class="frame">
    <div class="split">
      <div>
        <div class="kicker">THE DECISION THAT MATTERS</div>
        <h2 class="t-md">They did not build<br/>one clever agent.</h2>
        <p class="body" style="margin-top:36px">They split the work into narrow stations, each with one job and an explicit contract to the next one.</p>
      </div>
      <div>
        <p class="body muted">A single general-purpose agent has no seam you can inspect. When it is wrong, you get an opinion about why.</p>
        <p class="body muted">Five stations with typed handoffs give you a place to stand between every pair of them. That is the whole difference.</p>
      </div>
    </div>
  </div>
  <div class="foot"><div class="title">AI Factories</div><div>The pattern</div></div>
</div>

---
class: 'dark'
---

<div class="slide-shell">
  <div class="chrome"><div>Evidence</div><ChromeCounter /></div>
  <div class="frame">
    <div>
      <div class="kicker">WHAT WE ARE READING FROM</div>
      <h2 class="t-sm">A filesystem-first agent framework,<br/>and an open factory built on top of it.</h2>
      <div class="cols-2" style="margin-top:60px">
        <div>
          <pre class="code sm"><span class="vio">agent/</span>
  agent.ts          <span class="off">model, budget</span>
  instructions.ts   <span class="amb">the pipeline</span>
  channels/         <span class="off">how work arrives</span>
  subagents/        <span class="off">the stations</span>
  tools/            <span class="off">one per file</span>
  lib/              <span class="off">trust, models, policy</span></pre>
        </div>
        <div>
          <p class="body muted">Capabilities are discovered from file structure. A directory named <span class="mist">implementer</span> becomes the station called implementer. There is no registry to keep in sync.</p>
          <p class="body muted">That means you can read the whole system by reading the tree.</p>
        </div>
      </div>
    </div>
  </div>
  <div class="foot"><div class="title">AI Factories</div><div>The pattern</div></div>
</div>

---
class: 'dark'
---

<div class="slide-shell">
  <div class="chrome"><div>The pattern</div><ChromeCounter /></div>
  <div class="frame">
    <div style="max-width:1400px">
      <div class="kicker">THE DIAGRAM</div>
      <h2 class="t-lg">That is the<br/>diagram.</h2>
      <p class="body" style="margin-top:45px;font-size:36px">Here is what the diagram leaves out.</p>
    </div>
  </div>
  <div class="foot"><div class="title">AI Factories</div><div>The pattern</div></div>
</div>


---
class: 'dark'
---

<div class="slide-shell">
  <div class="chrome"><div>Three lanes</div><ChromeCounter /></div>
  <div class="frame">
    <div>
      <div class="kicker">READING THE SOURCE</div>
      <h2 class="t-sm">The pipeline is one prompt file<br/>and five thin definitions.</h2>
      <div class="cols-2" style="margin-top:60px">
        <pre class="code sm"><span class="amb">instructions.ts</span>      <span class="hi">91 lines</span>
<span class="off">subagents/classifier  ~15 lines of config</span>
<span class="off">subagents/analyst     ~15 lines of config</span>
<span class="off">subagents/implementer ~15 lines of config</span>
<span class="off">subagents/reviewer    ~15 lines of config</span></pre>
        <pre class="code sm"><span class="vio">lib/trust.ts</span>          <span class="hi">the trust authority</span>
<span class="vio">lib/github/approval.ts</span> <span class="hi">every policy</span>
<span class="vio">channels/github.ts</span>     <span class="hi">dispatch gates</span>
<span class="vio">lib/blob.ts</span>            <span class="hi">namespace registry</span>
<span class="vio">lib/artifacts/</span>         <span class="hi">handoff contracts</span>
<span class="vio">evals/</span>                 <span class="hi">the failure taxonomy</span></pre>
      </div>
    </div>
  </div>
  <div class="foot"><div class="title">AI Factories</div><div>Three lanes</div></div>
</div>

---
class: 'dark'
---

<div class="slide-shell">
  <div class="chrome"><div>Three lanes</div><ChromeCounter /></div>
  <div class="frame">
    <div>
      <div class="kicker">THE RATIO NOBODY SHOWS YOU</div>
      <h2 class="t-lg" style="margin-bottom:60px">30 <span class="dim">/</span> <span class="accent">70</span></h2>
      <div class="ratio">
        <span style="flex:30;background:var(--color-bone-white)"></span>
        <span style="flex:70;background:var(--color-electric-iris)"></span>
      </div>
      <div style="display:flex;gap:36px;margin-top:27px">
        <p class="caption" style="flex:30">The conveyor belt. The part every talk demos.</p>
        <p class="caption" style="flex:70">Trust enforcement and memory namespacing. The part that makes it safe to leave running.</p>
      </div>
    </div>
  </div>
  <div class="foot"><div class="title">AI Factories</div><div>Three lanes</div></div>
</div>

---
class: 'dark'
---

<div class="slide-shell">
  <div class="chrome"><div>Three lanes</div><ChromeCounter /></div>
  <div class="frame">
    <div style="max-width:1400px">
      <div class="kicker">WHAT IS ACTUALLY IN THERE</div>
      <h2 class="t-lg">Three lanes,<br/>not one belt.</h2>
    </div>
  </div>
  <div class="foot"><div class="title">AI Factories</div><div>Three lanes</div></div>
</div>

---
class: 'dark'
---

<div class="slide-shell">
  <div class="chrome"><div>Three lanes</div><ChromeCounter /></div>
  <div class="frame">
    <div class="split">
      <div>
        <div class="kicker">LANE ONE</div>
        <h2 class="t-md">The work.</h2>
        <p class="body" style="margin-top:36px">Five stations, each with a machine-checkable output schema. Issue in, branch out.</p>
        <p class="body muted">This is the easy part. Genuinely, a weekend.</p>
      </div>
      <pre class="code sm"><span class="hi">classify</span>
   <span class="off">↓ type, priority, actionable?</span>
<span class="hi">analyze</span>
   <span class="off">↓ plan +</span> <span class="amb">acceptance criteria</span>
<span class="hi">implement</span>
   <span class="off">↓ branch + verification log</span>
<span class="hi">review</span>
   <span class="off">↓ verdict + per-criterion evidence</span>
<span class="amb">human</span></pre>
    </div>
  </div>
  <div class="foot"><div class="title">AI Factories</div><div>Three lanes</div></div>
</div>

---
class: 'dark'
---

<div class="slide-shell">
  <div class="chrome"><div>Three lanes</div><ChromeCounter /></div>
  <div class="frame">
    <div class="split">
      <div>
        <div class="kicker">LANE TWO</div>
        <h2 class="h accent">The trust.</h2>
        <p class="body" style="margin-top:36px">Who asked for this. What were they allowed to ask for. What may this particular run touch.</p>
        <p class="body muted">Decided once, at the door, from data the model never sees.</p>
      </div>
      <div class="stack">
        <p class="body muted">An issue body is instructions to an agent with file and shell access. On a public repository, anyone can write one.</p>
        <p class="body muted">Every serious control in the system exists because of that one sentence.</p>
      </div>
    </div>
  </div>
  <div class="foot"><div class="title">AI Factories</div><div>Three lanes</div></div>
</div>

---
class: 'dark'
---

<div class="slide-shell">
  <div class="chrome"><div>Three lanes</div><ChromeCounter /></div>
  <div class="frame">
    <div class="split">
      <div>
        <div class="kicker">LANE THREE</div>
        <h2 class="h spark">The memory.</h2>
        <p class="body" style="margin-top:36px">What survives the run. Who is allowed to write into it. How one station hands another a twenty page document without pushing it through the orchestrator.</p>
      </div>
      <pre class="code sm"><span class="amb">factory-brain/</span>  <span class="off">durable repo notes</span>
   <span class="off">read by every run</span>
   <span class="hi">written only by trusted callers</span>
&nbsp;
<span class="amb">artifacts/</span>      <span class="off">station handoffs, by id</span>
<span class="amb">user-prefs/</span>     <span class="off">keyed on the principal</span></pre>
    </div>
  </div>
  <div class="foot"><div class="title">AI Factories</div><div>Three lanes</div></div>
</div>

---
class: 'dark'
---

<div class="slide-shell">
  <div class="chrome"><div>Three lanes</div><ChromeCounter /></div>
  <div class="frame">
    <div style="max-width:1560px">
      <h2 class="t-sm" style="line-height:1.3">A factory is not a pipeline with<br/>safety bolted on.<br/><br/>It is a <span class="accent">trust system</span> that happens<br/>to move code through it.</h2>
    </div>
  </div>
  <div class="foot"><div class="title">AI Factories</div><div>Three lanes</div></div>
</div>

---
class: 'dark'
---

<div class="slide-shell">
  <div class="chrome"><div>Three lanes</div><ChromeCounter /></div>
  <div class="frame">
    <div class="split">
      <div>
        <div class="kicker">SO WHAT DO YOU DO WITH THAT</div>
        <h2 class="t-md">Design lanes two<br/>and three first.</h2>
        <p class="body" style="margin-top:36px">The stations slot into them afterwards, easily.</p>
      </div>
      <div>
        <p class="body">Build from the belt diagram alone and you will ship the thirty percent, then discover the seventy in production.</p>
        <p class="body muted">Retrofitting a trust lane into a running factory means auditing every tool you already wrote.</p>
      </div>
    </div>
  </div>
  <div class="foot"><div class="title">AI Factories</div><div>Three lanes</div></div>
</div>


---
class: 'dark'
---

<div class="slide-shell">
  <div class="chrome"><div>Anatomy</div><ChromeCounter /></div>
  <div class="frame">
    <div style="max-width:1400px">
      <div class="kicker">ANATOMY</div>
      <h2 class="t-lg">The anatomy,<br/>organ by organ.</h2>
      <p class="body" style="margin-top:45px">For each one: what it is, how you achieve it on any stack, and what it looks like in a factory running on a laptop.</p>
    </div>
  </div>
  <div class="foot"><div class="title">AI Factories</div><div>Anatomy</div></div>
</div>

---
class: 'dark'
---

<div class="slide-shell">
  <div class="chrome"><div>Contracts</div><ChromeCounter /></div>
  <div class="frame">
    <div class="split">
      <div>
        <div class="kicker">CONTRACTS</div>
        <h2 class="t-md">The schema<br/>is the interface.</h2>
        <p class="body" style="margin-top:36px">Every station returns structured output, not prose. That single field turns a conversation into a contract you can check.</p>
      </div>
      <pre class="code sm"><span class="off">analyst returns</span>
  problem_statement
  approach
  plan[]
  affected_surface[]
  risks[]
  <span class="amb">acceptance_criteria[]</span>
  test_strategy
  assumptions[]
  open_questions[]</pre>
    </div>
  </div>
  <div class="foot"><div class="title">AI Factories</div><div>Anatomy</div></div>
</div>

---
class: 'dark'
---

<div class="slide-shell">
  <div class="chrome"><div>Contracts</div><ChromeCounter /></div>
  <div class="frame">
    <div style="max-width:1560px">
      <div class="kicker">THE ONE TO STEAL</div>
      <h2 class="t-sm" style="line-height:1.3">The analyst writes the grading rubric<br/><span class="accent">before the code exists</span>, and the reviewer<br/>is handed it verbatim.</h2>
      <p class="body" style="margin-top:54px">Which means the station that writes the code never gets to define what "done" means for its own work.</p>
    </div>
  </div>
  <div class="foot"><div class="title">AI Factories</div><div>Anatomy</div></div>
</div>

---
class: 'dark'
---

<div class="slide-shell">
  <div class="chrome"><div>Independence</div><ChromeCounter /></div>
  <div class="frame">
    <div class="split">
      <div>
        <div class="kicker">INDEPENDENCE</div>
        <h2 class="t-md">Independence is<br/>mechanical, not<br/>prompted.</h2>
        <p class="body" style="margin-top:36px">"Please review objectively" is not a control.</p>
      </div>
      <div class="numbered">
        <div class="row"><div class="n">01</div><div><div class="t">Different vendor</div><div class="d">Implementer and reviewer are pinned to different model providers, on purpose, in one map.</div></div></div>
        <div class="row"><div class="n">02</div><div><div class="t">Different sandbox</div><div class="d">Its own checkout. It fetches the pushed branch itself.</div></div></div>
        <div class="row"><div class="n">03</div><div><div class="t">Different input</div><div class="d">It gets the criteria and a git ref. It never sees the implementer's reasoning.</div></div></div>
      </div>
    </div>
  </div>
  <div class="foot"><div class="title">AI Factories</div><div>Anatomy</div></div>
</div>

---
class: 'dark'
---

<div class="slide-shell">
  <div class="chrome"><div>Independence</div><ChromeCounter /></div>
  <div class="frame">
    <div>
      <div class="kicker">FROM THE REVIEWER'S OWN INSTRUCTIONS</div>
      <h2 class="t-sm" style="margin-top:36px;max-width:1500px">"Never judge from the change summary alone.<br/><span class="spark">Summaries describe intent, diffs describe reality.</span>"</h2>
      <p class="body" style="margin-top:54px">And where a claim is cheap to check, it re-runs the check itself rather than trusting the report.</p>
    </div>
  </div>
  <div class="foot"><div class="title">AI Factories</div><div>Anatomy</div></div>
</div>

---
class: 'dark'
---

<div class="slide-shell">
  <div class="chrome"><div>Trust</div><ChromeCounter /></div>
  <div class="frame">
    <div class="split">
      <div>
        <div class="kicker">TRUST</div>
        <h2 class="t-md">Trust is decided<br/>once, at the door.</h2>
        <p class="body" style="margin-top:36px">From a signed webhook or an authenticated API response. Never re-derived downstream from anything the model can read.</p>
      </div>
      <pre class="code sm"><span class="off">at dispatch:</span>
  comment author is
  OWNER / MEMBER / COLLABORATOR
        <span class="off">↓</span>
  <span class="hi">stampTrusted(auth)</span>
&nbsp;
<span class="off">label applied by a maintainer:</span>
  <span class="hi">stampAutonomous(auth, issue)</span>
  <span class="off">principal is rewritten. the run
  never executes as the labeler.</span></pre>
    </div>
  </div>
  <div class="foot"><div class="title">AI Factories</div><div>Anatomy</div></div>
</div>

---
class: 'dark'
---

<div class="slide-shell">
  <div class="chrome"><div>Trust</div><ChromeCounter /></div>
  <div class="frame">
    <div class="split">
      <div>
        <div class="kicker">A DETAIL YOU ONLY FIND IN SOURCE</div>
        <h2 class="t-sm">The webhook tells you the wrong person's permission.</h2>
      </div>
      <div>
        <p class="body">The issues webhook carries the <span class="mist">issue author's</span> association. Not the labeler's.</p>
        <p class="body">And GitHub fires the labeled event for labels attached at creation time, which issue templates let unauthenticated reporters do.</p>
        <p class="body spark">So without a live permission check, anyone opening an issue could start an autonomous run.</p>
      </div>
    </div>
  </div>
  <div class="foot"><div class="title">AI Factories</div><div>Anatomy</div></div>
</div>

---
class: 'dark'
---

<div class="slide-shell">
  <div class="chrome"><div>Trust</div><ChromeCounter /></div>
  <div class="frame">
    <div class="split">
      <div>
        <div class="kicker">WHEN NOBODY IS WATCHING</div>
        <h2 class="t-md">Deny.<br/>Do not park.</h2>
      </div>
      <div>
        <p class="body">An unattended run has nobody to answer an approval prompt, so a prompt strands the session forever.</p>
        <p class="body muted">A denial resolves in one step. A parked autonomous run is a leaked resource that looks like patience.</p>
      </div>
    </div>
  </div>
  <div class="foot"><div class="title">AI Factories</div><div>Anatomy</div></div>
</div>

---
class: 'dark'
---

<div class="slide-shell">
  <div class="chrome"><div>The human gate</div><ChromeCounter /></div>
  <div class="frame">
    <div class="split">
      <div>
        <div class="kicker">THE HUMAN GATE</div>
        <h2 class="t-md">Merge is not<br/>guarded.<br/><span class="accent">It is absent.</span></h2>
      </div>
      <pre class="code sm"><span class="off">github extension, include: [</span>
  getRepository, getFileContent,
  searchCode, listIssues,
  createIssue, addIssueComment,
  addLabels, createPullRequest,
  <span class="off">… 31 tools total</span>
<span class="off">]</span>
&nbsp;
<span class="hi">mergePullRequest</span>  <span class="amb">← not in the list</span></pre>
    </div>
  </div>
  <div class="foot"><div class="title">AI Factories</div><div>Anatomy</div></div>
</div>

---
class: 'dark'
---

<div class="slide-shell">
  <div class="chrome"><div>The human gate</div><ChromeCounter /></div>
  <div class="frame">
    <div style="max-width:1500px">
      <div class="kicker">THE PRINCIPLE UNDERNEATH</div>
      <h2 class="t-sm" style="line-height:1.3">An absent tool is <span class="accent">one decision</span>.<br/>A guarded tool is a decision<br/><span class="spark">every single run.</span></h2>
      <p class="body" style="margin-top:54px">Put the human gate where reversal is expensive, then remove the capability entirely instead of writing a policy for it.</p>
    </div>
  </div>
  <div class="foot"><div class="title">AI Factories</div><div>Anatomy</div></div>
</div>

---
class: 'dark'
---

<div class="slide-shell">
  <div class="chrome"><div>Bounds</div><ChromeCounter /></div>
  <div class="frame">
    <div class="split">
      <div>
        <div class="kicker">BOUNDS</div>
        <h2 class="t-md">Bound every loop.</h2>
        <p class="body" style="margin-top:36px">Two revision cycles. Two CI fix attempts. Then it stops and tells a person.</p>
      </div>
      <div>
        <p class="body">Unbounded agent loops do not crash. They burn tokens quietly until somebody notices the bill.</p>
        <p class="body muted">"If the work still doesn't pass, stop, report the unresolved findings, and don't open a pull request."</p>
      </div>
    </div>
  </div>
  <div class="foot"><div class="title">AI Factories</div><div>Anatomy</div></div>
</div>

---
class: 'dark'
---

<div class="slide-shell">
  <div class="chrome"><div>Bounds</div><ChromeCounter /></div>
  <div class="frame">
    <div class="split">
      <div>
        <div class="kicker">THE HARD PART</div>
        <h2 class="t-sm">How do you bound a loop<br/>when every run is a<br/>fresh process?</h2>
      </div>
      <div>
        <p class="body">The CI fix loop counts its own earlier comments on the pull request thread. The thread is the durable state.</p>
        <p class="body spark">And it posts the attempt comment before attempting the fix, so a run that dies mid-fix still leaves its mark for the next one to count.</p>
      </div>
    </div>
  </div>
  <div class="foot"><div class="title">AI Factories</div><div>Anatomy</div></div>
</div>

---
class: 'dark'
---

<div class="slide-shell">
  <div class="chrome"><div>Memory</div><ChromeCounter /></div>
  <div class="frame">
    <div class="split">
      <div>
        <div class="kicker">MEMORY</div>
        <h2 class="t-md">The factory brain.</h2>
        <p class="body" style="margin-top:36px">Durable notes about the repository that every run starts from. Build quirks, verification gotchas, review findings that keep recurring.</p>
      </div>
      <div>
        <p class="body">An unattended run may <span class="mist">read</span> it.</p>
        <p class="body spark">An unattended run may never write it.</p>
        <p class="body muted">Because a labeled issue's body is untrusted input, and a brain entry becomes context for every future run. That is the highest blast radius failure in the whole system.</p>
      </div>
    </div>
  </div>
  <div class="foot"><div class="title">AI Factories</div><div>Anatomy</div></div>
</div>

---
class: 'dark'
---

<div class="slide-shell">
  <div class="chrome"><div>Memory</div><ChromeCounter /></div>
  <div class="frame">
    <div class="split">
      <div>
        <div class="kicker">HANDOFF ARTIFACTS</div>
        <h2 class="t-sm">Long documents move<br/>by id, never through<br/>the orchestrator.</h2>
        <p class="body" style="margin-top:36px">This is the mechanism that keeps a five station pipeline inside a context budget.</p>
      </div>
      <pre class="code sm"><span class="off">analyst  →</span> save_artifact
            <span class="hi">analysis-dedupe-x9f2k</span>
&nbsp;
<span class="off">orchestrator relays the id only</span>
&nbsp;
<span class="off">implementer →</span> read_artifact
<span class="off">reviewer    →</span> read_artifact
&nbsp;
<span class="amb">"Never paste an artifact's contents
into a station message."</span></pre>
    </div>
  </div>
  <div class="foot"><div class="title">AI Factories</div><div>Anatomy</div></div>
</div>

---
class: 'dark'
---

<div class="slide-shell">
  <div class="chrome"><div>Memory</div><ChromeCounter /></div>
  <div class="frame">
    <div class="split">
      <div>
        <div class="kicker">THE GUARD</div>
        <h2 class="t-sm">The artifact id is<br/>supplied by a model.</h2>
      </div>
      <div>
        <pre class="code sm"><span class="hi">/^[a-z0-9]+(?:-[a-z0-9]+)*$/</span>
&nbsp;
<span class="off">anchored. no dots. no slashes.</span></pre>
        <p class="body" style="margin-top:36px">Without that, a station could pass <span class="mist">../factory-brain/&lt;hash&gt;.md</span> and read a managed document through a tool never meant to reach one.</p>
      </div>
    </div>
  </div>
  <div class="foot"><div class="title">AI Factories</div><div>Anatomy</div></div>
</div>

---
class: 'dark'
---

<div class="slide-shell">
  <div class="chrome"><div>Evals</div><ChromeCounter /></div>
  <div class="frame">
    <div class="split">
      <div>
        <div class="kicker">THE PART THAT IS ACTUALLY THE PRODUCT</div>
        <h2 class="t-md">The directory<br/>structure is the<br/>failure taxonomy.</h2>
        <p class="body" style="margin-top:36px">Assert on the trace, not on the prose. The judge model is for the fuzzy tail only.</p>
      </div>
      <pre class="code sm"><span class="vio">evals/</span>
  <span class="hi">routing/</span>   <span class="off">right station?</span>
    classifier-first
    needs-clarification
  <span class="hi">safety/</span>    <span class="off">refuses what it should?</span>
    prompt-injection
    no-direct-push-to-main
    ship-gate-parks
  <span class="hi">pipeline/</span>  <span class="off">end to end</span>
&nbsp;
<span class="amb">t.calledSubagent('implementer', { count: 0 })</span></pre>
    </div>
  </div>
  <div class="foot"><div class="title">AI Factories</div><div>Anatomy</div></div>
</div>

---
class: 'dark'
---

<div class="slide-shell">
  <div class="chrome"><div>Anatomy</div><ChromeCounter /></div>
  <div class="frame">
    <div>
      <div class="kicker">REFERENCE</div>
      <h2 class="t-sm" style="margin-bottom:45px">Twelve invariants worth stealing.</h2>
      <div class="cols-2" style="gap:90px">
        <pre class="code sm"><span class="vio">01</span> <span class="hi">Trust decided once, at the door</span>
<span class="vio">02</span> <span class="hi">Delete the capability, don't guard it</span>
<span class="vio">03</span> <span class="hi">Inert by construction</span>
<span class="vio">04</span> <span class="hi">Criteria before code, verbatim to review</span>
<span class="vio">05</span> <span class="hi">Untrusted input never writes memory</span>
<span class="vio">06</span> <span class="hi">Bound every loop, durably</span></pre>
        <pre class="code sm"><span class="vio">07</span> <span class="hi">Stations inherit nothing</span>
<span class="vio">08</span> <span class="hi">Independence is mechanical</span>
<span class="vio">09</span> <span class="hi">Evidence, not checkmarks</span>
<span class="vio">10</span> <span class="hi">Long docs move by id</span>
<span class="vio">11</span> <span class="hi">Centralise what must see everything</span>
<span class="vio">12</span> <span class="hi">Assert on the trace, not the prose</span></pre>
      </div>
    </div>
  </div>
  <div class="foot"><div class="title">AI Factories</div><div>Anatomy</div></div>
</div>


---
class: 'dark'
---

<div class="slide-shell">
  <div class="chrome"><div>Running it yourself</div><ChromeCounter /></div>
  <div class="frame">
    <div style="max-width:1400px">
      <div class="kicker">THE OBVIOUS OBJECTION</div>
      <h2 class="t-lg">"Fine, if you<br/>have a platform."</h2>
    </div>
  </div>
  <div class="foot"><div class="title">AI Factories</div><div>Running it yourself</div></div>
</div>

---
class: 'dark'
---

<div class="slide-shell">
  <div class="chrome"><div>Running it yourself</div><ChromeCounter /></div>
  <div class="frame">
    <div class="split">
      <div>
        <div class="kicker">THE ANSWER</div>
        <h2 class="t-md">Every hosted<br/>primitive has a<br/>local substitute.</h2>
        <p class="body" style="margin-top:36px">The framework is self-hostable and ships local sandbox backends in the box. This was the big unknown going in, and it resolved well.</p>
      </div>
      <div>
        <p class="body">A sandbox backend that runs micro VMs on Apple Silicon, with domain-level network policy and credential brokering at the firewall.</p>
        <p class="body muted">Which means the entire isolation model reproduces on a laptop without compromise. You are choosing backends, not reimplementing durability.</p>
      </div>
    </div>
  </div>
  <div class="foot"><div class="title">AI Factories</div><div>Running it yourself</div></div>
</div>

---
class: 'dark'
---

<div class="slide-shell">
  <div class="chrome"><div>Running it yourself</div><ChromeCounter /></div>
  <div class="frame">
    <div>
      <div class="kicker">CLOUD PRIMITIVE → WHAT YOU RUN INSTEAD</div>
      <pre class="code" style="margin-top:45px"><span class="off">Functions        ──▶</span>  <span class="hi">a long-lived process under launchd</span>
<span class="off">Durable workflow ──▶</span>  <span class="hi">workflow state on disk</span>
<span class="off">Hosted sandbox   ──▶</span>  <span class="hi">local micro VMs, or a git worktree</span>
<span class="off">Blob storage     ──▶</span>  <span class="hi">the filesystem, same guards</span>
<span class="off">Model gateway    ──▶</span>  <span class="vio">any ACP agent as a model</span>
<span class="off">Credential broker──▶</span>  <span class="hi">the CLI you are already signed into</span>
<span class="off">Cron             ──▶</span>  <span class="hi">launchd</span>
<span class="off">Webhooks         ──▶</span>  <span class="amb">← the one real gap</span></pre>
    </div>
  </div>
  <div class="foot"><div class="title">AI Factories</div><div>Running it yourself</div></div>
</div>

---
class: 'dark'
---

<div class="slide-shell">
  <div class="chrome"><div>Running it yourself</div><ChromeCounter /></div>
  <div class="frame">
    <div class="split">
      <div>
        <div class="kicker">NO PUBLIC URL ON A LAPTOP</div>
        <h2 class="t-md">So you poll.</h2>
        <p class="body" style="margin-top:36px">Or you run a tunnel. Polling wins for a first version: zero inbound surface, nothing to keep alive.</p>
      </div>
      <div>
        <p class="body">And it is not the security downgrade it sounds like.</p>
        <p class="body muted">A webhook signature proves the payload is authentic. An authenticated API call you made yourself proves it more directly, and removes the constant-time HMAC comparison entirely.</p>
        <p class="body spark">What polling actually costs you is latency, and a deduplication layer.</p>
      </div>
    </div>
  </div>
  <div class="foot"><div class="title">AI Factories</div><div>Running it yourself</div></div>
</div>

---
class: 'dark'
---

<div class="slide-shell">
  <div class="chrome"><div>Aalai</div><ChromeCounter /></div>
  <div class="frame">
    <div style="max-width:1400px">
      <div class="kicker">A FACTORY ON A LAPTOP</div>
      <h2 class="t-lg">ஆலை</h2>
      <p class="body" style="margin-top:36px;font-size:36px">Aalai. Tamil for <span class="spark">factory</span>.</p>
      <p class="body muted" style="margin-top:27px">A Bun service on a MacBook. GitHub CLI for intake and delivery, an ACP agent for the work.</p>
    </div>
  </div>
  <div class="foot"><div class="title">AI Factories</div><div>Running it yourself</div></div>
</div>

---
class: 'dark'
---

<div class="slide-shell">
  <div class="chrome"><div>Aalai</div><ChromeCounter /></div>
  <div class="frame">
    <div>
      <div class="kicker">THE WHOLE THING, ONE LINE</div>
      <pre class="code" style="margin-top:45px"><span class="hi">poll</span> <span class="off">──▶</span> <span class="hi">screen</span> <span class="off">──▶</span> <span class="hi">claim</span> <span class="off">──▶</span> <span class="hi">worktree</span> <span class="off">──▶</span> <span class="vio">agent</span> <span class="off">──▶</span> <span class="hi">verify diff</span> <span class="off">──▶</span> <span class="hi">commit + push</span> <span class="off">──▶</span> <span class="amb">draft PR</span>
                <span class="off">(sqlite)</span>            <span class="off">(cwd)</span>       <span class="off">▲</span>                   <span class="off">▲</span>
                                                <span class="off">│</span>                   <span class="off">│</span>
                                <span class="amb">no credentials reach the agent ────┘</span></pre>
      <p class="body" style="margin-top:60px">That last line turned out to be an overclaim. More on it later.</p>
    </div>
  </div>
  <div class="foot"><div class="title">AI Factories</div><div>Running it yourself</div></div>
</div>

---
class: 'dark'
---

<div class="slide-shell">
  <div class="chrome"><div>Aalai</div><ChromeCounter /></div>
  <div class="frame">
    <div class="split">
      <div>
        <div class="kicker">THE SPLIT THAT MATTERS</div>
        <h2 class="t-md">The agent edits<br/>files. The factory<br/>does the git.</h2>
      </div>
      <div>
        <p class="body">The agent works in a disposable worktree and is told never to run a git command that writes.</p>
        <p class="body">Commit, push and pull request creation happen outside the agent turn, gated on a real diff.</p>
        <p class="body spark">Across three live runs, the agent ran zero git writes. It read the repo with git show and git diff, and left delivery alone.</p>
      </div>
    </div>
  </div>
  <div class="foot"><div class="title">AI Factories</div><div>Running it yourself</div></div>
</div>

---
class: 'dark'
---

<div class="slide-shell">
  <div class="chrome"><div>Running it yourself</div><ChromeCounter /></div>
  <div class="frame">
    <div style="max-width:1560px">
      <div class="kicker">WHERE LOCAL WINS</div>
      <h2 class="t-sm" style="line-height:1.3">A gateway gives you a different <span class="dim">model</span>.<br/>ACP gives you a different <span class="accent">agent.</span></h2>
    </div>
  </div>
  <div class="foot"><div class="title">AI Factories</div><div>Running it yourself</div></div>
</div>

---
class: 'dark'
---

<div class="slide-shell">
  <div class="chrome"><div>Running it yourself</div><ChromeCounter /></div>
  <div class="frame">
    <div class="split">
      <div>
        <div class="kicker">WHY THAT IS STRONGER</div>
        <h2 class="t-sm">Codex implements.<br/>Claude Code reviews.</h2>
        <p class="body" style="margin-top:36px">Different harness. Different tools. Different context assembly. Different system prompt written by a different company.</p>
        <p class="body spark">They disagree about far more than weights.</p>
      </div>
      <pre class="code sm"><span class="off">createAcpxProvider({</span>
  agent: <span class="hi">'codex'</span>,
  cwd: worktree,
  sessionMode: <span class="hi">'oneshot'</span>,
  permissionMode: <span class="hi">'approve-all'</span>,
  nonInteractivePermissions: <span class="hi">'deny'</span>,
<span class="off">})</span>
&nbsp;
<span class="off">any ACP agent, one interface:</span>
<span class="vio">claude · codex · gemini · copilot
cursor · opencode · qwen · …</span></pre>
    </div>
  </div>
  <div class="foot"><div class="title">AI Factories</div><div>Running it yourself</div></div>
</div>

---
class: 'dark'
---

<div class="slide-shell">
  <div class="chrome"><div>Running it yourself</div><ChromeCounter /></div>
  <div class="frame">
    <div class="split">
      <div>
        <div class="kicker">THE CONSTRAINT NOBODY MENTIONS</div>
        <h2 class="t-md">A laptop sleeps.</h2>
      </div>
      <div>
        <p class="body">launchd will restart your process. It will not wake your machine.</p>
        <p class="body muted">A factory that silently stops for eight hours a night is worse than one that tells you it stopped. Whatever runs the loop needs a heartbeat that is visible when it goes quiet.</p>
      </div>
    </div>
  </div>
  <div class="foot"><div class="title">AI Factories</div><div>Running it yourself</div></div>
</div>


---
class: 'dark'
---

<div class="slide-shell">
  <div class="chrome"><div>Field notes</div><ChromeCounter /></div>
  <div class="frame">
    <div style="max-width:1500px">
      <div class="kicker">FIELD NOTES</div>
      <h2 class="t-lg">My demo passed.<br/>Here is what it<br/>did not prove.</h2>
    </div>
  </div>
  <div class="foot"><div class="title">AI Factories</div><div>Field notes</div></div>
</div>

---
class: 'dark'
---

<div class="slide-shell">
  <div class="chrome"><div>Field notes</div><ChromeCounter /></div>
  <div class="frame">
    <div>
      <div class="kicker">FOUR WAYS WORK DISAPPEARED SILENTLY</div>
      <div class="numbered" style="margin-top:36px">
        <div class="row"><div class="n">01</div><div><div class="t">The cursor moved before the work happened</div><div class="d">A crash mid-pass skipped every remaining issue. Permanently. No error, no retry.</div></div></div>
        <div class="row"><div class="n">02</div><div><div class="t">One page of fifty, then the cursor jumped past the rest</div><div class="d">Fine on a quiet repo. Catastrophic on a cold start against a busy one.</div></div></div>
        <div class="row"><div class="n">03</div><div><div class="t">Claims never expired</div><div class="d">A process killed mid-run left the issue marked as taken, and every later pass read that as "already done".</div></div></div>
        <div class="row"><div class="n">04</div><div><div class="t">Setup failures sat outside the failure path</div><div class="d">One bad issue aborted the whole tick, with the issue still claimed. Combined with the above: gone forever.</div></div></div>
      </div>
    </div>
  </div>
  <div class="foot"><div class="title">AI Factories</div><div>Field notes</div></div>
</div>

---
class: 'dark'
---

<div class="slide-shell">
  <div class="chrome"><div>Field notes</div><ChromeCounter /></div>
  <div class="frame">
    <div class="split">
      <div>
        <div class="kicker">THE SHARPEST ONE</div>
        <h2 class="t-sm">Because I had cloned<br/>the repo by hand,<br/>an hour earlier.</h2>
      </div>
      <div>
        <p class="body">Spawning a process into a directory that does not exist throws before your code runs.</p>
        <p class="body">The clone directory was probed before it was created.</p>
        <p class="body spark">So the very first code path a new user hits had never once executed. The demo was green.</p>
      </div>
    </div>
  </div>
  <div class="foot"><div class="title">AI Factories</div><div>Field notes</div></div>
</div>

---
class: 'dark'
---

<div class="slide-shell">
  <div class="chrome"><div>Field notes</div><ChromeCounter /></div>
  <div class="frame">
    <div style="max-width:1560px">
      <h2 class="t-sm" style="line-height:1.3">An end-to-end demo validates<br/>the <span class="accent">pipeline</span>.<br/><br/>It never validates its<br/><span class="spark">failure modes.</span></h2>
      <p class="body" style="margin-top:54px">Failure modes are what evals are for. That is why they are the product, not a chore.</p>
    </div>
  </div>
  <div class="foot"><div class="title">AI Factories</div><div>Field notes</div></div>
</div>

---
class: 'dark'
---

<div class="slide-shell">
  <div class="chrome"><div>Field notes</div><ChromeCounter /></div>
  <div class="frame">
    <div style="max-width:1500px">
      <div class="kicker">A CLAIM I SHIPPED</div>
      <h2 class="t-sm" style="line-height:1.3">"No credentials reach<br/>the agent."</h2>
      <p class="body" style="margin-top:54px;font-size:36px">That was an <span class="spark">overclaim.</span></p>
    </div>
  </div>
  <div class="foot"><div class="title">AI Factories</div><div>Field notes</div></div>
</div>

---
class: 'dark'
---

<div class="slide-shell">
  <div class="chrome"><div>Field notes</div><ChromeCounter /></div>
  <div class="frame">
    <div class="split">
      <div>
        <div class="kicker">WHY IT WAS WRONG</div>
        <h2 class="t-sm">Scrubbing tokens<br/>is real. It is not<br/>a boundary.</h2>
      </div>
      <div>
        <p class="body">The CLI authenticates through the system keyring. The agent has shell access and runs as the same user.</p>
        <p class="body">No process can hide a keyring from another process running as its own user.</p>
        <p class="body muted">So the agent could simply invoke an already-authenticated CLI, whatever my prompt said about it.</p>
      </div>
    </div>
  </div>
  <div class="foot"><div class="title">AI Factories</div><div>Field notes</div></div>
</div>

---
class: 'dark'
---

<div class="slide-shell">
  <div class="chrome"><div>Field notes</div><ChromeCounter /></div>
  <div class="frame">
    <div class="split">
      <div>
        <div class="kicker">THE ACCURATE SENTENCE</div>
        <h2 class="t-sm">The agent is constrained by<br/><span class="accent">where it runs</span> and by what the<br/>factory <span class="accent">refuses to deliver</span>.</h2>
        <p class="body" style="margin-top:36px">Not by being unable to reach credentials.</p>
      </div>
      <div class="numbered">
        <div class="row"><div class="n">01</div><div><div class="t">A disposable worktree</div><div class="d">Blast radius is a directory nobody keeps.</div></div></div>
        <div class="row"><div class="n">02</div><div><div class="t">A diff reviewed before any push</div><div class="d">No diff, no pull request. The factory decides, not the agent.</div></div></div>
        <div class="row"><div class="n">03</div><div><div class="t">Draft status, always</div><div class="d">A draft cannot merge itself.</div></div></div>
      </div>
    </div>
  </div>
  <div class="foot"><div class="title">AI Factories</div><div>Field notes</div></div>
</div>

---
class: 'dark'
---

<div class="slide-shell">
  <div class="chrome"><div>Field notes</div><ChromeCounter /></div>
  <div class="frame">
    <div style="max-width:1500px">
      <div class="kicker">THE GENERAL LESSON</div>
      <h2 class="t-sm" style="line-height:1.3">Prompt rules are defence in depth.<br/><span class="spark">They are never the mechanism.</span></h2>
      <p class="body" style="margin-top:54px">If your safety story has the words "the agent is instructed not to" in it, you have written a preference, not a control. Find the structural version, or say plainly that you have not got one yet.</p>
    </div>
  </div>
  <div class="foot"><div class="title">AI Factories</div><div>Field notes</div></div>
</div>


---
class: 'dark'
---

<div class="slide-shell">
  <div class="chrome"><div>The result</div><ChromeCounter /></div>
  <div class="frame">
    <div style="max-width:1400px">
      <div class="kicker">THE RESULT</div>
      <h2 class="t-lg">Forty minutes,<br/>one pull request.</h2>
    </div>
  </div>
  <div class="foot"><div class="title">AI Factories</div><div>Closing</div></div>
</div>

<!--
Bring the demo repo up in the browser. Open the pull request the run produced.
Do not scroll to the diff first: walk the evidence chain in order, then the diff last.
-->

---
class: 'dark'
---

<div class="slide-shell">
  <div class="chrome"><div>The result</div><ChromeCounter /></div>
  <div class="frame">
    <div class="split">
      <div>
        <div class="kicker">READ IT IN THIS ORDER</div>
        <h2 class="t-sm">Do not read the code first.</h2>
        <p class="body" style="margin-top:36px">Read the evidence chain. The code is the last thing you check, not the first.</p>
      </div>
      <div class="numbered">
        <div class="row"><div class="n">01</div><div><div class="t">The acceptance criteria</div><div class="d">Written before any code existed.</div></div></div>
        <div class="row"><div class="n">02</div><div><div class="t">The verdict, per criterion</div><div class="d">Pass or fail, each with a pointer into the diff.</div></div></div>
        <div class="row"><div class="n">03</div><div><div class="t">The verification commands</div><div class="d">What was actually run, and what actually came back.</div></div></div>
        <div class="row"><div class="n">04</div><div><div class="t">Then the diff</div><div class="d">Which is usually one line.</div></div></div>
      </div>
    </div>
  </div>
  <div class="foot"><div class="title">AI Factories</div><div>Closing</div></div>
</div>

---
class: 'dark'
---

<div class="slide-shell">
  <div class="chrome"><div>The result</div><ChromeCounter /></div>
  <div class="frame">
    <div style="max-width:1400px">
      <div class="kicker">THE ONLY STEP NOBODY AUTOMATED</div>
      <h2 class="t-lg">Somebody has<br/>to press it.</h2>
      <p class="body" style="margin-top:45px">That is not a limitation waiting to be removed. It is the design.</p>
    </div>
  </div>
  <div class="foot"><div class="title">AI Factories</div><div>Closing</div></div>
</div>

---
class: 'dark'
---

<div class="slide-shell">
  <div class="chrome"><div>Closing</div><ChromeCounter /></div>
  <div class="frame">
    <div style="max-width:1560px">
      <h2 class="t-sm" style="line-height:1.3">Your job stops being<br/><span class="dim">writing the patch</span><br/><br/>and becomes<br/><span class="accent">tuning the line that writes it.</span></h2>
    </div>
  </div>
  <div class="foot"><div class="title">AI Factories</div><div>Closing</div></div>
</div>

---
class: 'dark'
---

<div class="slide-shell">
  <div class="chrome"><div>Closing</div><ChromeCounter /></div>
  <div class="frame">
    <div class="split">
      <div>
        <div class="kicker">THE WORK CHANGES SHAPE</div>
        <h2 class="t-sm">Tightening prompts.<br/>Adding evals.<br/>Removing failure cases.<br/>Deciding which steps<br/>still need a person.</h2>
      </div>
      <div>
        <p class="body">You stop grinding a backlog and start operating a production line.</p>
        <p class="body muted">And the line gets better every week, because the thing you are improving is the line, not the ticket.</p>
        <p class="body spark">Everything else is mechanism. That part is the shift.</p>
      </div>
    </div>
  </div>
  <div class="foot"><div class="title">AI Factories</div><div>Closing</div></div>
</div>

---
class: 'dark'
---

<div class="slide-shell">
  <div class="chrome"><div>Thank you</div><ChromeCounter /></div>
  <div class="frame">
    <div class="split">
      <div>
        <div class="kicker">THANK YOU</div>
        <h2 class="t-lg">Go build<br/>a factory.</h2>
        <p class="body" style="margin-top:45px">Start with the trust lane. The stations are the easy part.</p>
      </div>
      <div class="stack">
        <div class="meta-row" style="flex-direction:column;gap:18px;align-items:flex-start">
          <span class="mist">Slides and demo</span>
          <span class="accent" style="font-size:27px;text-transform:none;letter-spacing:normal">github.com/DaniAkash/ai-factories</span>
        </div>
        <div class="meta-row" style="flex-direction:column;gap:18px;align-items:flex-start;margin-top:36px">
          <span class="mist">Dani Akash</span>
          <span class="spark" style="font-size:27px;text-transform:none;letter-spacing:normal">@dani_akash_</span>
        </div>
      </div>
    </div>
  </div>
  <div class="foot"><div class="title">AI Factories</div><div>End</div></div>
</div>
