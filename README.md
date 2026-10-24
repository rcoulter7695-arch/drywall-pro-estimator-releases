<img width="3837" height="2007" alt="Screenshot 2026-10-24 170947" src="https://github.com/user-attachments/assets/14696184-2b98-4e38-bc2e-af1995cefbc2" />
<img width="3837" height="2001" alt="Screenshot 2026-10-24 171048" src="https://github.com/user-attachments/assets/3d97bd86-812b-4c42-a917-41922d5b4cba" />
<img width="3837" height="2004" alt="Screenshot 2026-10-24 171127" src="https://github.com/user-attachments/assets/a103231f-b5de-49b7-9274-b6b31735fb87" />
<img width="3774" height="2004" alt="Screenshot 2026-10-24 171151" src="https://github.com/user-attachments/assets/701d1e81-c76f-43e8-b51a-1785a7bb9b7a" />
<img width="3783" height="2004" alt="Screenshot 2026-10-24 171249" src="https://github.com/user-attachments/assets/9eb863af-d723-454a-b559-8f1ec0a19ae4" />
<img width="3837" height="1986" alt="Screenshot 2026-10-24 171337" src="https://github.com/user-attachments/assets/5d4e563a-db5c-4f84-8563-34612a1fa591" />
<img width="3780" height="2004" alt="Screenshot 2026-10-24 171400" src="https://github.com/user-attachments/assets/907ae613-e2d8-4b6c-ac73-647a11c8f9ad" />
# drywall-pro-estimator-releases
# Drywall Pro Estimator – Windows + Linux desktop app

A standalone, offline drywall material estimator. The app makes no network
requests and needs no account.

## Blueprint Takeoff tab

