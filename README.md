# Automate your software delivery with AI Factories

Slides and demo for the talk **"Automate your software delivery with AI Factories"** by [Dani Akash](https://github.com/DaniAkash).

> An AI software factory is a delivery pipeline where every stage is an agent: one triages the issue, one writes the spec, one implements it, one reviews it, and a human still owns the merge.

**Event:** TBA

**Recording:** TBA

## Repo layout

```
.
├── slides/     Slidev deck for the talk
└── design.md   Style reference: tokens, components, do's and don'ts
```

## Run the deck locally

```sh
cd slides
bun install
bun run dev
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

The demo is a factory running live on the speaker's own machine: an issue filed on stage at the start, and a reviewed draft pull request read together at the end.

## Speaker

**Dani Akash** - Founding Engineer at [BrowserOS](https://browseros.com).
