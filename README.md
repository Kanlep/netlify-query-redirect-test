**Issue Summary:**
I'm deploying a simple site to test Netlify redirect rules using `netlify.toml`. The intention is to block requests with suspicious query parameters and send them to a custom 404 page.

**Live Example:**
https://scintillating-kitsune-ca921c.netlify.app/?slot=777  
^ This should redirect to `/error/404.html`, but it does not. It just loads `index.html`.

**Repo:**
GitHub: https://github.com/Kanlep/netlify-query-redirect-test  
Contains:
- `index.html`
- `error/404.html`
- `netlify.toml`
- `README.md` ← this file you're reading

**netlify.toml**

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

Expected Behavior:
According to Netlify docs, this should trigger a redirect to /error/404.html with a 404 status if a query param like ?slot=777 is present.

Actual Behavior:
It just shows the default page. No redirect. No error.

We tried everything:
netlify.toml lives at the root of the project (beside index.html)
/error/404.html exists and is reachable directly
Deployed a fresh build with cleared cache
Confirmed deploy includes the correct files and structure
Changed param names (to rule out reserved word issues)
Tested from multiple browsers and incognito windows
Tested using only _redirects (placed at the project root)
Tried _redirects and netlify.toml together
Even removed _redirects to rule out conflicts
Verified _redirects wasn’t ignored in .gitignore by running git check-ignore -v _redirects

Netlify Deploy URL:
https://scintillating-kitsune-ca921c.netlify.app/
Try it with ?seo=slime


Conclusion
The docs say this should work.
It does not.

If this isn’t a bug, then the redirect docs need clarification.
If it is a bug, please help us and fix it.

Thanks in advance.  
-Kanlep

UPDATE: This redirect rule works only if you set force = true in each rule block.
Example:

toml
Copiar
Editar
[[redirects]]
  from = "/*"
  query = { slot = ":value" }
  to = "/error/404.html"
  status = 404
  force = true