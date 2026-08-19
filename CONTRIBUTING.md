# Contributing

## Adding an entry

Open a pull request adding one line to the relevant section:

```markdown
- [name](https://github.com/owner/repo) — what it is, in one line.
```

Guidelines, none of them surprising:

- **Working software.** Not announcements, not roadmaps, not a README for a
  thing that does not exist yet.
- **Say what it is, not what it aspires to be.** "A Go client library" beats
  "the fastest, most secure LNURLcash library".
- **A hyphen, and a full stop.** awesome-lint requires ` - ` between the link
  and its description, and rejects an en or em dash there, so the whole list
  uses a plain hyphen.
- **Reference implementations stay first** in their section. Newcomers should
  reach dni's mint and wallet before anything else.
- **Each link appears exactly once** in the list. awesome-lint rejects a
  repeated URL, so an entry belongs in its own section rather than being
  cross-listed; link a deeper page if you need to point somewhere twice.

## Removing an entry

Projects that no longer build, or that have been archived for a year, are
better removed than left to mislead. Open a pull request saying so.

## Scope

Things that implement, test, or directly support LNURLcash. Not general
Lightning tooling — there are excellent lists for that already.
