# Demo runbook

The live half of the talk. Fifteen minutes, on stage, with the factory running on this machine.

Everything below is a real command. Nothing here is staged output.

The factory serves a dashboard of its own, and that is what the audience watches: each station drawn as a station, and the acceptance criteria being answered one at a time. The terminal stays open beside it, because the log is the thing that proves the screen is not a mockup.

---

## Before you leave the house

| Check | Command | Expect |
| --- | --- | --- |
| Factory is healthy | `bun run src/index.ts doctor` | gh authenticated, git present, config valid |
| Dashboard is built | `bun run ui:install && bun run ui:build` | `ui/dist` written |
| The agent is warm | `bun run src/index.ts run <scratch-repo> <throwaway-issue>` | a full run, so the ACP adapter is downloaded and cached |
| Tests and evals are green | `bun run typecheck && bun run test && bun run eval` | 0 failures |
| Deck builds | `cd slides && bun run build` | built |

**Build the dashboard, do not run it from Vite.** The service serves `ui/dist` itself, so there is one process and one port. A dev server is a second thing to start and a second thing to fail.

**Pre-warming is not optional.** The first use of an ACP agent pulls its adapter through `npx`, which can take ten seconds or more. Do that at home, not on stage.

**Tether to your phone.** The run needs network for both the agent and GitHub. Do a full rehearsal on the tether, not on home wifi.

---

## Stage setup, before you start talking

1. **Start the factory.** It serves the dashboard on the same port.
   ```sh
   bun start
   ```
2. **Browser on `http://localhost:4173`**, full screen, with `present` toggled on. Test it from the back of the room.
3. **A terminal beside it**, at presentation size, tailing the log:
   ```sh
   tail -f ~/.aalai/logs/aalai.out.log
   ```
4. **A second browser tab** on the scratch repository's issues page.
5. **A clean slate.** See [Resetting between rehearsals](#resetting-between-rehearsals).

The dashboard carries the story and the terminal carries the proof. When you want the audience to believe the screen, point at the log.

---

## The beats

A full four station run takes **four to five minutes**. That is the clock everything else is arranged around: start it early, talk over it, come back.

### 00:00 to 01:00 · It is already running

Dashboard on screen, sitting on **Waiting for work**, stations all dashed and queued.

```sh
bun run src/index.ts status
```

Show that this is a service under `launchd`, not a script you are about to type. Hit `history` on the dashboard: previous runs and the pull requests they produced.

**Say:** this has been running on my laptop. It polls, it does not receive webhooks, and that is a deliberate choice.

### 01:00 to 02:00 · File a real bug

In the browser, on the scratch repo, open a new issue. Read the title and the expected-versus-actual out loud as you write it.

Use a bug with **a failing test already in the repo**. That matters: it lets the agent confirm the baseline failure before fixing anything, which is the "evidence, not checkmarks" argument happening live rather than being asserted.

### 02:00 · Start the run, and start the clock

```sh
bun run src/index.ts run <owner>/<repo> <issue-number>
```

Switch to the dashboard. The title fills in and `workspace` lights violet.

Do not watch it in silence. Move straight to the next beat.

### 02:00 to 06:30 · Watch the line, and open the machinery

The dashboard narrates itself. Call out what changes as it happens:

```
workspace    lights, then settles to the branch name
analyst      lights. Its commands scroll in the right-hand column.
             ▸ ACCEPTANCE CRITERIA fills in on the left ◂
implementer  lights. The criteria sit there, unanswered.
             ▸ the handoff under the arrow reads "a commit, on its own checkout"
reviewer     lights. Its first command is git diff against the branch.
             ▸ the criteria tick off, one at a time, each with its evidence
deliver      draft pull request
```

**The line to land**, when the criteria appear: those were written before any code existed, and the station that writes the code never sees them as negotiable.

**The second line**, when they start ticking: same list, now answered against the real diff.

While the agent works, drop to the terminal and show the machinery:

```sh
# the disposable workspace, made for this issue and thrown away after
ls ~/workbench/worktrees/<owner>/<repo>/

# the claim, which is why a double poll is harmless
sqlite3 ~/.aalai/aalai.sqlite \
  "select repo, issue, status, branch from runs order by started_at desc limit 5;"
```

Two checkouts appear for the same issue while the reviewer runs: the implementer's, and the reviewer's own. Point at that. The reviewer reads what was committed, from a directory the implementer never touched.

### 06:30 to 10:00 · The pull request, read in order

Click through from the dashboard footer. **Do not scroll to the diff first.** Read it top to bottom and say why:

1. **The problem**, restated by the planning station
2. **The approach**, and the alternative it rejected
3. **The acceptance criteria table**, one row per criterion, each with a pass or fail and the evidence for it
4. **The verification commands** and what they actually returned
5. **Then** the diff, which is usually one line

**Say:** the reason to trust that one line is everything above it. And notice the reviewer ran its own probes, not just the implementer's tests.

Point out that it is a **draft**. Marking it ready is a person's job, and merge is not in the factory's tool surface at all.

### 10:00 to 11:30 · The gate says no

Back on the dashboard. Set a label requirement and file an issue without it:

```sh
# in aalai.config.json, set:  "requireLabel": "aalai"
```

File an unlabelled issue on the scratch repo and wait one poll tick. It appears in the dashboard footer under **turned away at the door**, in amber, with the reason.

Nothing was cloned. No agent ran. The decision happened at the door, from the API response, before any model saw anything.

**Say:** the same gate refuses an issue opened by someone the repository does not trust. I cannot fake that on stage, so here is the test that proves it.

### 11:30 to 14:00 · Tune the line

This is the closing beat, and it is the reframe made literal.

```sh
bun run eval
```

Forty cases, all green. Then **break a rule on stage**. In `src/watch/intake.ts`, add `'NONE'` to `TRUSTED_ASSOCIATIONS`:

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
| The dashboard will not start | The factory does not care. It logs a warning and runs anyway. Drive the whole demo from the terminal; every beat above has a log line behind it. |
| The browser shows nothing | Check the toggle in the top right. `recorded` means the service has sent no events; `live ·` means it is connected. A recorded run is a working fallback on its own. |
| The run takes longer than expected | You have slack: beats at 10:00 and 11:30 are droppable, in that order. Never cut into the slides after the demo. |
| The agent produces no diff | That is a designed outcome, not a failure. The dashboard shows **stopped** with the reason, and no pull request is opened. Show that, and say so. |
| The reviewer requests changes | Better than a clean pass. The station lights again for the revision. It is bounded at two, and the loop is the point. |
| The run fails outright | Use it. A demo proves the pipeline and never its failure modes, which is the next slide anyway. |
| Network dies | Toggle to `recorded` and walk the recorded run. Have it on screen before you need it. |
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
bun start                                         # factory and dashboard, one process
bun run src/index.ts run <owner>/<repo> <n>       # one issue, now
bun run src/index.ts status                       # recent runs
bun run src/index.ts forget <owner>/<repo> <n>    # clear a run record
bun run src/index.ts once                         # one polling pass
bun run ui:install                                # once
bun run ui:build                                  # produce ui/dist for the service to serve
bun run service install | status | uninstall      # launchd
bun run eval                                      # the eval suite
bun run test                                      # unit tests
```

## The dashboard, in one paragraph

Each station is a rule whose style carries its state: dashed is queued, solid violet is working, thin white is done. The label under each arrow is what that station hands to the next one. The left column holds the acceptance criteria from the moment the analyst writes them until the reviewer answers them, which is the only element that persists across three stations. `present` drops to a projector-sized layout, `history` lists what previous runs produced, and issues the gate turned away appear in the footer.
