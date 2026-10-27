# skills

Agent skills I use to build software with coding agents. Each folder is one skill: a `SKILL.md` in the format Claude Code, Codex and other agents load.

## Install

```bash
npx skills@latest add Floriferous/skills
```

Or copy a skill's folder into `~/.claude/skills/` or a project's `.claude/skills/`.

## Skills

**Model-invoked** skills are reached for by the agent when the task fits; you can also type them. **User-invoked** skills run only when you type them.

### [`feedback-loops`](feedback-loops/SKILL.md) (model-invoked)

> "Dead programs tell no lies."
>
> Andrew Hunt and David Thomas, _The Pragmatic Programmer_

**The problem.** Agents make software look resilient: a fallback here, a default value there, a caught error that gets logged and ignored. Each one turns a bug into wrong behaviour that surfaces later, somewhere else. You end up digging through plausible-looking output to find what broke, and the agent can't learn from a failure it never sees.

**The fix.** Classify every failure path, every time. A defect, meaning the code, its configuration or its environment is not what was assumed, crashes where it is detected, with a message that names the cause. That's about 99% of failure paths. Only an expected failure, something the world may do to correct code such as a network timeout, gets a bounded, visible retry or fallback.

### [`dependency-gardening`](dependency-gardening/SKILL.md) (model-invoked)

**The problem.** A version bump and a changelog review can leave outdated patterns, unnecessary wrappers, and poor library usage untouched.

**The fix.** Reassess the selected library's entire adoption against current documentation and capabilities. Upgrade, simplify, remove obsolete workarounds, and account for every usage—including what should stay as it is.

### [`keyword-first-seo`](keyword-first-seo/SKILL.md) (model-invoked)

> "You'll be spending months on this keyword."
>
> Danny Postma

**The problem.** A product launched without an audience gets no visitors. Agents asked about SEO tend to suggest generic checklists such as meta tags, blog posts and sitemaps, which never decide what to build.

**The fix.** Put the keyword first. Choose one search term with low difficulty, enough volume and many long-tail children, then build the product, its pages and its launch around it. Earn the first backlinks with a Product Hunt launch, then wait. This follows [Danny Postma's thread](https://x.com/dannypostma/status/1646368426246680579), updated for Google's current spam policies.

## License

[MIT](LICENSE)
