# Netlify Query Redirect Failure Repro

This is a **minimal test case** for a Netlify redirect issue involving query parameters.

## The Issue

Despite following [Netlify’s official documentation](https://docs.netlify.com/routing/redirects/#query-parameters), this site **does not block requests** with known spam query parameters via `netlify.toml`.

## Expected Behavior

Requests like:

- `/index.html?seo=slime`
- `/index.html?apk=virus`
- `/index.html?slot=777`

…should **redirect to** `/error/404.html` with a `404` status.

## Actual Behavior

All of those URLs **load `index.html` as if nothing happened**. Redirects are ignored.

## Project Structure

netlify-query-redirect-test/
├── index.html ← visible test page
├── error/
│ └── 404.html ← where we want to send bad bots
└── netlify.toml ← configured with documented redirect rules


## netlify.toml

[[redirects]]
  from = "/*"
  query = { seo = ":value" }
  to = "/error/404.html"
  status = 404

[[redirects]]
  from = "/*"
  query = { apk = ":value" }
  to = "/error/404.html"
  status = 404

[[redirects]]
  from = "/*"
  query = { slot = ":value" }
  to = "/error/404.html"
  status = 404
  Live Test Site
  Your Netlify Deploy URL Here
 
Try it with ?seo=slime and weep with us.

- Conclusion -
The docs say this should work. It doesn’t.

If this isn’t a bug, then the redirect docs need clarification. If it is a bug, please help us and fix it.