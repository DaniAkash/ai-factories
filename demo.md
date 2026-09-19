# Demo runbook

The live half of the talk. Fifteen minutes, on stage, with the factory running on this machine.

Everything below is a real command. Nothing here is staged output.

---

## Before you leave the house

| Check | Command | Expect |
| --- | --- | --- |
| Factory is healthy | `bun run src/index.ts doctor` | gh authenticated, git present, config valid |
| The agent is warm | `bun run src/index.ts run <scratch-repo> <throwaway-issue>` | a full run, so the ACP adapter is downloaded and cached |
| Tests and evals are green | `bun run typecheck && bun run test && bun run eval` | 0 failures |
| Deck builds | `cd slides && bun run build` | built |

**Pre-warming is not optional.** The first use of an ACP agent pulls its adapter through `npx`, which can take ten seconds or more. Do that at home, not on stage.

**Tether to your phone.** The run needs network for both the agent and GitHub. Do a full rehearsal on the tether, not on home wifi.

---

## Stage setup, before you start talking

1. **Terminal at presentation size.** Test it from the back of the room, not on the laptop screen. The demo is fifteen minutes of watching a terminal; if it cannot be read, there is no demo.
2. **Three things open and ready:**
   - a terminal in the aalai directory
   - a browser on the scratch repository's issues page
   - the deck, on the slide before the handoff
3. **The service is already running.** Start it before you walk on:
   ```sh
   bun run service status     # confirm launchd has it
   tail -f ~/.aalai/logs/aalai.out.log
   ```