1. **Open plan**: a PDF or an image (PNG, JPG, ...). Multi-page PDFs work; each page has its own scale. Type a page number in the **Page** box and press Enter to jump straight there (« and » go to the first and last page).
2. **Set scale**: click two points a known distance apart (a dimension line), type the distance (12' 6" or 12.5).
   PDFs also have a scale preset (1/4" = 1'), which is only right if the sheet is full size, so check it with **Measure**.
3. **Simplest way: Box room.** Click one corner of the room, then the opposite corner. You get a box with four straight sides,
   so door swings, dimension lines and text on the drawing are never picked up. Walls = distance around × wall height;
   ceiling = length × width. For an L-shaped room draw two boxes. No plan? Use **+ Room by size** and type the length and width.
   Or **Trace room** to click the corners of an odd-shaped room.
   **Openings with no wall (garage door, wide archway):** while tracing, click the corner where the wall stops, press **O**
   (or click **Next side = opening**), then click the corner where the wall starts again. That side is drawn dashed and its length
   is left out of the wall total; the floor and ceiling still count the whole room. On a room you already drew (including a Box room),
   use **Mark opening** and click the side; click it again to put the wall back. For a partial opening such as a 16 ft garage door
   on a longer wall, type its width in **Garage door / opening** in the room details (works for any room, including Box rooms).
   Tip: **⛶ Big view** makes the plan fill the whole window (Esc to exit; F11 for full screen). **Rooms & totals** shows the side panel over it.
4. Set each room's **wall height** and **ceiling**: flat (including high ceilings), vaulted (peak in the middle or one slope),
   or tray. Add **Stairways** (run, rise, width, headroom) and **Extra areas** (closets, soffits).
5. **Totals** give wall and ceiling sq ft plus sheets, ProForm boxes, screws, tape and primer, using the same sheet rules
   and coverage rates as the Estimator tab (tape is counted in 500 ft rolls).

To start another plan, click **Remove plan** (click twice to confirm). It removes the open plan and the rooms traced on it
and keeps your stairways and extra areas. **Start over** clears everything.

It measures what you outline; it does not read dimensions or notes off the drawing, so check each outline and the scale.

## Save and open jobs

**Save job** (top right) writes a `.dwjob` file with the Estimator numbers, the rooms, stairways, scales and the plan files themselves,
so you can reopen the whole takeoff later with **Open job**. Opening a job replaces what is on screen.

## Company settings and printouts

**⚙ Settings** (top right) takes your company name, address, phone, email, a license line, a note for the bottom of printouts and a logo.
They are saved on this computer and appear at the top of every printout. The Settings window ends with an **About** section: version and build time, who programmed it, and the copyright line
(edit the name, year, phone and email in `index.html`, search for `ABOUT`, and the copyright in `package.json`).
The version at the top right comes from `package.json` and the build time is stamped each time you run `npm start` or a build.
Raise the version with `npm version minor --no-git-tag-version`.

## Printing

Both tabs have a **Print** button with an optional job name. It opens the print window; choose a printer, or **Save as PDF**.
The Blueprint printout has the totals, materials and a per-room breakdown.

## Check for updates

**⚙ Settings → About → Check for updates** asks one small web file whether a newer version exists. It is the only thing in the app that
uses the internet, and only when the button is clicked. Customers still download the new installer themselves.

1. Put a file like `update/version.json` (a template is included) on the web. With GitHub, commit it to a public repository and use the
   "Raw" address, e.g. `https://raw.githubusercontent.com/YOURNAME/YOUR-RELEASES-REPO/main/version.json`.
   `version` is the newest version, `url` is the page customers open to download it (your Releases page), `notes` is optional.
2. Put that address in `package.json` as `"dpeUpdateUrl"`, then rebuild.
3. Each time you release, raise the version (`npm version minor --no-git-tag-version`), build, upload the installers to the release,
   and change `version` in the web file. The button compares the two numbers.

## License keys (yearly or monthly, works offline)

Keys are made with `tools/keymaker.js`, which only you run. The app holds just the *public* half of the key pair, so it can check a key
but never make one. A key carries the customer name, an end date (or "never") and, if you want, one Computer ID.

    node tools/keymaker.js init                                   (once; also: npm run keys -- init)
    node tools/keymaker.js make --name "Mike's Drywall" --months 12
    node tools/keymaker.js make --name "Mike's Drywall" --months 1        monthly
    node tools/keymaker.js make --name "Mike" --until 2027-12-31
    node tools/keymaker.js make --name "Mike" --months 12 --machine ABCD-EFGH-JKLM   only that computer
    node tools/keymaker.js list

- **Until you run `init`, the app asks for no key.** After `init`, rebuild: that build asks for a key on first start.
- **Back up `tools/keys/private.json`** (it is not uploaded to git). If you lose it you cannot make keys the app accepts; if someone gets it
  they can make keys. Running `init` again makes every key you already sent stop working, so it asks you to add `--force`.
- You email the customer the key; they paste it into the lock screen. They can send you the **Computer ID** shown there if you want to tie
  the key to their computer. A key works through the end of its last day. Two weeks before the end the app shows "License ends in N days",
  and in About they can paste a renewed key. When it ends the app locks until a new key is entered.
- Turning the computer's clock back does not give more time: the app remembers the latest date it has seen.
- **Your own copy:** `npm run build:win:owner` (or `build:linux:owner`) makes files named `...OWNER...` that never ask for a key.
  Keep those private: anyone who has one can use the app for free. Or make yourself a key with `--lifetime`.
- This stops casual sharing and expired copies. It cannot stop someone who takes the app apart.

## Requirements

Node.js (LTS) from https://nodejs.org, then in this folder run `npm install` once.

## Windows (.exe)

Build on Windows:

    npm run build:win

Output in `dist/`:
- `Drywall-Pro-Estimator-Portable.exe` – single file, no install needed
- `Drywall-Pro-Estimator-Setup.exe`    – installer

The .exe is unsigned, so SmartScreen may show "Unknown publisher".
Click **More info → Run anyway**.

## Linux (AppImage, .deb, .rpm)

Build on Linux:

    npm run build:linux

Output in `dist/`:
- `Drywall-Pro-Estimator-x86_64.AppImage` – works on any distro, no install
- `drywall-pro-estimator_1.0.0_amd64.deb` – Debian / Ubuntu / Mint
- `drywall-pro-estimator-1.0.0.x86_64.rpm` – Fedora / RHEL / openSUSE

Install:

    # AppImage (any distro)
    chmod +x Drywall-Pro-Estimator-x86_64.AppImage
    ./Drywall-Pro-Estimator-x86_64.AppImage

    # Debian / Ubuntu
    sudo apt install ./drywall-pro-estimator_1.0.0_amd64.deb

    # Fedora
    sudo dnf install ./drywall-pro-estimator-1.0.0.x86_64.rpm

Building the .rpm needs `rpm` installed (Fedora: `sudo dnf install rpm-build`;
Debian/Ubuntu: `sudo apt install rpm`). Skip it by building only
`npm run build:linux -- --linux AppImage deb` if you don't need it.

## Logo on the Windows .exe

The build stamps `build/icon.ico` into the .exe (scripts/afterPack.js, using the `rcedit` package that `npm install` fetches),
so the desktop shortcut, Start menu and Add/Remove Programs show the logo. If an old blank icon lingers after reinstalling,
uninstall the old version first, then reinstall (or restart Explorer / clear the Windows icon cache).

## Both at once

`npm run build:all` builds everything. This is easiest on Linux with Wine
installed; otherwise build each platform on its own OS.

## Notes

- To test without building, run `npm start`.
- To add an icon, put a 512x512 `icon.png` in a `build/` folder
  (electron-builder picks it up for Linux, and converts it for Windows).
