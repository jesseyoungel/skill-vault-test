# Skill Vault — private marketplace test

Throwaway repo. Its only job is to answer one question:

> **Can a Cowork member install and update plugins from a private GitHub repo?**

If yes, adding new skills is "members hit Update," and you stub everything so the
full list shows in the `/` menu. If no, you ship plugin ZIPs a few times a year and
lean on a catalog skill in between.

Delete this repo when the test is done.

---

## What's in here

```
.claude-plugin/marketplace.json          the catalog
plugins/jyoungel/
  .claude-plugin/plugin.json             version 1.0.0
  skills/ping/SKILL.md                   replies "PONG v1"
```

---

## Stage 1 — can Cowork see a private repo at all?

You own this repo, so this is the cheap version of the test. If Cowork can't reach a
repo *you* own, it can't reach one your members are invited to either — and you're done
in five minutes.

1. Push this repo to GitHub as **private** (see Pushing, below).
2. Cowork → **Customize → Plugins → Add marketplace**
3. Enter `YOURACCOUNT/skill-vault-test`
4. Install the **jyoungel** plugin.
5. Run `/jyoungel:ping`

**Record what happened, not just pass/fail.** The interesting part is *how* it
authenticated:

- Did it prompt you to sign in to GitHub?
- Did it connect silently? (If so — whose credentials? That question matters in Stage 2.)
- Did it fail, and with what message?

| Check | Result |
| :---- | :----- |
| Marketplace added | |
| Plugin installed | |
| `/jyoungel:ping` appeared in the `/` menu | |
| `/jyoungel:ping` returned PONG v1 | |
| Auth prompt seen? | |

---

## Stage 2 — can someone who *isn't* you?

**This is the one that decides it.** Stage 1 only proves you can read your own repo.

1. Make a second free GitHub account (throwaway is fine — faster than asking a member,
   and nobody's goodwill is wasted if it breaks).
2. Invite it to this repo as a **read-only collaborator**.
3. Sign into Cowork on that side.
4. Repeat Stage 1 steps 2–5.

| Check | Result |
| :---- | :----- |
| Second account added the marketplace | |
| Second account installed the plugin | |
| `/jyoungel:ping` worked | |
| What the sign-in flow asked for | |

Also worth noting: if it asked for a GitHub OAuth scope, **write down which scope**. If it
wants full `repo` access to all their repositories, some members will balk, and that's a
signup-conversion problem worth knowing about now.

---

## Stage 3 — does Update actually deliver a new skill?

This is the whole thing you're buying.

1. Run `../skill-vault-stage3/apply-stage3.sh` (adds a `pong` skill, bumps to 1.0.1).
2. Push.
3. In Cowork, on the **second** account, hit **Update** on the marketplace.
4. Run `/jyoungel:pong`.

| Check | Result |
| :---- | :----- |
| Update button pulled 1.0.1 | |
| `/jyoungel:pong` appeared | |
| Returned PONG v2 | |
| How many clicks it took, start to finish | |

**Expectation to hold:** Update is a member action, not a silent push. Background refresh
is unreliable for private repos over HTTPS. Members will click Update the way they'd
update any app — they won't wake up current.

---

## Pushing this to GitHub

Already committed locally as `v1.0.0`. Pick one:

**GitHub Desktop** (installer is on your Desktop) — Add → Add Existing Repository → point
it at this folder → Publish repository → **keep "Keep this code private" checked**.

**Command line** — create the empty private repo on github.com first, then:

```bash
cd "C:/Users/Young/Desktop/Sandbox/skill-vault-test"
git remote add origin https://github.com/YOURACCOUNT/skill-vault-test.git
git branch -M main
git push -u origin main
```

---

## Reading the result

| Outcome | What you build |
| :------ | :------------- |
| Stages 1–3 all pass | Private marketplace. Stub every skill, full `/` menu, members hit Update. The catalog skill becomes a nice-to-have for mid-cycle additions. |
| Stage 1 passes, Stage 2 fails | Cowork was using *your* credentials. Members can't. Fall back to ZIP uploads + catalog skill. |
| Stage 1 fails | Private repos are out for Cowork entirely. ZIP uploads a few times a year + catalog skill covering everything in between. |

Whichever way it lands, log it to
`G:\Vaults\Young El OS\Sandbox\Skill-Version-Control\MEMORY.md` — the architecture note
lists this as the open question everything else waits on.
