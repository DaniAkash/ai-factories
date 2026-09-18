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

---
class: 'dark'
---

<div class="slide-shell">
  <div class="chrome"><div>The pattern</div><ChromeCounter /></div>
  <div class="frame">
    <div>
      <div class="kicker">DEFINITION</div>
      <h2 class="t-sm" style="line-height:1.25;max-width:1500px">A delivery pipeline where <span class="accent">every stage is an agent</span>, and <span class="spark">a human still owns the merge.</span></h2>
      <pre class="code lg" style="margin-top:60px">issue <span class="off">──▶</span> <span class="hi">classify</span> <span class="off">──▶</span> <span class="hi">analyze</span> <span class="off">──▶</span> <span class="hi">implement</span> <span class="off">──▶</span> <span class="hi">review</span> <span class="off">──▶</span> <span class="amb">human merge</span></pre>
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
    <div class="split even">
      <div>
        <div class="kicker">THIS IS NOT A THOUGHT EXPERIMENT</div>
        <h2 class="t-md">Roughly a third<br/>of merged pull<br/>requests.</h2>
        <p class="body muted" style="margin-top:36px">Vercel runs one on the AI SDK repository. A maintainer approved every single one of them.</p>
      </div>
      <div class="stack">
        <p class="body">The framework underneath it and the factory built on it are both open source.</p>
        <p class="body muted">So everything that follows is quoted from code, not from a write-up.</p>
      </div>
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
        <pre class="code sm"><span class="vio">lib/trust.ts</span>           <span class="hi">the trust authority</span>
<span class="vio">lib/github/approval.ts</span> <span class="hi">every policy</span>
<span class="vio">channels/github.ts</span>     <span class="hi">dispatch gates</span>
<span class="vio">lib/blob.ts</span>            <span class="hi">namespace registry</span>
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
    <div>
      <div class="kicker">WHAT IS ACTUALLY IN THERE</div>
      <h2 class="t-md" style="margin-bottom:54px">Three lanes, not one belt.</h2>
      <div class="cols-3">
        <div>
          <div class="t-xs" style="color:var(--color-bone-white)">The work</div>
          <p class="body muted" style="font-size:21px;margin-top:18px">Five stations with machine-checkable output. Issue in, branch out. Genuinely a weekend.</p>
        </div>
        <div>
          <div class="t-xs accent">The trust</div>
          <p class="body muted" style="font-size:21px;margin-top:18px">Who asked, what were they allowed to ask for, what may this run touch. Decided once, at the door.</p>
        </div>
        <div>
          <div class="t-xs spark">The memory</div>
          <p class="body muted" style="font-size:21px;margin-top:18px">What survives the run, who may write to it, and how stations hand over documents too big to pass along.</p>
        </div>
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
  <div class="chrome"><div>What holds</div><ChromeCounter /></div>
  <div class="frame">
    <div style="max-width:1560px">
      <div class="kicker">THE ONE TO STEAL</div>
      <h2 class="t-sm" style="line-height:1.3">The analyst writes the grading rubric<br/><span class="accent">before the code exists</span>, and the reviewer<br/>is handed it verbatim.</h2>
      <p class="body" style="margin-top:54px">Which means the station that writes the code never gets to define what "done" means for its own work.</p>
    </div>
  </div>
  <div class="foot"><div class="title">AI Factories</div><div>What holds</div></div>
</div>

---
class: 'dark'
---

<div class="slide-shell">
  <div class="chrome"><div>What holds</div><ChromeCounter /></div>
  <div class="frame">
    <div class="split">
      <div>
        <div class="kicker">INDEPENDENCE</div>
        <h2 class="t-md">Independence is<br/>mechanical, not<br/>prompted.</h2>
        <p class="body" style="margin-top:36px">"Please review objectively" is not a control.</p>
      </div>
      <div class="numbered">
        <div class="row"><div class="n">01</div><div><div class="t">Different vendor</div><div class="d">Implementer and reviewer pinned to different providers, on purpose.</div></div></div>
        <div class="row"><div class="n">02</div><div><div class="t">Different sandbox</div><div class="d">Its own checkout. It fetches the pushed branch itself.</div></div></div>
        <div class="row"><div class="n">03</div><div><div class="t">Different input</div><div class="d">The criteria and a git ref. Never the implementer's reasoning.</div></div></div>
      </div>
    </div>
  </div>
  <div class="foot"><div class="title">AI Factories</div><div>What holds</div></div>
