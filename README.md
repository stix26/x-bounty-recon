# X / xAI Bug Bounty Reconnaissance

Bug bounty recon pipeline output for X/xAI (hackerone.com/x).

## Session: 2026-07-05
- **Subdomains found:** 706
- **Live hosts:** www(301), api(404), ads(302), business(302), help(403)
- **CORS:** Reflective CORS on www.x.com, ads.x.com, business.x.com (echoes Origin header)
- **CORS:** Accepts `Origin: null`
- **Status:** Recon complete, informational CORS findings
