# Starting XSS: The Most Common Vulnerability You'll Ever Test For

New series, new topic: **Cross-Site Scripting (XSS)**. If API security was about what a server trusts a client to say, XSS flips that around — it's about what a *browser* trusts a server to hand it, and what happens when an attacker gets to control that.

This is a genuinely foundational topic. It's also, by PortSwigger's own account, probably the most frequently occurring web security vulnerability out there — which makes it a natural next step after API testing rather than a detour.

## What XSS Actually Is

Cross-site scripting is a vulnerability that lets an attacker compromise the interactions a user has with a vulnerable application. It works by circumventing the same-origin policy — the browser mechanism that's supposed to keep different websites isolated from each other.

The mechanism itself is simple to state: a vulnerable website returns malicious JavaScript to a user's browser. When that code runs inside the victim's browser, the attacker can fully compromise the victim's interaction with the application from that point on.

What makes this dangerous isn't the JavaScript itself — it's *whose browser* it's running in and *what session it has access to*. If the code executes in a victim's browser, it runs with all the same access that victim has: their cookies, their session, their permissions on the site.

## Why This Deserves Its Own Deep Dive

A few things make XSS worth taking seriously rather than treating as a "well-known basics" checkbox:

- **It's everywhere.** Any place user input eventually ends up back in a page — a search box, a comment field, a URL parameter — is a candidate.
- **Impact scales with the victim.** On a site where everyone's anonymous and all data is public, XSS might barely matter. On a site handling banking transactions or healthcare records, or where the victim happens to have admin privileges, XSS can mean full compromise of the application and every user on it.
- **It's not one bug — it's a family of related ones.** The underlying idea (attacker-controlled input reaching a place where it gets executed) shows up in several structurally different ways, which is exactly why this module breaks it into multiple distinct types rather than treating XSS as a single technique.

## The Three Types, at a Glance

PortSwigger splits XSS into three categories based on *where* the malicious script actually comes from:

- **Reflected XSS** — the malicious script comes from the current HTTP request. The simplest case: input goes into the request, comes straight back out in the response, unsanitized.
- **Stored XSS** — the malicious script comes from the website's own database. An attacker submits a payload once (a comment, a profile field, a message) and it gets served to every user who later views that content.
- **DOM-based XSS** — the vulnerability lives entirely in client-side JavaScript, not the server. Attacker-controlled data flows into a part of the page's DOM in an unsafe way, with the server never even seeing the malicious payload as such.

These aren't just three flavors of the same bug — they differ in *where* the vulnerability actually lives (request/response cycle, server-side storage, or purely client-side code), which changes how you'd go about finding and testing for each one.

## What This Series Will Cover

Following the module structure section by section:

1. **Reflected XSS** — how it works, its impact, the different contexts it shows up in, and how to test for it
2. **Stored XSS** — same breakdown, but for the persistent, "waits for a victim" version
3. **DOM-based XSS** — sources and sinks, testing methodology, exploitation in third-party dependencies like jQuery and AngularJS
4. **XSS contexts** — how the right payload depends entirely on *where* your input lands: between HTML tags, inside a tag attribute, inside a `<script>` block, or inside a client-side template
5. **Exploiting XSS** — what an attacker actually does once they have working script execution: stealing cookies, capturing passwords, bypassing CSRF protections
6. **Dangling markup injection** — a fallback technique for capturing data cross-domain when full script execution isn't possible
7. **Content Security Policy (CSP)** — the browser-level defense against XSS, how it helps, and how it can sometimes be bypassed
8. **Preventing XSS** — input filtering, output encoding, response headers, and how these combine into actual defense

## How I'll Be Writing This Series

Same rules as the API security series: no lab solutions, no handed-over payloads. The value here isn't memorizing a specific `<script>` tag — it's understanding *why* a given context requires a specific kind of payload, so you can reason your way to the right one on any target, lab or otherwise.

Each post will cover:
- The mechanism, explained plainly
- Why that particular variant of XSS exists — what architectural decision makes it possible
- How to recognize and test for it without giving away the exact exploit
- How it maps to real-world impact, not just "you can pop an alert box"

**Next up:** reflected XSS — the simplest form of the vulnerability, and the one most people picture when they first hear the term.