4. **A clean slate.** See [Resetting between rehearsals](#resetting-between-rehearsals).

---

## The beats

A full four station run takes **four to five minutes**. That is the clock everything else is arranged around: start it early, talk over it, come back.

### 00:00 to 01:00 · It is already running

```sh
bun run service status
bun run src/index.ts status
```

Show that this is a service under `launchd`, not a script you are about to type. `status` lists previous runs and the pull requests they produced.

**Say:** this has been running on my laptop. It polls, it does not receive webhooks, and that is a deliberate choice.

### 01:00 to 02:00 · File a real bug

In the browser, on the scratch repo, open a new issue. Read the title and the expected-versus-actual out loud as you write it.

Use a bug with **a failing test already in the repo**. That matters: it lets the agent confirm the baseline failure before fixing anything, which is the "evidence, not checkmarks" argument happening live rather than being asserted.

### 02:00 · Start the run, and start the clock

```sh
bun run src/index.ts run <owner>/<repo> <issue-number>
```

Do not watch it in silence. Move straight to the next beat.

### 02:00 to 06:30 · Narrate the stations, and open the machinery

The log prints each stage as it happens. Call them out as they appear:

```
workspace   worktree ready            a clean checkout, made for this issue
analyst     tool ...                  it is reading the repo before planning
pipeline    plan ready  criteria=7    seven acceptance criteria, written before any code
implementer tool Editing files        now it writes
pipeline    committed locally         committed, not pushed. nothing has left this machine
workspace   review worktree ready     a second checkout, for the reviewer
reviewer    tool git diff main...     it reads the real diff, not a summary
pipeline    verdict  approve 7/7      judged one criterion at a time
deliver     draft pull request opened
```

While the agent works, open a second pane and show the machinery:

```sh
# the disposable workspace, made for this issue and thrown away after
ls ~/workbench/worktrees/<owner>/<repo>/

# the claim, which is why a double poll is harmless
sqlite3 ~/.aalai/aalai.sqlite \
  "select repo, issue, status, branch from runs order by started_at desc limit 5;"

# the conventions the agent was told to read
cat ~/workbench/worktrees/<owner>/<repo>/aalai-issue-<n>/AGENTS.md
```

**The line to land here:** when the review worktree appears, point at it. Two checkouts of the same branch. The reviewer reads what was committed, from a directory the implementer never touched.

### 06:30 to 10:00 · The pull request, read in order

Open the draft PR. **Do not scroll to the diff first.** Read it top to bottom and say why:

1. **The problem**, restated by the planning station
2. **The approach**, and the alternative it rejected
3. **The acceptance criteria table**, one row per criterion, each with a pass or fail and the evidence for it
4. **The verification commands** and what they actually returned
5. **Then** the diff, which is usually one line

**Say:** the reason to trust that one line is everything above it. And notice the reviewer ran its own probes, not just the implementer's tests.

Point out that it is a **draft**. Marking it ready is a person's job, and merge is not in the factory's tool surface at all.

### 10:00 to 11:30 · The gate says no

Show a run that is refused before anything starts. Set a label requirement:

```sh
# in aalai.config.json, set:  "requireLabel": "aalai"
bun run src/index.ts run <owner>/<repo> <an-unlabelled-issue>
```

```
error  issue rejected by intake policy  reason="missing label \"aalai\""
```

Nothing was cloned. No agent ran. The decision happened at the door, from the API response, before any model saw anything.

**Say:** the same gate refuses an issue opened by someone the repository does not trust. I cannot fake that on stage, so here is the test that proves it.

### 11:30 to 14:00 · Tune the line

This is the closing beat, and it is the reframe made literal.

```sh
bun run eval
```

Thirty cases, all green. Then **break a rule on stage**. In `src/watch/intake.ts`, add `'NONE'` to `TRUSTED_ASSOCIATIONS`:

```diff
 export const TRUSTED_ASSOCIATIONS: ReadonlySet<string> = new Set([
   'OWNER',
   'MEMBER',
   'COLLABORATOR',
+  'NONE',
 ])
```

```sh
bun run eval
```

```
(fail) an untrusted author never gets a run > a stranger opening an issue is refused
```

Remove the line, run it again, green.

**Say:** I changed one line of policy and the suite told me immediately what moved. That is the difference between a factory you tune and a factory you hope about.

### 14:00 to 15:00 · Hand back

Leave the pull request on screen. Return to the deck.

---

## Fallbacks

| If | Then |
| --- | --- |
| The run takes longer than expected | You have slack: beats at 10:00 and 11:30 are droppable, in that order. Never cut into the slides after the demo. |
| The agent produces no diff | That is a designed outcome, not a failure. aalai comments on the issue and opens nothing. Show that, and say so: no diff, no pull request. |
| The reviewer requests changes | Better than a clean pass. Let it run the revision. It is bounded at two, and the loop is the point. |
| The run fails outright | Use it. Switch to the recorded run, and say that a demo proves the pipeline and never its failure modes. That is the next slide anyway. |
| Network dies | Play `demo/recording.cast` or the recorded video. Have it one keystroke away. |
| Anything hangs | `Ctrl-C`. The claim carries a lease, so the issue is not stuck: it becomes runnable again after `staleClaimMinutes`. Or `bun run src/index.ts forget <repo> <issue>` to clear it now. |

---

## Resetting between rehearsals

A run records a claim, pushes a branch, and opens a PR. To rehearse the same issue again:

```sh
# 1. let the factory forget the run
bun run src/index.ts forget <owner>/<repo> <issue-number>

# 2. close the pull request and delete its branch
gh pr close <pr-number> --repo <owner>/<repo> --delete-branch

# 3. reopen the issue
gh issue reopen <issue-number> --repo <owner>/<repo>

# 4. clear any worktrees left from a failed run
git -C ~/workbench/<owner>/<repo> worktree prune
```

For a fresh bug instead of a replay, add a new broken helper with a failing test, push it, and file a new issue. That is the cleanest rehearsal loop and it is what every run in the repository's history was built on.

---

## Commands, all in one place

```sh
bun run src/index.ts doctor                       # health check
bun run src/index.ts run <owner>/<repo> <n>       # one issue, now
bun run src/index.ts status                       # recent runs
bun run src/index.ts forget <owner>/<repo> <n>    # clear a run record
bun run once                                      # one polling pass
bun start                                         # the watch loop, foreground
bun run service install | status | uninstall      # launchd
bun run eval                                      # the eval suite
bun run test                                      # unit tests
```
