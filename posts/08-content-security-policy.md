# Content Security Policy: The Browser's Built-In Defense Against XSS

Post 7 covered dangling markup injection — a fallback for when script execution is blocked. This post covers the mechanism that's often *doing* that blocking in the first place: **Content Security Policy (CSP)**.

CSP is the browser-level defense most modern sites lean on to reduce the impact of XSS, even when input sanitization has gaps. It's worth understanding both how it helps and where it can still be bypassed — because both halves matter for testing.

## What CSP Actually Is

Content Security Policy is a browser security mechanism that aims to mitigate the impact of XSS and some other injection attacks. It works by letting a site declare, via an HTTP response header, exactly which sources of content the browser is allowed to trust — scripts, styles, images, fonts, and more — and the browser then enforces that policy while rendering the page.

The core idea: even if an attacker manages to inject a `<script>` tag into a page, a properly configured CSP can prevent that script from actually executing, because the browser refuses to run script content that doesn't match the declared policy.

## How It Restricts Script Execution

A CSP typically restricts scripts along a few different dimensions:

- **Where scripts can be loaded from** — a policy might only allow scripts from the site's own domain, blocking any script an attacker tries to load from an external, attacker-controlled server.
- **Inline scripts** — a strict policy can block inline `<script>` blocks and inline event handlers entirely, forcing all JavaScript to come from external files. This alone defeats a large share of straightforward XSS payloads, since so many rely on injecting inline script.
- **Unsafe evaluation** — a policy can disallow the use of `eval()` and similar functions that execute strings as code, closing off another common code-execution route.

Together, these restrictions mean that even a successful HTML injection doesn't automatically translate into successful script execution — the browser itself refuses to cooperate if the injected script doesn't fit within the declared policy.

## Where CSP Still Falls Short

CSP is a mitigation, not a guarantee, and there are a few recurring ways it fails to fully block XSS:

**Overly permissive policies.** A policy that allows scripts from a broad set of domains, or that still permits `unsafe-inline`, gives back much of the protection CSP is meant to provide. If an attacker can find any allowed source that also happens to host attacker-controllable content (a JSONP endpoint, an open redirect, an uploaded file), the policy's allowlist can be turned against it.

**Bypasses via allowed third-party resources.** If a policy trusts a large CDN or a domain hosting many different scripts, and any one of those hosted scripts can be manipulated or aliased by the attacker, the CSP's own trust in that domain becomes the bypass.

**Misconfiguration.** CSP syntax is easy to get subtly wrong — directives that don't apply to the resource type actually being loaded, fallback directives that are more permissive than intended, or policies that are present but don't actually cover the vector being exploited.

## Why This Matters for Testing, Not Just Defense

From a testing perspective, finding a working XSS payload isn't the end of the assessment if CSP is in place — the next question is whether the discovered injection point can actually execute under the site's specific policy. A payload that works perfectly on an unprotected page might be completely inert on a CSP-protected one, and conversely, a seemingly solid CSP might still have a gap that makes a specific injection point exploitable after all.

This is also why reviewing the actual CSP header on a target is a standard step once XSS is suspected: knowing exactly what the policy allows (and doesn't) tells you whether it's worth continuing to hunt for a working payload, or whether the more productive path is looking for a CSP bypass specifically.

## Where This Fits in the Series

This post closes the loop on everything from Posts 2 through 7: those posts were about finding and exploiting XSS assuming no additional browser-level protection was in play. CSP is the piece that sits between "vulnerability exists" and "vulnerability is actually exploitable in practice" on a real, modern site — which is exactly why it's worth understanding on its own, rather than treating XSS testing as complete once an injection point is found.

**Next up:** the final post in this series — preventing XSS from the defender's side: input filtering, output encoding, response headers, and how CSP fits in as a last line of defense rather than a primary one.
