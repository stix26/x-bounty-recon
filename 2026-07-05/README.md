# X (Twitter) Bug Bounty Recon - 2026-07-05

## Summary
- **Target**: x.com
- **Subdomains Enumerated**: 706 (via subfinder)
- **Date**: 2026-07-05
- **Primary Domain**: x.com (redirects to twitter.com)

## Live Hosts
| Host | Status | Notes |
|------|--------|-------|
| www.x.com | 200 OK | Redirects to twitter.com |
| help.x.com | 403 | Cloudflare challenge |
| support.x.com | 301 -> help.x.com |
| blog.x.com | 403 | Cloudflare challenge |
| mail.x.com | 301 -> mail.google.com/a/twitter.com |
| developer.x.com | 200 OK | Developer portal |
| api.x.com | 404 | |

## CORS Testing
- **CRITICAL**: `access-control-allow-origin: https://evil.com` reflected on:
  - www.x.com
  - support.x.com
  - mail.x.com
- These hosts mirror the Origin header back in the response.
- `access-control-expose-headers` also exposed

## Exposed Files
| Path | Status | Size | Notes |
|------|--------|------|-------|
| /dump.sql | 200 | 265 KB | Likely redirect to twitter.com |
| /admin | 200 | 62 KB | Redirect page |
| /api | 200 | 1.6 MB | Large - likely landing page |
| /backup | 200 | 84 KB | Redirect page |
| /robots.txt | 200 | 2.5 KB | Valid robots.txt |
| /.env | 403 | | Blocked |
| /.git/config | 403 | | Blocked |

## Directory Fuzzing (ffuf)
- All paths return HTTP 301 redirect (0 bytes) to twitter.com
- `.env` and `.git/config` return HTTP 403 - properly blocked

## Security Findings
- **Severity**: HIGH - CORS misconfiguration allows arbitrary origin reflection
- Cloudflare WAF in place on help/blog subdomains
- HSTS enabled with includeSubdomains
