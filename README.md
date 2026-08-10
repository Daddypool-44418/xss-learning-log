# XSS Learning Log

A public log of my journey through PortSwigger's [Cross-Site Scripting (XSS)](https://portswigger.net/web-security/cross-site-scripting) module on Web Security Academy — written to document what I'm learning and to help other beginners in offensive security follow the same path.

Junior web pentester, still early in the journey — learning something new every day and writing about it as I go. This is my second public learning series after [API Security](https://github.com/Daddypool-44418/api-security-learning-log).

**No lab spoilers.** You won't find copy-paste payloads or step-by-step solutions to PortSwigger's labs here — solving them yourself is where the actual learning happens. What you will find is how I think through each concept, what I tried, what failed, and how it maps to real-world testing.

## Posts

Posts follow the PortSwigger XSS module in order, so you can read along with the module section-by-section.

| # | Topic | Covers | Status |
|---|-------|--------|--------|
| 01 | [Starting XSS: What It Actually Is](posts/01-starting-xss.md) | What XSS is, how it works, the three main types (reflected/stored/DOM-based), impact | ✅ Published |
| 02 | [Reflected XSS](posts/02-reflected-xss.md) | Reflected XSS — how it works, impact, contexts, testing | ✅ Published |
| 03 | [Stored XSS](posts/03-stored-xss.md) | Stored XSS — how it works, impact, contexts, testing | ✅ Published |
| 04 | [DOM-Based XSS](posts/04-dom-based-xss.md) | DOM XSS — sources/sinks, testing, exploiting, third-party dependencies (jQuery, AngularJS) | ✅ Published |
| 05 | [XSS Contexts](posts/05-xss-contexts.md) | Injecting between HTML tags, in tag attributes, into JavaScript, client-side template injection | ✅ Published |
| 06 | [Exploiting XSS](posts/06-exploiting-xss.md) | Stealing cookies, capturing passwords, performing CSRF | ✅ Published |
| 07 | [Dangling Markup Injection](posts/07-dangling-markup-injection.md) | Capturing data cross-domain when full XSS isn't possible | ✅ Published |
| 08 | Content Security Policy (CSP) | Mitigating XSS with CSP, bypassing CSP, clickjacking protection | 🔜 Coming up |
| 09 | Preventing XSS | Input filtering, output encoding, response headers, template engines, CSP as last line of defense | 🔜 Coming up |

## About Me

Junior web pentester, still early in the journey — learning something new every day and writing about it as I go.

- LinkedIn: https://www.linkedin.com/in/dparas780/
- Bugcrowd: https://bugcrowd.com/h/dparas7808
- Also see: [API Security Learning Log](https://github.com/Daddypool-44418/api-security-learning-log)

## Why This Repo Exists

Most security learning happens in private — a lab gets solved, a certificate gets earned, and none of the actual thinking behind it is visible to anyone else. This repo is an attempt to make that thinking visible: partly to solidify my own understanding by writing it out, and partly because I wish more people documented their "still learning" phase instead of only their finished credentials.

If you're working through the same PortSwigger module, feel free to open an issue or reach out — happy to compare notes.
