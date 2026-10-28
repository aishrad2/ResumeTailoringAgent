# Decisions

Short notes on the main choices and why.

## 1. YAML resume file, not PDF
**Why:** A PDF has to be parsed, which adds a new attack surface and makes the
resume less reliable as a source of truth. A YAML file I wrote myself is
trusted and has stable IDs for every bullet.
**Tradeoff:** Harder for non-developers. A PDF import helper could come later
and would output a YAML draft to review.

## 2. If a rewrite fails the check, use the original bullet
**Why:** Dropping the bullet could leave a section empty. The original is
always safe, and the flag shows me what happened.
**Tradeoff:** The original may match the job less well.

## 3. "Skills list" means the `skills` list at the top of `resume.yaml`
**Why:** The checker needs a clear list of what I'm allowed to claim.
**Tradeoff:** A rewrite could credit a skill to a bullet that didn't use it.
A stricter mode (bullet-level skills only) could be added later.

## 4. Local and offline
**Why:** Resumes are personal. The matching model is downloaded once in a
setup step, then everything runs with no internet, except the local AI on
this machine.
**Tradeoff:** One extra setup step.

## 5. The AI sits behind a simple interface
**Why:** Tests can swap in a fake AI that tries to cheat. Coding tools like
Jules work in the cloud and can't reach a local model, so tests need a fake
anyway.
**Tradeoff:** A little extra code.

## 6. Review in a local web page, accept or reject each change
**Why:** Human review is the main safeguard against made-up or inflated
claims, so it has to be quick enough to actually do. Accept or reject per
change is fast, and the page highlights what changed and badges risky edits.
**Tradeoff:** More to build (a small local web server). No editing text in v1.

## 7. The PDF is built only from approved text
**Why:** What I reviewed is exactly what gets sent. No regenerating after
approval, and no "accept all" shortcut. Text is escaped and nothing external
is loaded, so the PDF tool can't be tricked by odd text.
**Tradeoff:** Simple styling only.

## 8. Trust my own computer and store nothing
**Why:** I can't defend against malware on my own machine, so it's out of
scope. The only files are `resume.yaml` in and the PDF out, which keeps the
project small. The localhost-only checks stay as a guard against
misconfiguration, not as a malware defense.
**Tradeoff:** If the machine is compromised, there is no protection.

## 9. Two guards on the local web page
**Why:** Other browser tabs can send requests to a local server. A random
token (generated at startup) and a check of the `Host` header stop that, and
also stop DNS rebinding.
**Tradeoff:** A few extra lines of code.

## 10. Three simple habits for setup downloads
**Why:** Setup is the one time outside code comes onto my machine. Pinning
exact versions, using official sources only, and writing down the model names
covers the realistic risks for a personal project.
**Tradeoff:** Doesn't protect against a compromised official source. Accepted.