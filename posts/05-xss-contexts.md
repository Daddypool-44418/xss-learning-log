# XSS Contexts: Why the Payload Depends Entirely on Where You Land

Posts 2 through 4 covered the three types of XSS by *how* the malicious script gets to the victim — reflected, stored, or DOM-based. This post covers something that cuts across all three: **once you've found a spot where your input reaches the page unsanitized, what actually determines whether a given payload will work there?**

The answer is context. The exact same underlying vulnerability can require completely different payloads depending on where, structurally, your input lands.

## Why Context Matters More Than the Vulnerability Type

It's tempting to think of XSS testing as "find an injection point, paste a standard payload, see if it fires." That works occasionally, but it misses the actual skill involved. A page can insert your input into dozens of structurally different places, and each one has its own syntax rules for what counts as "still just data" versus "now interpreted as markup or code."

Understanding context means being able to look at where your input lands and reason out what's needed to break out of that context and reach something the browser will execute — rather than throwing a list of known payloads at every field and hoping one sticks.

## Between HTML Tags

The simplest context: your input lands directly in the body of the page, between two existing tags, with nothing else surrounding it. If there's no filtering at all, this is the most straightforward case — you can introduce a completely new tag, like a script element, directly into the page's markup.

The complications here usually come from filtering: some characters might be blocked or encoded, some tags might be stripped, and figuring out exactly what's allowed to get through is its own puzzle. There's often more than one way to achieve script execution even between tags, so if the most obvious tag gets filtered, there are other event-driven HTML elements worth considering.

## Inside a Tag Attribute

A different situation: your input lands inside the value of an existing HTML attribute — for example, inside a `value=""` on an input field, reflecting whatever was submitted. Here, simply injecting a new tag won't work, because you're still inside the attribute's quotes as far as the parser is concerned.

The approach shifts to closing out the attribute (and sometimes the tag itself) first, so the browser goes back to parsing regular HTML. Alternatively, if there's no way to break out of the attribute, some attributes accept JavaScript directly as their value (event handler attributes) — meaning a payload might not need to escape the attribute at all, just make use of the attribute it's already sitting inside.

## Inside JavaScript

When input lands inside an existing `<script>` block — say, as part of a JavaScript string that gets built dynamically from user input — the rules change again. Now you're not trying to introduce HTML at all; you're trying to break out of a string literal within already-executing JavaScript.

This usually means closing the string with the correct quote character, and then either terminating the current statement cleanly or commenting out the rest of the original line so the surrounding code doesn't throw an error and stop your injected code from running.

## Client-Side Template Injection

A more specific and increasingly relevant context: applications using client-side templating frameworks (like AngularJS) sometimes pass user input into a template expression, which the framework then evaluates. If that expression syntax reaches user input without sanitization, framework-specific expression syntax can be used to trigger execution — a category of its own, separate from traditional HTML or JavaScript injection, and it requires knowing the specific templating framework in play rather than a generic technique.

## The Skill This Builds Toward

Notice that none of these four contexts share the same solution. What they share is a method: identify exactly what syntax surrounds your input, work out what's needed to escape that syntax cleanly, and only then think about what payload actually achieves execution once you're free of it.

This is also why the earlier posts on reflected, stored, and DOM-based XSS all mentioned context in passing without fully explaining it — the type of XSS tells you *where* to look for the vulnerability; context tells you *what to do* once you've found it. They're independent axes, not the same question asked twice.

**Next up:** exploiting XSS — once you've got working script execution in a victim's browser, what an attacker actually does with it: stealing cookies, capturing credentials, and using it to bypass CSRF protections.
