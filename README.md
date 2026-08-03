# Web Cache Deception Learning Log

A public log of my journey through PortSwigger's [Web cache deception](https://portswigger.net/web-security/web-cache-deception) module on Web Security Academy — written to document what I'm learning and to help other beginners in offensive security follow the same path.

I'm a junior offensive security professional (web app pentesting, Active Directory exploitation) previously documented an [API security learning series](https://github.com/YOUR_USERNAME/api-security-learning-log) — this repo picks up with a new topic. Same format: break down each concept as I learn it, not lab walkthroughs, but the actual thought process behind each vulnerability class.

**No lab spoilers.** You won't find copy-paste payloads or step-by-step solutions to PortSwigger's labs here — solving them yourself is where the actual learning happens. What you will find is how I think through each concept, what I tried, what failed, and how it maps to real-world caching infrastructure.

## Posts

Posts follow the PortSwigger Web cache deception module in order, so you can read along with the module section-by-section.

| # | Topic | Covers | Status |
|---|-------|--------|--------|
| 01 | Starting Web Cache Deception | Intro — what WCD is, how it differs from cache poisoning, module overview | 🔜 Coming up |
| 02 | Web Caches & Constructing an Attack | Cache keys, cache rules, using a cache buster, detecting cached responses | 🔜 Coming up |
| 03 | Exploiting Static Extension Cache Rules | Path mapping discrepancies, delimiter discrepancies, delimiter decoding discrepancies | 🔜 Coming up |
| 04 | Exploiting Static Directory Cache Rules | Normalization discrepancies (origin server and cache server) | 🔜 Coming up |
| 05 | Exploiting File Name Cache Rules | Normalization discrepancies applied to exact file name matches | 🔜 Coming up |
| 06 | Preventing Web Cache Deception Vulnerabilities | Cache-Control headers, CDN configuration, defensive recap | 🔜 Coming up |

## About Me

Junior offensive security professional focused on web application pentesting and Active Directory exploitation. Currently expanding into API security and web caching vulnerabilities.

- LinkedIn: (https://www.linkedin.com/in/dparas780/)
- Bugcrowd: (https://bugcrowd.com/h/dparas7808)
- Also see: [API Security Learning Log](https://github.com/Daddypool-44418/api-security-learning-log)

## Why This Repo Exists

Most security learning happens in private — a lab gets solved, a certificate gets earned, and none of the actual thinking behind it is visible to anyone else. This repo is an attempt to make that thinking visible: partly to solidify my own understanding by writing it out, and partly because I wish more people documented their "still learning" phase instead of only their finished credentials.

If you're working through the same PortSwigger module, feel free to open an issue or reach out — happy to compare notes.