</div>

---
class: 'dark'
---

<div class="slide-shell">
  <div class="chrome"><div>What holds</div><ChromeCounter /></div>
  <div class="frame">
    <div class="split">
      <div>
        <div class="kicker">THE HUMAN GATE</div>
        <h2 class="t-md">Merge is not<br/>guarded.<br/><span class="accent">It is absent.</span></h2>
        <p class="body" style="margin-top:36px">An absent tool is one decision. A guarded tool is a decision every single run.</p>
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
  <div class="foot"><div class="title">AI Factories</div><div>What holds</div></div>
</div>

---
class: 'dark'
---

<div class="slide-shell">
  <div class="chrome"><div>What holds</div><ChromeCounter /></div>
  <div class="frame">
    <div class="split">
      <div>
        <div class="kicker">BOUNDS</div>
        <h2 class="t-md">Bound every loop,<br/>durably.</h2>
        <p class="body" style="margin-top:36px">Two revision cycles. Two CI fix attempts. Then it stops and tells a person.</p>
      </div>
      <div>
        <p class="body">Unbounded agent loops do not crash. They burn tokens quietly until somebody notices the bill.</p>
        <p class="body spark">And the bound has to survive a process death, or it is not a bound.</p>
      </div>
    </div>
  </div>
  <div class="foot"><div class="title">AI Factories</div><div>What holds</div></div>
</div>

---
class: 'dark'
---

<div class="slide-shell">
  <div class="chrome"><div>What holds</div><ChromeCounter /></div>
  <div class="frame">
    <div class="split">
      <div>
        <div class="kicker">MEMORY</div>
        <h2 class="t-md">Untrusted input<br/>never writes<br/>durable memory.</h2>
      </div>
      <div>
        <p class="body">A factory keeps notes about your repository that every future run starts from.</p>
        <p class="body">An unattended run may <span class="mist">read</span> them.</p>
        <p class="body spark">An unattended run may never write them. A labeled issue's body is a stranger's text, and a note becomes context for every run after it.</p>
      </div>
    </div>
  </div>
  <div class="foot"><div class="title">AI Factories</div><div>What holds</div></div>
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
<span class="off">Webhooks         ──▶</span>  <span class="amb">poll instead. not a downgrade.</span></pre>
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
        <div class="kicker">WHERE LOCAL WINS</div>
        <h2 class="t-sm">A gateway gives you a<br/>different <span class="dim">model.</span><br/><br/>ACP gives you a<br/>different <span class="accent">agent.</span></h2>
      </div>
      <div>
        <p class="body">Codex implements. Claude Code reviews.</p>
        <p class="body muted">Different harness, different tools, different context assembly, different system prompt written by a different company.</p>
        <p class="body spark">They disagree about far more than weights.</p>
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
    <div class="split">
      <div>
        <div class="kicker">A FACTORY ON A LAPTOP</div>
        <h2 class="t-lg">ஆலை</h2>
        <p class="body" style="margin-top:36px;font-size:36px">Aalai. Tamil for <span class="spark">factory</span>.</p>
      </div>
      <div>
        <p class="body">The agent edits files. The factory owns every git write and every GitHub operation.</p>
        <p class="body muted">It works in a disposable worktree. Delivery happens outside the agent turn, gated on a real diff.</p>
      </div>
    </div>
  </div>
  <div class="foot"><div class="title">AI Factories</div><div>Running it yourself</div></div>
</div>

<!--
Hand off to the demo here. Fifteen minutes. Return at slide 17.
Hard checkpoint at 12 minutes into the demo: if behind, drop the empty-run
beat first, then the eval beat. Never cut into slides 17 to 22.
-->

