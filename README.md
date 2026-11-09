# slouchpad.com

The website for Slouchpad (formerly Couchpad): plain HTML and CSS, no build step and no dependencies.

```
index.html          the whole site: home, setup, support and privacy sections
setup/, support/, privacy/  redirect stubs to the sections of index.html (links in the apps and in App Store Connect)
404.html            not-found page
assets/             stylesheet, icons and the Fredoka font for the wordmark (licence in assets/fonts/OFL.txt)
CNAME               the custom domain (slouchpad.com)
```

Preview locally: `python3 -m http.server` in this folder, then open http://localhost:8000.

## The Mac download

The Download button points at `https://github.com/addywaddy/slouchpad-site/releases/latest/download/Slouchpad.dmg`, so each Mac
release is published as a GitHub Release in **this** repository, with a file called exactly `Slouchpad.dmg` attached. The app
repository's `make mac-publish` does that (`SITE_REPO` in `scripts/publish-mac.sh`). The button always serves the release
marked as the latest one.

The older Couchpad releases (v1.0) stay in the `couchpad-site` repository; the file names `Couchpad.dmg` there keep working.

`support@slouchpad.com` appears on the support and privacy pages. Set up a mailbox or forwarding for it, for example with
Cloudflare Email Routing.

## Hosting on GitHub Pages

1. Create a **public** repository called `slouchpad-site` and push this folder to it.
2. Settings > Pages: deploy from the `main` branch, root folder.
3. Verify the domain: GitHub account settings > Pages > Add a domain, then add the TXT record GitHub shows. This stops anyone
   else claiming it.
4. DNS for `slouchpad.com`:
   - four `A` records for the apex: 185.199.108.153, 185.199.109.153, 185.199.110.153, 185.199.111.153
   - four `AAAA` records: 2606:50c0:8000::153, 2606:50c0:8001::153, 2606:50c0:8002::153, 2606:50c0:8003::153
   - a `CNAME` for `www` pointing at `<github-user>.github.io`
5. Settings > Pages: tick **Enforce HTTPS** once the certificate is ready (up to an hour).
6. Check that https://slouchpad.com/privacy/ and https://slouchpad.com/support/ load. App Store Connect needs both URLs.

Check GitHub's current Pages documentation for the DNS values before relying on the ones above.

## Moving from couchpad.app

The old site (repository `couchpad-site`, domain couchpad.app) becomes a redirect to this one, so links in installed apps
(Help & setup, Privacy) and anything already shared keep working. Order:

1. Set this site up (above) and publish a Mac release here, so the Download button works.
2. Check slouchpad.com, `/setup/`, `/privacy/` and `/support/`.
3. Only then switch the old repository to its redirect pages (branch `redirect-to-slouchpad` there).
4. Keep renewing couchpad.app: builds already out link to it. A registrar's "URL forwarding" is not enough for that domain,
   because `.app` domains only load over HTTPS and such forwarding usually serves plain HTTP; the redirect pages on GitHub Pages
   get a certificate from GitHub.
5. In the app repository, point the links in the apps at slouchpad.com, and update the Support, Marketing and Privacy URLs in
   App Store Connect.
6. Retake the screenshots under `assets/screens/` that show the old name (the Mac menu, the disk image, the Accessibility
   and Local Network settings, the PIN caption).
