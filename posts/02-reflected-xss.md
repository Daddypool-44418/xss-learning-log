# Reflected XSS: The Simplest Form, and Why That Doesn't Mean Simple to Fix

Post 1 covered the three types of XSS at a high level. This post goes deep on the first and simplest one: **reflected XSS** — the version most people picture when they first hear the term.

## What Makes It "Reflected"

Reflected XSS happens when an application takes some input from an HTTP request and includes that data in the immediate response, without properly sanitizing it. If the payload lands somewhere the browser will parse as executable code, the attacker's script runs — but only for whoever actually sends that specific request.

That last part is the defining feature: the vulnerable input and the vulnerable output happen in the *same* request/response cycle. There's no storage step, no waiting for a future visitor. The moment the request goes out, the payload comes right back and executes.

## Why "Reflected" Doesn't Mean "Harmless"

A common misconception is that because the attacker's own request triggers it, reflected XSS is somehow lower stakes than stored XSS. That's backwards. Since the payload only executes for the person who sends the request, an attacker's real work isn't crafting the payload — it's getting a *victim* to send that exact request instead.

That's typically done by getting the victim to visit a URL controlled or influenced by the attacker — a link shared over email, posted somewhere public, or embedded in another website. Once the victim's browser sends that request, the vulnerable application reflects the attacker's script straight back into the victim's own session.

## How the Vulnerability Actually Gets Introduced

The mechanics: an application reads a value straight out of the request — a query parameter, part of the URL path, a header, whatever the app happens to reflect — and inserts it directly into the HTML response without encoding it properly for the context it lands in.

The simplest illustrative shape of it: a search feature that takes a `search` parameter and echoes it back on the results page, something like "You searched for: [value]." If that value gets inserted into the page as raw HTML instead of being encoded, and the attacker controls what's in `[value]`, they control a chunk of that page's markup.

## Where to Look for It

Reflected XSS candidates show up anywhere user-controllable input makes a round trip into the response:
- Search results ("no results found for `X`")
- Error messages that echo back what caused the error
- Any parameter used to personalize or confirm what the user submitted
- Values reflected into HTML comments, attributes, or inline scripts, not just visible page text

The general test is straightforward in concept: send a distinctive, harmless marker value as input, then check whether that exact value shows up unescaped in the response, in a spot where it would actually be interpreted as markup or script rather than just displayed as plain text.

## Context Changes Everything

Here's the part that makes reflected XSS more interesting than "find a spot and paste a payload." Where your input lands in the response determines what kind of payload is even possible:

- Reflected directly between two HTML tags — one shape of payload
- Reflected inside a tag's attribute value — a different shape entirely
- Reflected inside an existing `<script>` block — different again
- Reflected inside a JavaScript string, a URL, or a CSS context — each has its own rules for what breaks out of the surrounding syntax

This is worth sitting with rather than rushing past: the *existence* of reflected XSS depends on finding unsanitized input in the response at all, but *exploiting* it depends entirely on correctly reading the surrounding context and crafting something that fits it. Two apps can both be "vulnerable to reflected XSS" and require completely different payloads because the input lands in different places.

## Why This Sets Up the Rest of the Series

Reflected XSS is the cleanest version of the underlying problem: input goes in, comes back out, unsanitized. Stored XSS (next post) is structurally almost identical — the only real difference is a database sits in between the input and the output, which changes *who* gets affected and *when*. And the context-dependent payload-crafting covered briefly here is exactly what Post 5 (XSS contexts) will go much deeper on, since it applies just as much to stored and DOM-based XSS as it does here.

**Next up:** stored XSS — what changes when the attacker's payload doesn't need a victim to click a link at all, because it's already sitting in the application waiting for someone to view it.