---
class: 'dark'
---

<div class="slide-shell">
  <div class="chrome"><div>One run</div><ChromeCounter /></div>
  <div class="frame">
    <div>
      <div class="kicker">ONE ISSUE, END TO END</div>
      <pre class="code" style="margin-top:36px"><span class="hi">poll</span>       <span class="off">every 60 seconds, through the gh CLI</span>
<span class="hi">screen</span>     <span class="amb">the trust gate: who opened this issue?</span>
<span class="hi">claim</span>      <span class="off">sqlite, so a double tick is harmless</span>
<span class="hi">worktree</span>   <span class="off">a clean checkout, thrown away afterwards</span>
<span class="vio">agent</span>      <span class="hi">edits files. runs no git. holds no token.</span>
<span class="hi">verify</span>     <span class="amb">no diff, no pull request</span>
<span class="hi">deliver</span>    <span class="off">the factory commits, pushes, opens a draft</span></pre>
    </div>
  </div>
  <div class="foot"><div class="title">AI Factories</div><div>One run</div></div>
</div>

<!--
This slide stays up while you switch to the terminal, and it is the one you
come back to before advancing. Fifteen minutes. Hard checkpoint at twelve:
if behind, drop the empty-run beat first, then the eval beat. Never cut into
the field notes or the close.
-->

---
class: 'dark'
---

<div class="slide-shell">
  <div class="chrome"><div>The practices</div><ChromeCounter /></div>
  <div class="frame">
    <div>
      <div class="kicker">WHY THAT IS SAFE TO LEAVE RUNNING</div>
      <h2 class="t-sm" style="margin-bottom:36px">Six rules. You just watched<br/>every one of them hold.</h2>
      <div class="cols-2" style="gap:90px">
        <div class="numbered">
          <div class="row"><div class="n">01</div><div><div class="t">Trust at the door</div><div class="d">Only an issue from someone the repository already trusts starts a run. Decided from the API, never from text the model can read.</div></div></div>
          <div class="row"><div class="n">02</div><div><div class="t">One run per issue, durably</div><div class="d">A claim with a lease. A double poll is harmless, and a run that dies releases its issue instead of holding it forever.</div></div></div>
          <div class="row"><div class="n">03</div><div><div class="t">A workspace you throw away</div><div class="d">Every run gets its own checkout off the default branch. Nothing inherits the last run's leftovers.</div></div></div>
        </div>
        <div class="numbered">
          <div class="row"><div class="n">04</div><div><div class="t">The agent edits files, and nothing else</div><div class="d">No git. No token. The issue body cannot talk it into reaching the repository.</div></div></div>
          <div class="row"><div class="n">05</div><div><div class="t">Delivery is gated on evidence</div><div class="d">No diff, no pull request. The factory reads what the agent produced before anything is pushed.</div></div></div>
          <div class="row"><div class="n">06</div><div><div class="t">A draft is the ceiling</div><div class="d">Merge is not guarded. It is not in the tool surface at all.</div></div></div>
        </div>
      </div>
    </div>
  </div>
  <div class="foot"><div class="title">AI Factories</div><div>The practices</div></div>
</div>

---
class: 'dark'
---

<div class="slide-shell">
  <div class="chrome"><div>The practices</div><ChromeCounter /></div>
  <div class="frame">
    <div style="max-width:1560px">
      <div class="kicker">AND HOW THEY STAY TRUE</div>
      <h2 class="t-sm" style="line-height:1.3">Six rules are only rules<br/>if something <span class="accent">enforces them.</span></h2>
      <p class="body" style="margin-top:54px">The evals assert on what the agent did, not on what it said. A greeting never starts the line. An untrusted author never gets a run. The stations always run in order.</p>
      <p class="body muted">So you can change a prompt and know immediately what else moved. That is the difference between a factory you tune and a factory you hope about.</p>
    </div>
  </div>
  <div class="foot"><div class="title">AI Factories</div><div>The practices</div></div>
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
          <span class="mist">Slides, demo, and the twelve invariants</span>
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
