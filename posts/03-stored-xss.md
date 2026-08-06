# Stored XSS: When the Payload Doesn't Need You to Click Anything

Post 2 covered reflected XSS — a payload that only executes because the victim was tricked into sending a specific request. This post covers the more dangerous sibling: **stored XSS**, where the attacker doesn't need the victim to do anything except visit a page normally.

## What Makes It "Stored"

Stored XSS happens when an application takes attacker-controlled input and saves it — in a database, a message forum, a comment log, a visitor log, anywhere persistent — and later includes that data in a response for other users, without properly sanitizing it.

The defining difference from reflected XSS: the malicious script doesn't come from the current request at all. It comes from the application's own storage. The attacker submits the payload once. From that point on, anyone who views the content it's attached to gets the payload served to them, automatically, as part of a completely normal page load.

## Why This Is Considered More Dangerous

With reflected XSS, the attacker has to actively get a victim to click a specific crafted link. That's a real hurdle — it requires social engineering, and it only ever targets whoever clicks it.

Stored XSS removes that hurdle entirely. Once the payload is sitting in the application, it fires for *every* user who views the affected page — no link to click, no social engineering required, just normal use of the site. If the payload lands somewhere with heavy traffic (a popular forum thread, a widely-viewed profile page, a product review), the blast radius can be far larger than anything reflected XSS could realistically achieve.

## Where It Tends to Show Up

Anywhere an application accepts input from one user and later displays it to others is a candidate:
- Comments and forum posts
- User profile fields (display name, bio, "about me" sections)
- Product reviews
- Support ticket messages
- Usernames themselves, if they get rendered elsewhere in the app

The common thread across all of these: input crosses a trust boundary — it goes from "something one user typed" to "something rendered in another user's browser" — and if it isn't properly encoded at the point it's rendered, it's a candidate.

## Testing for It, Conceptually

The general shape of testing is similar to reflected XSS but with an added step: submit a distinctive marker value somewhere the app is likely to store it, then check whether that marker later reappears, unescaped, on a page viewed independently of the request that submitted it. The "independently" part matters — that's what confirms it went through storage rather than just being reflected back in the same response.

From there, the same context-dependent thinking from reflected XSS applies just as much here: where the stored value gets rendered — inside plain HTML, inside an attribute, inside a script block — determines what shape of payload would actually execute.

## Why the Same Vulnerability Splits Into Two Categories

It's worth pausing on why PortSwigger treats reflected and stored XSS as genuinely separate topics rather than one lesson with two examples. The root technical flaw is identical in both — unsanitized input reaching a place where it gets interpreted as code. What's different is the *delivery mechanism* and, as a direct result, the *attack surface*:

- Reflected XSS requires crafting and distributing a malicious link — the vulnerability and the delivery are the same step.
- Stored XSS separates those two things entirely — the vulnerability is submitting the payload once, and delivery is just other users doing what they'd normally do on the site anyway.

That distinction changes how you'd realistically defend against each one too, which is part of why the module treats them as separate topics rather than folding stored XSS in as a footnote to reflected XSS.

## Where This Leaves the Series So Far

Two posts in, the pattern connecting reflected and stored XSS is the same: unsanitized input reaching an unsafe output context, just with a different path in between. The next type, DOM-based XSS, breaks that pattern in a more fundamental way — it's the first form covered so far where the server may never even see the malicious payload at all.

**Next up:** DOM-based XSS — what happens when the entire vulnerability lives in client-side JavaScript, and the server has nothing to do with it.
