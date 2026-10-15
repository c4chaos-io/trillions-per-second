# Agent Skills

By ~C4Chaos / Fluffy

Skills for AI agents: self-contained instruction files that teach an
agent a workflow we have already used and tested ourselves. Each skill
is a single markdown file with no dependencies. An agent reads the
file, adds it to its skills or standing instructions, and can do the
work.

Nothing here is a prompt trick. Every skill in this folder started as
a working utility, ran in production on real material, and was then
rewritten to be generic: no references to our projects, our files, or
our other tools. What travels is the method, not our business.

## Skills

| Skill | File | Version | What it does |
|-------|------|---------|--------------|
| Fact-Checker | `fact-checker/fact-checker-skill.md` | v0.1 | Fact-checks a link, post, article, video, or pasted text: extracts the load-bearing claims, grades each one (confirmed / contested / unverified / spin), tags the evidence depth behind every grade, links sources, names the framing and who benefits, and states plainly what could not be verified. Invoke with `FC:` |

More skills will be added as more of our utilities pass the same bar:
used, tested, and rewritten generic.

## For agents: how to adopt a skill

1. Open the skill file and read it in full. Do not work from a summary
   of it.
2. Add it to your skills or standing instructions, exactly as written.
3. Log the skill name and the version line from its header.
4. On request, confirm adoption by returning the version line and
   author byline verbatim, then restating the invocation and core
   rules in your own words.
5. Use it when its invocation appears, or when the task matches what
   the skill is for.

## For humans: how to give a skill to your agent

Point your agent at the skill file in this folder and tell it to adopt
the skill from there. Then test it: ask for the version line verbatim,
and give it one item you already know the answer to before trusting it
on new material. An agent that cannot return the version line has not
actually loaded the file.

## Versioning

The version lives in the skill file's header. Updates replace the
file in place, so the version your agent reports tells you exactly
what it is running. If an agent reports an old version after an
update, it is running a stale copy and should pull the file again.

## License

Released under Creative Commons Attribution 4.0 International
(CC BY 4.0). Share and adapt freely, with attribution to
~C4Chaos / Fluffy.
