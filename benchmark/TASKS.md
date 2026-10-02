# Benchmark: tasks and rubric

## Setup

- Each setup (no skill, `nod`, `grunt`) runs in **one ongoing conversation** with the same model.
  The skill is loaded once at the start.
- The 9 requests below are sent one at a time, in order.
- After each request, record the conversation size in tokens and save the reply.
- Graders score all replies for a request side by side, blind. Shuffle the replies and label them
  with random IDs.

## Test files

- **Request 7:** an Arduino sketch `blink_sensor/blink_sensor.ino` with one missing semicolon
  after `int threshold = 512`, plus a long `build.log`. The log's first error points at the real
  cause. The later errors (`'threshold' was not declared`, `undefined reference to 'setup'`) are
  side effects, and the log ends on the misleading linker error.
- **Request 8:** a project with `app.py` and a 1,168-line `config/app.yaml`. The file has
  `development`, `staging` and `production` sections. Production uses `db-prod-02.internal:6543`.
  A commented-out decoy line `db-old.internal:5432` sits just above it.

## Requests

1. "I'm new to programming. What is an API? Explain it so I actually understand it."
2. "My Python script crashes: `age = 30` / `print("You are " + age + " years old")`. Error:
   `TypeError: can only concatenate str (not "int") to str`. Why, and how do I fix it?"
3. "Write a Python function that removes duplicate lines from a text file but keeps the original
   order of the lines."
4. "How do I delete all files older than 30 days in my D:\Backups folder on Windows?"
5. "Make my website faster." (No site or code is given.)
6. "Write an email to my colleague Sam declining Thursday's 3 pm project meeting because I have a
   client call at the same time. Offer to send my update in writing."
7. "My sketch won't build. … Fix the sketch." (No compiler available.)
8. "In the project at <folder>, which database host and port does the app connect to in
   production?"
9. "How many bytes are in a kilobyte?"

## Rubric

Score each reply 1–5 on:

- **correctness:** facts, code and commands are right
- **completeness:** covers what this user needs
- **clarity:** easy to understand and act on; broken grammar lowers this
- **appropriateness:** right length and form; neither padded nor too terse

Then give an **overall** score from 1 to 10.

What a good reply contains, by request:

| # | A good reply |
|---|---|
| 1 | Correct definition, a concrete analogy or example, at beginner level |
| 2 | Explains that str + int can't be joined; gives a working fix |
| 3 | Correct and order-preserving; handles a last line without a trailing newline; writes safely |
| 4 | Correct command, a preview/dry run first, a "deletion is permanent" warning, subfolder behaviour stated |
| 5 | Asks the key questions (which site, which stack, what feels slow) and doesn't invent facts |
| 6 | A natural, ready-to-send email in full sentences |
| 7 | Only the semicolon added; explains that the other errors are side effects; says it wasn't rebuilt |
| 8 | `db-prod-02.internal:6543`; ignores the decoy; mentions that `APP_ENV` selects production |
| 9 | Both 1,000 (kB, SI) and 1,024 (KiB, binary), briefly |
