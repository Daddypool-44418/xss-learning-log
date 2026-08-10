# Dangling Markup Injection: Stealing Data When Full XSS Isn't Possible

Posts 2 through 6 assumed you could get a full script to execute. This post covers what to do when you can't — when input filtering is strong enough to block `<script>` tags, event handlers, and anything else that would normally give you code execution, but you can still inject *some* unclosed HTML.

That's dangling markup injection: a technique that doesn't rely on running JavaScript at all.

## The Core Idea

Dangling markup injection works by injecting an HTML tag that's deliberately left unclosed, along with an attribute that points to an attacker-controlled server. Because the tag is never closed, the browser keeps consuming page content as if it were still part of that tag's attribute — right up until it hits a closing quote or the tag naturally ends elsewhere in the page.

The practical effect: whatever HTML markup normally would have followed the injection point — including sensitive data that renders later in the page, like a CSRF token or other page content — gets swallowed up as part of that dangling attribute's value instead. When the browser then goes to fetch the resource that attribute points to, it sends that captured content along with the request, straight to a server the attacker controls.

## Why This Matters When XSS Isn't Fully Blocked

This technique exists for a specific gap: sites that filter aggressively enough to stop script execution but don't sanitize HTML injection completely still leave a path to steal data. Even strict CSP settings that block inline scripts don't necessarily stop a browser from making a request to fetch an image or a resource — which is exactly the mechanism dangling markup relies on.

That's the useful reframe here: dangling markup isn't a weaker version of XSS that happens to still work. It's a genuinely different attack that targets HTML parsing behavior rather than script execution, which is why it can succeed in situations where script-based XSS is fully blocked.

## What Kind of Data Gets Captured

Because the technique relies on the browser continuing to read page content into an unclosed attribute, anything that appears later in the page's HTML — before whatever eventually closes the dangling tag — is fair game. This commonly includes things like CSRF tokens embedded in forms, or any other sensitive value the page happens to render into its markup after the injection point.

## The Limitations

Dangling markup injection has real constraints compared to full XSS, and it's worth being upfront about them:

- It depends heavily on the exact structure of the page — where the injection point sits relative to the data you're trying to capture.
- Browsers differ in exactly how they handle unclosed tags and where they stop consuming content, which affects reliability across different browser engines.
- It can only capture what the browser encodes into a request as it fetches the pointed-to resource — it's not equivalent to arbitrary script execution, so it's a narrower tool suited to a narrower goal: exfiltrating specific visible data, not general compromise of the page.

## Why This Fits Here in the Series

This technique sits naturally after the exploitation post rather than before it — it only makes sense once you understand what an attacker is actually after (session tokens, CSRF tokens, sensitive page content) and why cookie theft or credential capture via full script execution is the default goal. Dangling markup injection is best understood as the fallback move when that default goal is blocked, not a separate vulnerability class competing with XSS for attention.

**Next up:** Content Security Policy — the browser-level defense that's supposed to stop a lot of what's been covered so far, how it actually works, and where it can still be bypassed.
