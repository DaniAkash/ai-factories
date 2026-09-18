# Automate your software delivery with AI Factories

Slides and demo for the talk **"Automate your software delivery with AI Factories"** by [Dani Akash](https://github.com/DaniAkash).

> An AI software factory is a delivery pipeline where every stage is an agent: one triages the issue, one writes the spec, one implements it, one reviews it, and a human still owns the merge.

**Event:** TBA

**Recording:** TBA

## Repo layout

```
.
├── slides/
│   ├── slides.md         30 minute cut, 22 slides  (the default)
│   └── slides-45min.md   45 minute cut, 61 slides
└── design.md             Style reference: tokens, components, do's and don'ts
```

Two cuts, because the talk is given at two lengths. The **30 minute cut** splits 15 minutes of slides around a 15 minute live demo: the demo shows the mechanism, so the slides argue the case rather than explaining how it works. The **45 minute cut** carries the full anatomy on slides and uses the demo as a bracket.

## Run the deck locally

```sh
cd slides
bun install
bun run dev                    # the 30 minute cut
bunx slidev slides-45min.md    # the 45 minute cut
```

Built with [Slidev](https://sli.dev). Design tokens live in [`design.md`](./design.md).

## What the talk covers

The pattern is already running in the open: Vercel operates a factory on the AI SDK repository, where it authors roughly a third of merged pull requests and maintainers approve every one of them.

Reading that factory line by line turns up something the usual diagram hides. The conveyor belt of stations is about a third of the code. The rest is trust enforcement and memory namespacing, which is what makes a factory safe to leave running unattended.

Topics include:

- Why station output schemas are contracts, and why acceptance criteria are written before any code exists
- Reviewer independence as three mechanical facts rather than a prompt
- Deciding trust once, at the door, from data the model never sees
- Placing the human gate where reversal is expensive, and deleting the capability instead of guarding it
- Bounding retry loops durably across stateless invocations
- Keeping untrusted input out of any memory that feeds future runs
- Substituting every hosted primitive with something that runs on a laptop
- Why an end-to-end demo validates the pipeline and never its failure modes

The demo is a factory running live on the speaker's own machine. In the 30 minute cut it is half the talk: an issue filed on stage, the worktree and the claim table opened while the run works, the pull request read as an evidence chain, the trust gate refusing an untrusted issue, and a rule changed on stage to watch the behaviour move.

The twelve invariants the talk draws on are listed in the 45 minute deck and in the anatomy section above.

## Speaker

**Dani Akash** - Founding Engineer at [BrowserOS](https://browseros.com).
