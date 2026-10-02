# thiruvalluvar

A Claude Code skill that cuts token use without lowering answer quality or reasoning depth.

It is named after Thiruvalluvar, the weaver-poet who put a whole philosophy into 1,330 couplets of
seven words each: say everything that matters, and nothing that does not.

## What it does

Most "save tokens" skills shorten the AI's sentences. Our tests showed two problems with that:

- **Shorter sentences save little.** Most tokens in a working session come from reading files and
  from wasted steps, not from the reply text.
- **Broken grammar costs quality.** Dropping small words made answers harder to read and lowered
  their scores.

thiruvalluvar targets the waste instead:

- **Work rules:** verify before building, stop retrying after two failures, search large files
  instead of paging through them, never re-read unchanged files, and treat overwrites as
  destructive.
- **Reply rules:** short plain bullets (what happened, what we got, what you need to know), in
  complete sentences. There is no recap and no filler, and the reply never restates the request.
- **Thinking stays full.** The skill spends thinking on the task, never on polishing the reply.

## Modes

| Mode | For | Reply |
|---|---|---|
| `nod` (default) | most people | the overall picture: what happened and why |
| `grunt` | people who only want the output | the result, plus at most two lines needed to use it safely |

Switch with `/thiruvalluvar nod` or `/thiruvalluvar grunt`. The user's words always outrank the
mode: say you are new, or ask "why", and you get a full explanation.

## Install (Claude Code)

1. Copy the `skills/thiruvalluvar` folder into `~/.claude/skills/` (on Windows:
   `C:\Users\<you>\.claude\skills\`).
2. To use it for every session, add this to `~/.claude/CLAUDE.md`:

   ```markdown
   Default output style: the `thiruvalluvar` skill in mode nod, from the first reply of every
   session. Switch with /thiruvalluvar grunt or /thiruvalluvar nod.
   ```

3. Or use it on demand by typing `/thiruvalluvar` in a session.

Other AI coding agents that read an `AGENTS.md` file can paste the body of `SKILL.md` into it.

## Benchmark results

We ran 9 everyday requests in one ongoing conversation per setup, on the same model. The
requests were: explain a concept to a beginner, fix a Python error, write a function, delete old
files safely, answer a vague request, write an email, fix a build error in a project, find a value
in a 1,168-line config file, and answer a quick fact. Graders scored every answer blind (1–10),
with answers shuffled under random IDs.

| Setup | Quality (out of 10) | Conversation tokens after 9 requests | Words written |
|---|---|---|---|
| No skill | 8.4 | 111,339 | 3,153 |
| **thiruvalluvar nod** | **8.7** | **83,763 (−25%)** | 1,976 (−37%) |
| thiruvalluvar grunt | 8.1 | 80,432 (−28%) | 1,110 (−65%) |

For comparison, a popular "talk like a caveman" style skill scored 7.6 on the same requests in
our earlier single-question round, against 8.7 for no skill. It wrote 63% fewer words but used
more total tokens than no skill in that round.

**Limits, stated honestly:**

- Each setup ran one conversation, so quality differences of about 0.3 are within noise. The token
  savings were large and appeared in every round.
- The graders were AI models, not people.
- The results come from one model (Claude). Other models may behave differently.
- A newer vague-request rule ("ask the two or three questions that decide the answer") was added
  after the last benchmark round.

See [benchmark/TASKS.md](benchmark/TASKS.md) for the requests and grading rubric, so you can
repeat the test.

## License

MIT. See [LICENSE](LICENSE).
