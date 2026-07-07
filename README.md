# Gnome Weather

## Publishing (GitHub Pages) — required for App Store submission

The App Store listing needs public Privacy Policy and Support URLs. Three steps:

1. Create an empty GitHub repo (e.g. `gnome-weather`) at github.com/new
2. Push this folder:
   ```
   cd ~/projects/gnome-weather
   git add -A && git commit -m "site"
   git remote add origin git@github.com:<YOUR-USERNAME>/gnome-weather.git
   git push -u origin master
   ```
3. On GitHub: repo → Settings → Pages → Source: "Deploy from a branch" → branch `master`, folder `/ (root)` → Save.

After ~a minute the pages are live at:
- Privacy: `https://<YOUR-USERNAME>.github.io/gnome-weather/privacy.html`
- Support: `https://<YOUR-USERNAME>.github.io/gnome-weather/privacy.html#support`

Paste those two URLs into App Store Connect.
