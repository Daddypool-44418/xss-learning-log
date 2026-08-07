# DOM-Based XSS: When the Server Never Even Sees the Attack

Posts 2 and 3 covered reflected and stored XSS — both cases where the server takes attacker input and puts it, unsanitized, into an HTTP response. This post covers something structurally different: **DOM-based XSS**, where the entire vulnerability can exist without the server ever processing the malicious payload as such.

## What Makes It Different

DOM-based XSS arises when client-side JavaScript takes data from an attacker-controllable source and passes it into a sink that supports dynamic code execution, unsafely, within the page. The vulnerability lives entirely in how the page's own script handles data — not in what the server generates.

This is the key distinction from the first two types: with reflected and stored XSS, the malicious script is literally part of the HTTP response the server sends. With DOM-based XSS, the server's response might be completely clean — no attacker payload anywhere in it. The dangerous behavior only happens after the page loads, when the page's own JavaScript reads some piece of attacker-controlled data and writes it somewhere unsafe.

## Sources and Sinks

Two concepts anchor this whole topic:

**A source** is anywhere JavaScript can read attacker-controllable data from. Common ones include the URL (via `location`, `document.URL`, `document.referrer`), the fragment/hash portion of the URL, `window.name`, and data pulled from `postMessage` events.

**A sink** is a JavaScript function or DOM property that can cause script execution if it's fed dangerous data. Common ones include `document.write()`, `innerHTML`, `eval()`, and various jQuery methods that manipulate HTML content dynamically.

The vulnerability exists specifically when data flows from a source into a sink without being sanitized in between. Not every source-to-sink flow is dangerous — plenty of code reads from `location.href` and does something completely safe with it. The bug appears when that flow ends in a sink that interprets the data as executable markup or code, with nothing filtering it along the way.

## Why the URL Is Such a Common Source

A lot of DOM XSS examples center on the URL because it's an unusually convenient attacker-controlled input: an attacker can craft a link with a specific fragment or query value, and as soon as a victim clicks it, that value is sitting right there in the browser, fully under the attacker's control, before the page's JavaScript ever touches it.

Combine that with a common client-side pattern — reading part of the URL and writing it into the page to, say, highlight a search term or restore some UI state — and you get exactly the source-to-sink flow this vulnerability depends on.

## Testing for It

Because the server may never see the payload, testing DOM XSS requires a different mindset than reflected or stored XSS. Instead of only watching what comes back in the HTTP response, the useful signal is watching what the page's own JavaScript *does* with a given piece of input, client-side, after the page has already loaded.

Practically, that means:
- Reviewing client-side JavaScript for known dangerous sinks
- Tracing backward from a sink to see what source feeds it, and whether anything sanitizes the data along that path
- Using tools that can trace source-to-sink data flow automatically, since manually reading through large or minified JS files for every possible flow doesn't scale well

## Third-Party Libraries Aren't Exempt

The module also covers DOM XSS in the context of common libraries like jQuery and AngularJS — a useful reminder that this vulnerability class isn't limited to hand-written application code. Popular libraries have their own methods that act as sinks (certain jQuery selector and manipulation functions, for instance), and a page can be vulnerable purely through how it uses a library, even if the application's own code looks fine on the surface. Older versions of these libraries have also had their own history of exploitable DOM XSS behavior baked into the library itself.

## Where This Leaves the Three Types

Looking back across Posts 2 through 4, the pattern is: reflected and stored XSS both flow through the server (immediately, or via storage first), while DOM-based XSS never has to touch the server at all after the initial page load. Same underlying idea — attacker data reaching a place it gets executed — but three genuinely different places to look for it, and three different testing approaches to match.

**Next up:** XSS contexts — a closer look at exactly how the location where your input lands (between tags, inside an attribute, inside a script block, inside a client-side template) determines what shape of payload will actually work, something that applies across all three types covered so far.
