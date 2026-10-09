# Aperio

Two folders:

- `aperio-site/` is your website. `index.html` is the download page, `app/` is the web app (installable), `downloads/` holds the installers.
- `aperio-desktop/` builds the Mac and Windows installers.

## 1. Put the site online
Run `vercel` inside `aperio-site` (or drag the folder into Vercel). Everything is static. Install from the browser works as soon as it is served over https.

## 2. Build the Mac and Windows apps (free, no Windows PC needed)
GitHub builds both for you.
1. Create a free GitHub account and a new **public** repository (for example `aperio`). Upload the contents of this folder to it. Website uploads often skip the hidden `.github` folder, so in the repository choose **Add file, Create new file**, type `.github/workflows/build-desktop.yml` as the name, paste in the contents of `.github/workflows/build-desktop.yml` from this folder, and commit. The workflow only shows in the Actions tab once that file is on the main branch. With Git: `git init`, `git add .`, `git commit -m "Aperio"`, then follow GitHub's push instructions.
2. Open the repository's **Actions** tab, choose **Build desktop apps**, and press **Run workflow**. It takes about 10 minutes.
3. When it finishes, the **Releases** page has `Aperio.dmg` (Mac) and `Aperio-Setup.exe` (Windows).
4. In `aperio-site/index.html`, set `const REPO='yourname/aperio';`, then run `vercel --prod`. The download buttons now appear.

To build on your own Mac instead: inside `aperio-desktop` run `npm install --save-dev electron electron-builder` then `npx electron-builder --mac`. The `.dmg` appears in `dist/`.

Change `appId` and `author` in `aperio-desktop/package.json` first. The apps are unsigned, so the first launch needs right-click, Open on Mac, or More info, Run anyway on Windows. If macOS says the app is "damaged", run `xattr -cr /Applications/Aperio.app` once. Signing removes these prompts but needs an Apple Developer account (about US$99 a year) and a Windows certificate.

## 3. Updating the app
Edit `aperio-site/app/index.html`. If you use Tailwind classes that did not exist before, rebuild the CSS:

    npx tailwindcss@3.4.17 -i in.css -o aperio-site/app/tailwind.css --content aperio-site/app/index.html --minify

(`in.css` contains `@tailwind base;@tailwind components;@tailwind utilities;`). Copy the new `index.html` and `tailwind.css` into `aperio-desktop/app/` too, and bump the cache name in `sw.js`.
