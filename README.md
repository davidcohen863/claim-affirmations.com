# claim-affirmations.com

Static marketing + legal site for Claim. No build step — plain HTML/CSS, deployable to any static
host. The app links here via `AppLinks.swift` (paywall + Settings → About), so the paths
`/privacy/`, `/terms/`, and `/support/` must keep working.

```
index.html            landing page (hero, demo video, how-it-works, footer)
privacy/index.html    Privacy Policy
terms/index.html      Terms of Service
support/index.html    Support / FAQ (use as the App Store "Support URL")
404.html              not-found page (GitHub Pages picks this up automatically)
CNAME                 custom-domain marker for GitHub Pages
assets/               icon (from AppIcon.png) + hero.mp4 (the onboarding welcome video)
```

## Deploying (GitHub Pages, free)

The app repo is private, so the site deploys from its own small **public** repo:

```bash
# one-time: create the public site repo and push this folder to it
gh repo create davidcohen863/claim-affirmations.com --public -y
git -C website init -b main && git -C website add -A && git -C website commit -m "Claim website"
git -C website remote add origin https://github.com/davidcohen863/claim-affirmations.com.git
git -C website push -u origin main

# enable Pages serving from the main branch root
gh api -X POST repos/davidcohen863/claim-affirmations.com/pages \
  -f 'source[branch]=main' -f 'source[path]=/'
```

After DNS (below) resolves, turn on **Enforce HTTPS** in the repo's Pages settings.

To update the live site later: copy changed files from `website/` here and push (or script it).

## GoDaddy DNS (one-time, in the GoDaddy dashboard)

My Products → claim-affirmations.com → DNS:

| Type  | Name | Value                     |
|-------|------|---------------------------|
| A     | @    | 185.199.108.153           |
| A     | @    | 185.199.109.153           |
| A     | @    | 185.199.110.153           |
| A     | @    | 185.199.111.153           |
| CNAME | www  | davidcohen863.github.io   |

Delete GoDaddy's default "Parked" A record. Propagation: minutes to a few hours.

Also worth doing while in GoDaddy: add **email forwarding** for
`support@claim-affirmations.com` → your real inbox (Domain Settings → Email Forwarding) — the site
and both legal documents use that address.

## Before App Store launch

- Swap the landing-page CTA (`index.html`, `class="cta"`) from "Coming soon" to the real App Store
  URL once the app has an App Store ID.
- In App Store Connect, set Privacy Policy URL → `https://claim-affirmations.com/privacy/`,
  Support URL → `https://claim-affirmations.com/support/`, Marketing URL →
  `https://claim-affirmations.com/`.
- Optionally replace the CTA pill with Apple's official App Store badge artwork (download from
  Apple's marketing-tools page; it requires accepting their guidelines, so it isn't vendored here).
