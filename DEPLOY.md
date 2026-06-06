# Deploying the ReplyTrove Landing Site

## Recommended: Cloudflare Pages (free, custom domain + HTTPS)

1. Push the `landing/` folder to a GitHub repository (e.g. `replytrove/landing`)
2. Go to https://dash.cloudflare.com → Pages → Create a project → Connect to Git
3. Select the repo, set **Build command** to *(empty)* and **Build output directory** to `/` (or `.`)
4. Deploy — Cloudflare auto-deploys on every `git push`
5. Add custom domain: Pages → your project → Custom domains → `replytrove.app`
   - Add a CNAME record in Cloudflare DNS: `replytrove.app → <project>.pages.dev`
   - Cloudflare issues a free TLS certificate automatically

The `_redirects` file is already included and handles any direct URL navigation.

## Alternative: Vercel (also free)

```bash
npm i -g vercel
cd landing
vercel --prod
```
Then add `replytrove.app` as a custom domain in the Vercel dashboard.

## After deployment — update Supabase Auth URLs

Go to: https://supabase.com/dashboard/project/qvmcbkaajxwvsxohukdz/auth/url-configuration

Set:
- **Site URL**: `https://replytrove.app`
- **Redirect URLs** (add):
  - `replytrove://auth-callback`
  - `replytrove://channel-connected`
  - `https://replytrove.app/**`

## Meta App — required URLs to configure

In Meta Developer Console (https://developers.facebook.com/apps):
- **Privacy Policy URL**: `https://replytrove.app/privacy.html`
- **Terms of Service URL**: `https://replytrove.app/terms.html`
- **Data Deletion Callback URL**: `https://replytrove.app/data-deletion.html`
- **OAuth Redirect URI**: your Supabase Edge Function URL
  e.g. `https://qvmcbkaajxwvsxohukdz.supabase.co/functions/v1/instagram-oauth`
