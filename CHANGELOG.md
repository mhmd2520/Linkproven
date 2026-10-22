# Changelog

All notable changes to LinkProven (formerly PingPair) are documented here.
The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [1.1.5] - 2026-10-22

### Changed

- **A report keeps the config its run started with, not just the record.**
  The config, the record and the armed reference are copied when a run
  starts, so applying a Config change before "Save report now" can no
  longer change the config a report ships with or how its cases are judged.
  Multi-Client runs keep theirs the same way.
- **fping options in a profile are limited to ones that can't break the
  results**: `-l -s -D -M -R`, and `-t -r -b -B -H -O` with a number.
  Options such as `-q`, `-A`, `-n` or `-6` passed the old check but emptied
  the latency and loss columns.
- **Sounds play from memory**: no sound cache folder any more. A full sound
  queue drops a per-case cue before it drops an alert or a verdict.
- **Closing LinkProven with the cable already unplugged** still puts the IP
  LinkProven set back to DHCP.
- **In-app updates stage inside LinkProven's own folder**, for portable copies
  too, and always check the download's SHA-256 again there before
  installing; a failed attempt cleans up after itself.
- **Cloud sync (not yet released): a second save of a run uploads.** The
  measurement stays immutable, but a re-save adds its new files and keeps the
  new sidecar as a revision; a save that would change the results is still
  refused.
- The taskbar button follows a run's progress without polling, and the sound
  settings are read once instead of on every sound.

### Fixed

- Stopping on the Client no longer shows the Server a "Client connection
  error" banner; it says the Client closed the connection.
- A preview in the Sounds dialog cuts in even while a sound is being prepared.
- The Server's listener no longer spins if accepting a connection keeps failing.
- Config: Apply refuses while the JSON pane doesn't parse (it used to apply
  the older form values), and a half-typed JSON edit survives a tab switch.
- Wi-Fi and Ethernet crash recovery keep retrying when a restore fails, and
  log when there is nothing to undo.
- Ctrl+S with every export format unticked now says nothing was saved.
- The linkproven.com FAQ sentence about closing the app reads correctly.

## [1.1.4] - 2026-10-22

### Changed

- **A report keeps the record the run started with.** The test point, cable
  ID, ends and the rest of the record are captured when a run starts, and
  the cable fields and the Plan page's "Start this TP" are locked while it
  runs. Editing them mid-run, or loading the next test point before "Save
  report now", can no longer relabel the data; "Save report now" also keeps
  the subset you ran.
- **Closing LinkProven only undoes an IP address LinkProven set.** The
  Ethernet adapter goes back to DHCP on close only when "Set the correct IP"
  put its address there; a static address you typed yourself (a home network
  on 192.168.1.x) stays.
- **The Config tab keeps changes you haven't applied** when you look at
  another tab, and Apply now picks up a JSON-pane edit made a moment before.
- **fping options in a profile are an allow list** (output and probe options
  such as `-t`, `-r`, `-e`); a profile with any other fping option is refused
  with the list. Payloads must be 1–65,507 bytes and bandwidths at least
  1 Mbps (0 meant "unlimited" to iperf3).
- **Loss reads the same in every format.** Excel cells hold the loss worked
  out from the datagram counts and show "<0.001" like the Word, PDF and TXT
  reports; the reference and comparison tables use the main table's
  precision; comparison tables say "Latency" and "Loss", not "Avg" and
  "Packet"; the diff's A/B labels always match its numbers.
- **A multi-test-point session is judged on every test point.** Its sidecar
  verdict covers all segments (a failing TP2 used to read PASS in Analysis),
  and a judged session with a test point that ran nothing reads INCOMPLETE.
- **CSV text cells that start like a formula** (`=`, `+`, `-`, `@`) get a
  leading `'`, so an imported campaign can't run a formula in Excel; a plan
  exported and imported again keeps its labels exactly.
- **Cloud sync (not yet released): runs are immutable in the cloud.** A run
  uploads once; another save of it stays on the laptop. Only an owner
  deletes a run, no report file can be deleted while a run lists it, and a
  campaign id belongs to its workspace.
- **An installed copy stages its updates inside its own folder**, which only
  administrators can write, and checks the download's SHA-256 again there
  before installing; a portable copy checks it again in place.

### Fixed

- **Sounds no longer cut each other off.** Windows plays one sound at a
  time, so "Report saved" and the last case's cue were cut short by the
  sound that followed. Cues now play one after another, and a sound's
  first play no longer freezes the window for a moment.
- **A run ends with one sound.** An aborted sweep used to sound the
  disconnect, then "Run finished", then a warning; now the abort sounds once.
  A continuous run plays its start sound once, not once per test point.
- **Pressing Stop on the Client no longer sounds the disconnect alarm on the
  Server**, and a per-case error the sweep carries on past no longer counts
  as the run's one disconnect (which used to leave a real cable pull silent).
  Stopping on the Server mid-case no longer reads as "client disconnected".
- Stopping while the Client is still connecting no longer raises a "Server
  connection error" banner, and a Stop clicked in the first instant of a run
  is no longer ignored.
- "Quiet during a sweep" no longer silences "Run finished" outside a run.
- A copied path from a toast now pastes into Explorer (hidden wrap
  characters were copied with it). Ctrl+S with every export format unticked
  says nothing was saved instead of "Saved 0 file(s)".
- When iperf3 fails at the start of a case, the case now ends at once
  instead of pinging out its full 30 s.
- iperf3 output with a negative lost count (duplicate datagrams) parses, and
  100 % loss reads "100%" in the Multi-Client live log, not "1e+02%".
- The Server's listener survives a connection reset before accept.
- A reference limit of 0 (e.g. loss ≤ 0) now reads a clean 0 as PASS, not
  MARGINAL.
- Analysis: a new run arriving no longer moves your ticks to other runs. A
  blank export folder exports to the reports folder, not the working folder.
  Notes and run names with `<` or `&` no longer break the comparison PDF.
- The in-app updater refuses to install over a folder that isn't
  LinkProven's own (it would have deleted the other files in it).
- Help and About links open the browser as you, not as administrator.
- Wi-Fi crash recovery keeps retrying if re-enabling Wi-Fi fails. Adapters
  named "WLAN" (localised Windows) are no longer taken for the wired NIC,
  and adapter names with a typographic apostrophe work in PowerShell.
- A malformed cloud-sync outbox note is set aside instead of stopping every
  upload. Plan: a failed campaign save or rename now says so. Config: a
  corrected field clears the red error. The summary sheet closes when the
  run page is rebuilt.

## [1.1.3] - 2026-10-22

### Changed

- **A white logo on the light theme.** In the app (splash, rail, welcome
  tour) and on linkproven.com (header, footer, intro and the tab icon), a
  light page now shows the logo on a white tile with a soft grey edge and
  shadow, and the mark in deeper cyans. Dark pages keep the navy tile, and
  so do the desktop, taskbar and installer icons, which Windows shows the
  same whatever the app's theme.

### Fixed

- The logo and "LinkProven" at the top of the rail now sit on one line; the
  logo used to sit a few pixels lower than the wordmark.
- The site intro's shadows were drawn at full strength, which a dark page
  hid; they now match the icon.

## [1.1.2] - 2026-10-22

### Changed

- **The new icon is the logo everywhere in the app and on the website.**
  The launch splash now builds the navy tile up: the tile fades in, the two
  dots pop in, the pulse draws between them and the glow settles, on the
  same timing as before. The rail, the welcome tour, linkproven.com's
  header, footer, first-visit intro, browser-tab and home-screen icons and
  its link-preview card all use the tile. Word and PDF reports keep the
  flat line mark in their header.

## [1.1.1] - 2026-10-22

### Changed

- **A new app icon you can actually see.** The desktop shortcut, taskbar,
  title bar and installer used the bare thin pulse, which all but vanished
  on a wallpaper. The icon is now the pulse on a navy tile lit from above,
  with a fine bevelled edge and a soft shadow under the mark. The line and
  dots are drawn per size, so at 16 and 24 px the two dots still read as
  dots and the spike as a line. The splash, rail, reports and website keep
  the flat mark.

## [1.1.0] - 2026-10-22

The "Feel" release: you can follow a sweep by ear and from a glance at the
taskbar, notices no longer block the window, and the keyboard reaches every
page.

### Added

- **A sound for each moment.** Run start, warm-up settled or failed, each
  case (clean or with loss), a retry, run finished, the four verdicts (PASS,
  FAIL, REPEAT, INCOMPLETE), report saved, an error, a Wi-Fi block, a
  disconnect and a question each have their own sound. Choose **Sonar**
  (soft pings with an echo, the default), **Tick**, **Retro** or **Silent**
  per event, with a ▶ preview, under Setup → Appearance → **Sounds…**. The
  sounds are generated by the app itself (no audio files ship) and played
  through Windows.
- **When to stay quiet.** Volume; **Quiet during a sweep** (only the run
  start, the verdict, errors, Wi-Fi blocks and disconnects); **Respect
  Windows "Do not disturb"** (silent in presentation mode, a full-screen app
  or quiet hours, with errors and disconnects still allowed through); and
  **Use Windows system sounds instead** for anyone who prefers their own
  sound scheme. Button-click and page-change ticks are there too, off by
  default.
- **Never just a sound.** When LinkProven is behind other windows its
  taskbar button flashes for a verdict, an error or a disconnect. While a
  test runs, the button fills like a progress bar, and turns red after an
  error. On the Results tab each case's cell flashes once as it finishes,
  unless Windows animations are off.
- **Notices at the bottom right.** "Report saved" (with **Open folder**),
  "Nothing to fix", "Sweep stopped, nothing saved" and the other notices
  that needed no answer used to open a window you had to close. Now they
  appear at the bottom right and close after about 5 s; hover one to keep
  it open. They are read out by screen readers and never cover the run
  summary. Questions still wait for you.
- **Keyboard shortcuts.** Ctrl+1 … Ctrl+9 and Ctrl+0 go to the ten pages,
  F5 runs, Esc stops (after asking), Ctrl+S saves the report, F1 opens Help
  for the page you are on, and Ctrl+/ lists them all. The rail and the
  Run / Stop buttons show their keys.

### Changed

- The old single "Play notification sounds" switch is now the master
  switch beside **Sounds…**. A disconnect or warm-up failure sounds when it
  happens, not again when the run ends.

### Also in this release

- **Cloud sync, opt-in (platform P2; not active in any build yet).** Sign in
  without a password: Google or Apple in your own browser, or a 6-digit
  code from your email. Then turn on **Upload saved reports to my account**
  on Save Options. Saving never waits for the network: each run is queued
  in `%APPDATA%\LinkProven\outbox` and uploaded in the background, with
  retries. A sign-in token and the account's email are kept in Windows
  Credential Manager; the browser sign-in runs as you, never as
  administrator.
  The panel appears only in a build that has a cloud environment, and none
  does until the hosted projects exist; until then the app behaves as if
  sync did not exist.
- Supabase backend for it: workspaces (a personal one per user; teams
  later) owning `devices`, `campaigns` (with their plans), `runs`,
  `run_sidecars`, `run_points` and `run_files`, the private `reports`
  bucket and `ingest_run()`, with every client write going through that one
  function. Deleting a user's account keeps the workspace's records. See
  `docs/adr/0001-platform-backend.md`.

- **Stable ids (sidecar schema 9).** Every run gets a `run_uuid` when it
  starts, so all saves of one run are the same run, and its sidecar records
  `utc_offset_min` so the start time is a real instant wherever it is read.
  Every campaign file gets a permanent `id`, kept through renames and
  written into each record saved under it; older campaign files get one the
  first time they are opened. Older reports read exactly as before.

### Changed

- Campaign status reads saved runs through `core.run_index`, the code the
  cloud's index is tested against, so each saved run gives the same test
  points and verdicts on the Plan page as in the portal.
- A test-point number typed in Arabic-Indic digits (`TP ٣`) is still read as
  `TP3`; other scripts' digits are no longer treated as numbers.

## [1.0.0] - 2026-10-21

PingPair is now **LinkProven** (linkproven.com). This is the first LinkProven
release and a fresh start: it carries everything from 0.8.0 to 0.9.0, and it
does not take over a PingPair install's settings.

### Changed

- **New name everywhere.** The window, rail, splash, About, Help, welcome
  tour and every report format now say LinkProven, with the two-tone
  **Link**Proven wordmark beside the same pulse mark. Reports link to
  www.linkproven.com.
- **New identifiers.** The program is `linkproven` (`python -m linkproven`).
  Settings live under `LinkProven`, data under `%APPDATA%\LinkProven`, and
  firewall rules are named `LinkProven … (in)`. Reports are saved as
  `LinkProven_<time>`. Environment switches are `LINKPROVEN_*`.
- **Updates come from github.com/mhmd2520/Linkproven.** PingPair 0.6.3
  installs keep checking the PingPair repository and stay on 0.6.3; install
  LinkProven alongside them.
- The window title, About and the installer show the version as
  **v1.0.0** ("LinkProven v1.0.0"), like the splash and welcome tour.
- **Releases carry the installer and its checksum beside the .zip.** The
  updater now checks the .zip against its own `.sha256` by name. It used to
  take whichever checksum the release listed last, which could have been the
  installer's and would then have refused the update.

## [0.9.0] - 2026-10-18

The Campaign release: PingPair now keeps track of a whole train's test
points, not one sweep at a time.

### Added

- **A Plan page for the test campaign.** It holds the train set, location,
  contract, procedure and work order, the default cable, who tests and who
  witnesses, and the test points with both ends, design length and breaks.
  Test points can be typed, laid out as TP1–TP8 in one click, or imported
  from a CSV copy of the procedure's results table. Each row's status (PASS,
  FAIL, REPEAT, INCOMPLETE, not run) is read from the saved reports, and the
  page names the next test point to run. Every change saves as you make it.
  With **No campaign** selected, PingPair and its reports work as before.
- **A richer "Cable under test" record on Single-Client.**
  - The test point is a picker filled from the plan. Picking one fills its
    Cable ID and ends, and "tp 3" is saved as "TP3".
  - Cable ID suggests IDs from the plan and from earlier reports, and a
    barcode scanner works in it.
  - Length takes metres or feet and is checked against the design length
    and the 100 m channel limit.
  - **Record details…** holds the ends, the cable type, the conditions
    (temperature, humidity, train state), a witness tick and a note.
- **Required fields.** A campaign can require Test point, Cable ID, Tested
  by and more. A validated run with one missing asks first: **Fill now**, or
  **Run anyway**, in which case the report says it is not a valid record.
- **A context bar on every page** shows the campaign, train, test point,
  cable and length, so a report is never saved under the previous test
  point's cable.
- **A summary sheet replaces the finished popup.** It slides in from the
  right with the verdict, its four numbers and the files written, and says
  what to do on FAIL or REPEAT. It offers **Next: TP5** and **Retest TP4**.
  A retest's report names the run it repeats.
- **Test kit on the record.** Each laptop's model and serial number, its
  network card and driver version, and whether the card is a USB adapter
  are recorded with the evidence. When the laptop pair differs from the
  bench baseline's, the verdict's comment says so.
- **Link drops and speed changes are evidence.** The link is watched for
  the whole sweep, including the gaps between cases. Any drop or
  renegotiation fails criterion (1) with its time. It is listed in the
  report and marked on the sweep timeline.
- **Battery guard.** Before a run on battery, PingPair compares the time
  left with the sweep's estimate and asks before starting. A Server laptop
  low on battery is named in the log and the summary sheet.

### Changed

- Reports with a campaign carry its record: the rows on the cover, a
  sign-off pre-filled with names, organisations and roles, the train in the
  running header, and train set / ends / design length columns in the CSV.
  The sidecar is now schema 8, with the record in a `record` block and each
  test point's Cable ID in a session's segments.
- **A new PingPair logo.** The badge is gone. The two dots and the pulse
  sit on a transparent background on both themes. The pulse is a single
  thin, sharp, symmetrical spike, with a soft cyan glow. The app icon, the
  side menu, the welcome card, the report header and the website all use
  it. Small icons (the taskbar, the title bar, browser tabs) use a heavier
  version so the spike stays visible.
- **The launch splash animates.** The dots appear, the pulse draws from
  one to the other, then the name, the glow and the version follow. It is
  the same 2.2-second animation the website plays on a first visit. The
  main window is built once the animation has finished, so it no longer
  stutters. With Windows animations turned off, the splash shows its
  finished frame.

### Fixed

- The "Pair" half of the name in the side menu now follows the theme. It
  was always the bright cyan, which is hard to read on the light theme.

## [0.8.2] - 2026-10-17

### Fixed

- **The selected page in the side menu is readable again on the dark
  theme.** Its label and icon were white on bright cyan (1.8:1 contrast) and
  all but disappeared. They are now dark on the cyan (10.4:1). On the light
  theme the highlight is one step darker, so its white label reads 5.4:1
  instead of 3.7:1.
- The menu icons take the theme's colours as soon as the window opens,
  not only after the theme changes.

## [0.8.1] - 2026-10-15

What the first two bench runs on 0.8.0 (V11, two cables, both PASS) showed.

### Fixed

- **The live throughput chart now draws while the case runs.** Cygwin iperf3
  buffered its per-second rows when its output went to PingPair, so they all
  arrived as the case ended. The chart filled for a moment, then the next
  case cleared it. The client now runs with `--forceflush`.
- **The sweep timeline's "now" line follows the running block.** It was
  placed by total elapsed time against estimates and fell a block or more
  behind on a long sweep. The running case (and the warm-up) now fills from
  its own start, the line rides that edge, and "100%" appears only when the
  sweep has finished.
- **No console window during start-up.** Every program PingPair starts is
  now created without a console window, including any started by a library.
  If one ever appears again, the log names the program and the code that
  started it.

### Changed

- **Splash:** the PingPair mark in 3D (lit, extruded, with glossy nodes), with
  no slate box behind it and the name below.
- **Run page layout:** Subset and Continuous now share the left column and
  Cable under test the right, so the two columns come out about the same
  height and the stretched empty band is gone. Margins are unified.
- **Field hints are written directly** ("TP3", "Car A ↔ Car B", "12.50"),
  with no "e.g." and no other company's name in them.
- **A note on the Run page** asks for Test point and Cable ID before a
  validated run, since both go into the report's Table-2 row and sign-off.
- **Reports:**
  - The case-matrix figure marks a case over the link's capacity with ▲, as
    the app does.
  - "How to read" explains why ping times run to a few hundred ms in the
    cases that fill the link: the pings wait behind the test traffic.
  - PDF tables have a little more padding, with more space under the Table-2
    row.

## [0.8.0] - 2026-10-14

The redesign. The window now fits a laptop, every run is drawn as well as
tabled, reports open on their verdict, and PingPair can be installed like
any other Windows app.

### Changed — the window (Concept A · Rail)

- **A left navigation rail replaces the top tab bar.** It shows icons and
  names on a wide window and icons only on a narrow one. A dot marks the page
  a warning is about.
- **Every page scrolls, and none scrolls sideways.** The window used to need
  1200×720, which is more than a 1366×768 laptop has at 125 % scaling (about
  1093×614 usable), so fields collapsed. It now fits down to 860×500, which
  covers 150 % scaling too. Side-by-side groups stack and form columns re-flow
  (4 → 2 → 1) when they don't fit.
- **The run dock.** On Single-Client and Multi-Client, Run / Stop, the status
  line and progress are pinned under the page and never scroll away.
- **Status chips.** Role, link and target sit in one row at the top. A
  warning collapses to an orange chip; a click opens it again.

### Added — run visuals

All visuals are drawn from data PingPair already records, on dark panels in
both themes.

- **Single-Client:**
  - A **Results** tab with:
    - the case matrix (Verdict / Loss / % offered / Avg ms on fixed
      best-to-worst scales; a click takes a case in or out of the next
      run);
    - throughput as % of offered;
    - latency range per case;
    - wire load per case (payload + 66 B against the link);
    - reference bands with the worst case marked.
  - The **Validation** tab gains:
    - the link diagram (both NICs, the cable, the 100 m channel limit);
    - "Did the cable deliver?" (Client card sent against Server card
      received);
    - payload labels on the loss-per-second chart.
  - The progress bar becomes a **sweep timeline**: warm-up and every case to
    scale, retries marked, and a "now" line.
- **Setup:** the laptop readiness checks as a ring. A click on a failing
  segment opens the fixes.
- **Analysis:** a **Matrix** tab for the first ticked run, and **vs Baseline**,
  which shows each case's loss against the bench baseline.
- **Multi-Client:** one bar per simulated client against the offered rate,
  with slow clients in amber.

### Changed — reports

- **The verdict is on page one.** A PASS / FAIL / REPEAT / INCOMPLETE tile,
  four key numbers and the Table-2 row sit on the cover, with one tile per
  test point for a session. Before, a forced page break pushed the verdict to
  page 2. A report with nothing judged gains no verdict.
- **Page numbers** ("Page X of Y"), a running header (run ID, test point) and
  a footer in Word and PDF.
- **A sign-off block** (Tested by / Witnessed by, with name, organisation,
  signature and date) in Word, PDF and text.
- **One look across all five formats:** one palette (navy, brand cyan, four
  verdict colours), Inter in the PDF, the record in portrait and only wide
  appendix tables in landscape. Metrics rows are coloured by case result, and
  a short "How to read this report" box replaces the dense captions.
- **The case matrix and the lost-per-second strip appear as figures** in the
  report body.
- **Excel opens on a branded Summary sheet** with the verdict; the raw grid
  is now the "Results" sheet.
- **CSV** gains `test_point`, `cable_id`, `case_result` and `cable_delivered`
  for a validated run. A run without validation keeps the old header.

### Fixed

- A multi-segment report judged every test point's reference on the first
  segment's cases. Each test point is now judged on its own.

### Added — installing

- **An installer** (`PingPair-0.8.0-setup.exe`) puts PingPair in Program
  Files with Start-menu and desktop icons. Installed copies save reports and
  profiles in `Documents\PingPair`. Uninstalling never deletes reports. The
  portable folder still works as before.
- **The `_internal` folder is hidden**, so the portable folder shows only
  `PingPair.exe` and `Reports`.
- The in-app updater keeps an installed copy installed, and refuses a broken
  bundle that has no `_internal` folder.

## [0.7.5] - 2026-10-14

A full code review of Phase V (five parallel reviewers) found places where a
verdict rested on evidence it did not have, where the screen disagreed with
its own report, and where a report described a different run from the one
measured. All are fixed, each with a regression test.

### Fixed — the External-IP dialog

- The "External IP change detected" dialog no longer opens over the launch
  splash. The check waits until the main window is on screen.

### Fixed — verdicts

- **Pressing Stop no longer shows FAIL on screen** while the saved report
  says INCOMPLETE. The live Validation panel and the between-segments
  dialog now judge exactly what the report judges, including earlier
  attempts at a retried case.
- **A missing network-card count is "not recorded", never 0.** A Server that
  sent no packet count used to make the report blame the cable for every
  frame the Client sent.
- **The Client counts card traffic over the same span as the Server.** It
  starts at SERVER_READY and stops as CASE_DONE goes out, just inside the
  Server's window, never after it. The Client used to start later and stop
  earlier, so a few control frames counted only on the Server's side could
  hide frames the cable dropped.
- **Laptops that did not settle no longer excuse a short card count.** A
  case the cable did not deliver is FAIL, not REPEAT.
- **The link speed is the slower laptop's.** With a 1000/100 Mb/s pair, or
  with no link speed read at all, a case the link cannot carry is no longer
  failed on throughput.
- **The live grid colours each case from the same judgement as the
  verdict**: the loss limit, the bench baseline and earlier attempts. It
  used to paint any loss red, even one within the bench baseline.
- A grid cell with no datagram count reads "n/a", not a green "0". Every
  loss on screen and in reports goes through the `<0.001` formatter. A
  NaN reading is "not recorded" on screen, as in the report.
- A bench baseline can no longer come from a one-PC continuous session, from
  a run made with validation off, or from a report older than 0.7. This
  applies to both "Use last run" and "Load from report…". A baseline would
  quietly loosen every case's loss limit.
- An unrecorded NIC error count no longer crashes the save of a lossy run,
  and no longer reads as "0 errors" in the warm-up.

### Fixed — reports

- **"Validation off" now means the old reports**, as the setting promises:
  no laptop snapshots, NIC counters, warm-up or retries are recorded. The
  Server's first reply is again a 10 s wait, not 30 s.
- The report records the Server address the cases actually ran against when
  the Client's "Server host" override is set. It used to record the
  profile's address, commands included.
- Why a laptop's state could not be read (a PowerShell timeout, no wired
  adapter) is now logged and shown as a Notes row in the evidence table.
- Saving every format judges the run once, and parses each case's Server
  JSON once.

### Fixed — robustness

- A warm-up that measures nothing (the Server could not bind, the firewall
  blocks iperf3, the link went down) stops at once and lets the real error
  show. It used to try three times and then advise checking mains power.
- If the link drops during the warm-up, the sweep stops with the "cable
  unplugged" message instead of running one more doomed case.
- A retry that is stopped, or that fails to measure anything, no longer
  replaces the measured attempt. A failed retry is listed, and no further
  retries run.
- Stop, close and role switches no longer wait out a 12–20 s PowerShell
  probe, which could outlast the window's shutdown wait. The probe is
  killed instead, including the Setup tab's laptop check.
- The laptop check no longer runs while "Apply all fixes" is restarting the
  network adapter, and no sweep or Multi-Client run starts during it.
- A failing laptop check no longer leaves the box stuck on "Reading…".
- An older Server is given no warm-up (before 0.7.0) and no retries (before
  0.7.4). It would count them as sweep cases ("case 23 of 20"). A warm-up
  whose receiver counted 0 datagrams is "could not run", not "settled".

## [0.7.4] - 2026-10-14

The bench is clean on 0.7.3: both UGREEN cables PASS (0 of 15.2 million
datagrams and 0 of 71,668 pings lost). One earlier run read REPEAT on the
case that fills the link hardest (200 B / 90 Mb/s, ~47,000 datagrams/s): a
~0.2 s pause on the Server laptop overflowed the 1 MB buffer, while the
cable delivered every frame. This release sets the defaults that make a
loss-free run the normal case, at the user's request.

### Changed

- **Cable validation is on by default.** Every two-laptop Single-Client
  sweep gets the warm-up, retries and the Table-2 verdict without a trip to
  Config. A Loopback (single-PC) run is never judged — there is no cable —
  and its report carries no verdict.
- **UDP buffer default 2 MB** (`-w 2048K`): about 0.2 s of cushion at the
  heaviest case, covering the pause seen on the bench. With fping's 1 s
  timeout (0.7.3) the larger send queue costs only ping latency in the cases
  that fill the link (up to ~0.2 s), not lost pings.
- **iperf3 and fping run at Windows high priority** (Config → Test plan, on
  by default), so background work (Defender, Windows Update, indexing)
  pauses them less often. Each laptop applies its own setting.
- **A lossy case is retried up to twice** (was once), stopping at the first
  clean attempt; every attempt stays in the report. Config → Cable
  validation → "Re-run a lossy case, up to N".
- **Each test point carries its own Cable ID.** The between-segments dialog
  asks for the next test point's cable (prefilled with the last one); the
  session Table-2 and each per-test-point report use it.

### Fixed — whole-app review (5 parallel reviewers)

Verdict and reports:
- A clean retry no longer washes out a cable fault: a lossy case whose card
  counts came up short is not retried, and an earlier attempt that lost
  frames on the cable fails the case even if the retry was clean.
- A run stopped part-way reads INCOMPLETE, never PASS on the cases it
  managed; a case the operator stopped is not counted as a cable failure.
- A case with no throughput reading, or with nothing received at all, is
  "not recorded" — it used to pass.
- A lost ping is put down to the laptop only when its reply provably reached
  the Client's card; the warm-up REPEAT rule now needs a loss failure with
  clean NIC counters (not a dropped case or unrecorded counters).
- A loss below 0.001 % prints "<0.001", never "0.000" beside a FAIL.
- The session banner no longer says "ALL TEST POINTS PASS" above a test
  point that never ran.
- A TCP sweep is never judged (no datagram counts; the warm-up could never
  settle and blocked the sweep); a Loopback run gets no on-screen verdict
  either, matching its report.
- iperf3's exponent loss text (`7.2e-05%`) no longer turns a 1-datagram loss
  into an errored case.
- Excel no longer turns a Cable ID "0042" into 42; amber, not red, for a
  lossy row whose card counts were not recorded.
- A Loopback or incomplete run can no longer become the bench baseline.

App:
- An aborted test point in a session no longer throws away the test points
  already finished — you are offered to save them.
- A warm-up stop explains the readiness checks instead of listing
  connection causes; the Server no longer shows "case 21/20" after a retry.
- "Apply all fixes" is refused during a sweep (it restarts the adapter);
  closing the app during a slow fix no longer crashes; the readiness box
  follows a role change on the Setup tab.
- The Client's "Server host" now reaches iperf3 and fping, not only the
  control channel.
- Stop pressed just before a case spawned no longer runs ~30 s of fping.
- The primary adapter is the one holding a test-subnet address (then a
  linked, address-less wired card) — a docked laptop no longer had its
  office card set to 192.168.1.x, measured or reverted.
- The Ping tab and the gateway check count only real replies (`TTL=`), so
  "Destination host unreachable" is no longer a received ping; ping output
  is decoded in the console code page; `ping` and `control` are pinned to
  System32.
- Self-update on a profile path with non-ASCII characters is refused with a
  clear message instead of leaving the About tab stuck on "Updating…".
- Multi-Client: binds to the client IP only when this PC holds it; report
  times are the run's own, not the moment Save was clicked; closing during
  the preflight no longer crashes; a Stop just before iperf3 starts is
  honoured; TCP capacity uses full-size segments; iptables port ranges use
  `5201:5230`.

Security:
- Peer- or user-supplied text can no longer inject markup into the PDF (a
  hostile Server could make the PDF fetch a remote image) or a live formula
  into Excel; protocol errors no longer echo the peer's value.
- A malformed reply from the other laptop costs one field, not the sweep.
- A profile can no longer pass fping `-f/--file`, `-g/--generate` or a
  network path.
- Status labels that show peer text are plain text.

## [0.7.3] - 2026-10-13

The first runs on 0.7.2 lost no UDP datagrams in any of 40 cases and read
REPEAT on ping loss alone (5.3 % and 5.2 %) — and the 1 MB buffer had
barely changed the ping figures. Every case's *maximum* ping sat just under
10 ms. fping's own help explains it: the reply timeout is 500 ms "except
with -l/-c/-C, where it's the -p period". PingPair runs fping with
`-c` and `-p 10`, so **every reply slower than 10 ms has been reported as a
lost ping**, in every version. At 90 Mb/s a full link queue makes most
replies slower than that; the Client's card received them all.

### Fixed

- **fping now waits 1 s for each reply** (`-t 1000`, Config → Test plan →
  **fping Timeout (-t, ms)**; 0 = fping's own default). Applies to the
  Single-Client sweep and the Multi-Client tab; a `-t` in fping's Extra args
  wins. Pacing is unchanged (300 probes at `-p 10`: 4.83 s without, 4.82 s
  with). Expect ping loss near 0 and higher, true, maximum ping times in the
  70-90 Mb/s cases.

## [0.7.2] - 2026-10-13

The first runs on 0.7.1 lost no UDP datagrams in any of 40 cases — and
almost every ping in the 90 Mb/s cases (5 of 1,886 answered at 200 B /
90 Mb/s, 42-53 at 600 B / 90 Mb/s, 9-30 lost in other 70-90 Mb/s cases).
The replies did come back: the Server's card sent them and the Client's
card received them, after fping (500 ms timeout) had given up. iperf3's
`-w` is client-only and sizes both directions, so 0.7.1's 4 MB was also
the Client's send queue; a case at or over the link's capacity fills it,
and each ping waited ~0.4 s behind it.

### Changed

- **UDP buffer default 1 MB** (`-w 1024K`): about 0.1 s of traffic either
  way — 16x the old 64 KB receive cushion, well inside fping's wait. Cases
  at or over link capacity will show ping latency up to ~0.1 s (the queue,
  measured honestly); those are not judged on ping. The setting was renamed
  (`udp_buffer_kb`) so a profile saved by 0.7.1 drops its 4096.

### Added

- **UDP socket buffer** table in the report's evidence section: the size
  each case asked for and the receive buffer the Server's iperf3 actually
  got, read back from the Server.

## [0.7.1] - 2026-10-13

The first two cable runs on 0.7.0 lost packets on two UGREEN cables that
had read 0 % before, and 0.7.0's own evidence showed where they went: in
every lossy case the Server's network card had received every packet the
Client's card sent, with no NIC errors. iperf3 never got them because
Windows gave it a 64 KB UDP receive buffer — about 10 ms of 200 B traffic
at 50-90 Mb/s — and every run so far, back to v0.2.4, had that buffer.

### Changed

- **The sweep asks iperf3 for a 4 MB UDP buffer** (`-w 4096K` on the
  Client; iperf3 passes it to the Server, so the receiving laptop's buffer
  grows too — verified on the bundled 3.21: 65,536 → 4,194,304 bytes).
  Config → Test plan → **UDP buffer (iperf3 -w, KB)**; 0 restores iperf3's
  default. UDP only — on TCP `-w` is the window and would change what a case
  measures.
- **REPEAT when the cable is proven to have delivered every packet.** Each
  case's acceptance row now compares the two cards' counters: the Server's
  card must have counted every frame the Client's card sent (no allowance),
  and the Client's card every reply (within 20 background frames per case,
  the multicast chatter a card filters out). When every failing case balances and no card counted a damaged
  frame, the verdict is REPEAT, not FAIL, and the Comment says how many
  datagrams reached the Server's card and were dropped inside the laptop.
  A short count or any NIC error stays a FAIL.
- **The warm-up is judged on loss and NIC errors.** 0.7.0's 2.0 ms average
  ping limit would have stopped sound sweeps — good cables on settled
  laptops averaged 1.5-2.5 ms at 200 B / 50 Mb/s. The ping limit is now
  optional (**Also ping ≤**, off by default). The setting was renamed
  (`warmup_ping_limit_ms`) so a profile saved by 0.7.0 drops the old value.

### Added

- **Cable delivered** column in the report's case-by-case table, and
  **Server card received** in the evidence section's lossy-cases table.
- Amber "lost inside a laptop" cells on the Validation sub-tab's grid.

### Fixed

- The evidence section said a loss spread over a case points at the cable.
  It does not — the bench lost datagrams spread over 26 of 31 s that the
  Server's card had received. The column is now **When**, and the caption
  points at the card counts.

## [0.7.0] - 2026-10-12

Cable validation. Bench runs on a train project's on-board LAN showed that
a run's packet loss often said more about the two laptops than about the
cable: the same 2 m patch cord lost 6.24 % on one run and nothing on the
next, and every lossy run had laptops that had not settled (on battery,
power-saving NIC features, a small receive buffer, Windows still busy after
start-up). This release makes PingPair notice that state, record the
evidence that tells a cable fault from a laptop drop, and give the cable a
PASS / FAIL verdict in the shape of the test procedure's Table-2.

### Added

- **Cable validation (Config → Cable validation).** Off by default. When on,
  every Single-Client report leads with a verdict — PASS, FAIL, INCOMPLETE
  (a criterion had no evidence) or REPEAT (losses on laptops that never
  settled) — judged on four criteria: both NICs at the expected speed in
  full duplex; ping loss 0 %; every case at ≥ 99 % of the throughput it can
  deliver with 0 % UDP loss (or no worse than the bench baseline); and no
  NIC receive/transmit errors on either laptop. The scope is all cases by
  default, or only those up to a chosen bandwidth. A case that asks for more
  than the link can carry (200 B at 90 Mb/s needs ~120 % of 100 Mb/s once
  each packet's 66 B of overhead is counted) is judged on loss only.
- **The Table-2 row, ready to copy:** Test point, Cable, Loss % (lost ÷ sent
  over the scope, packet-weighted, from the receiving laptop's own counts),
  Pass / Fail and a one-line Comment, in Word, PDF, text and Excel.
- **Laptop readiness on the Setup tab.** Each laptop checks itself: mains
  power, Windows power mode, the adapter's Receive Buffers, its energy-saving
  features (Energy Efficient Ethernet, Ultra Low Power Mode, Green Ethernet …),
  Speed & Duplex and the link. "Apply all fixes…" switches to Best performance
  and applies the adapter changes with a single adapter restart — only after
  you confirm the exact list.
- **Evidence in every report** ("Link and laptop evidence"): both laptops'
  link, duplex, power and NIC settings at the start of the run; each NIC's
  error and discard counters over the run; the receiving end's exact
  lost / sent counts per case and in which seconds the loss happened — a
  one- or two-second burst points at a laptop, loss spread across the case
  with NIC errors at the cable.
- **Warm-up.** With validation on, short unrecorded settle runs precede the
  sweep until the laptops lose nothing and ping fast; if they never settle
  the sweep is stopped (or, if you choose, run and marked).
- **One retry for a lossy case** that showed no NIC errors, with both attempts
  kept in the report.
- **Bench baseline.** Any sweep can be made the baseline (Validation tab →
  Use last run, or Load from report…); it is stored inside every report it
  judges, so a later change cannot re-judge an old report.
- **Validation sub-tab on Single-Client:** a payload × bandwidth grid coloured
  as each case finishes, packets lost per second across the sweep, live NIC
  error tiles for both laptops, the warm-up state and the running verdict.
- **Test-point sessions.** Continuous mode numbers the next segment for you
  (TP3 → TP4), shows each test point's verdict between segments, writes a
  Table-2 with one row per test point, and can also save each test point as
  its own report. The new `{tp}` filename token (e.g. `Test2-{tp}`) names
  them. Test point and Cable ID sit beside Cable length on the Single-Client
  tab.

### Changed

- The sweep sidecar is now schema 7: per-case UDP / ping counts, NIC counter
  deltas and retry history, plus `evidence` and (with validation on)
  `acceptance` blocks. All additive — older readers ignore them.
- A v0.7 Client asks the Server to describe its laptop and to send its NIC
  counters; the additions are optional message fields, so mixing versions
  still works (the missing evidence reads "not recorded").

### Fixed

- **Stopping a continuous run no longer strands its finished segments.** The
  stop message pointed at "Save report now", which could only ever save the
  last single sweep; it now offers to save the completed segments as the
  session report.
- The save dialog said `{time}` gives HHMMSS; it gives HHMM.

## [0.6.3] - 2026-08-25

The product description had outgrown the app. It still promised a fixed
20-case test between two laptops, which stopped being the whole story once
the grid became editable, the Multi-Client tab arrived, and Loopback made a
second machine optional.

### Changed

- **The tagline is now what the app actually does** — "PingPair automates
  latency and throughput testing on your network — configurable sweeps,
  multi-client load tests, or a single-PC run — and turns every test into a
  ready-to-file Word, PDF or Excel report." It is rolled out everywhere the
  old one lived: the website hero, page titles, social preview cards, search
  metadata and both FAQ answers; the About tab; the first-boot welcome brief;
  and the README openers.
- **The Quick Start tour and the site's capability cards match it.** The tour
  now opens on "a configurable grid of test cases — 20 cases by default"
  rather than "a fixed 20-case LAN test", and the whole tour says "machine"
  where it used to say "laptop" — the two ends have been able to be VMs or
  embedded computers for a long time. The site's first capability card is now
  "Configurable sweep, hands-off".
- **The user guide frames the 4 x 5 grid as the default it is**, not as the
  only grid, and points at the Config tab for changing it.

### Fixed

- **Two timing figures left over from 0.6.0.** That release paced fping's
  probe count so a case stops ~17 s past its own end, cutting a full sweep
  from ~16 minutes to ~11 — but the online docs still said "16 minutes for
  the full 20 cases" and the user guide still described a case as taking
  "~50 s". Both now read the real numbers (~11 minutes, ~31 s per case),
  checked against `estimate_case_wall_s` rather than against the old prose.

## [0.6.2] - 2026-08-19

Two findings from reading the app log after a v0.6.1 run.

### Fixed

- **A Multi-Client run is no longer reported as a corrupt file.** Multi-Client
  runs are saved into the same reports tree as sweeps and are deliberately
  excluded from the Analysis tab, which compares 20-case sweeps. That
  exclusion was signalled with the same error as an unreadable file, so the
  folder scan counted a perfectly good report as a parse failure: the log said
  `skipped malformed sidecar`, and the status bar pinned `0 run(s) loaded ·
  1 couldn't be parsed (see log)` — the wording, and the never-auto-clearing
  treatment, reserved for genuine corruption. "Not a sweep" is now its own
  signal: the scan passes over such a run without counting it, notes it once
  at INFO, and does not re-examine it on every later scan. A sidecar that
  really is unreadable is still counted and still warns.

### Added

- **Saving a report set is recorded in the log** — one line naming the folder
  and the formats written. A run's whole output previously left no trace, so
  the log could not corroborate that a run had happened or where its files
  went; that matters most for an automatic save the operator never pressed a
  button for.

## [0.6.1] - 2026-08-19

Two findings from reading a v0.6.0 multi-client run — its live log and the
five report files it saved.

### Fixed

- **A clean run no longer closes its live log claiming a lost ping.** The
  Multi-Client log summarises fping into one line a second, and counted any
  line containing "unreachable" as a lost probe. fping's end-of-run summary
  block contains the row `       0 unreachable` — so **every** run added one
  loss that never happened, and swallowed that row on the way, leaving a
  summary that read `1 alive` then `0 unknown addresses` with a hole between
  them. A run measured at 1,886 sent / 1,886 received ended its log with
  `(1 lost)` while the report saved beside it read 0.00 % loss. Only lines
  carrying a probe's own `<target> : [<seq>],` stem are counted now, so a
  timed-out probe still counts exactly once and fping's ICMP-unreachable
  warnings — which describe a probe that times out and is counted on its own
  line — are shown verbatim instead of being counted a second time.
- **The same section is named the same way in every Multi-Client report
  format.** One saved run's `.docx` headed a table "Per Simulated Client"
  while its `.txt` headed the same table "Per simulated client", and the
  workbook's load-calculation sheet disagreed with its own title cell. The
  headings were per-writer literals; they are shared constants now, in the
  document title case the sweep's reports already use.

## [0.6.0] - 2026-08-19

A review pass over speed, data integrity and silent failures. The headline
change corrects measured numbers in every sweep report, so results produced
by 0.5.1 and earlier are not directly comparable with results produced by
this version — see the first entry.

### Fixed

- **Each case's latency and loss are now measured over the same window as
  its throughput.** fping is asked for a fixed number of probes, and that
  number was worked out as though `-p 10` meant one probe every 10 ms. It
  does not: the bundled fping cannot send faster than about one probe every
  **15.9 ms** — measured at `-p 10`, `-p 12` and `-p 15`, which all deliver
  300 probes in 4.78 s. So a 30 s case asked for 3000 probes, fping needed
  47.7 s to send them, and iperf3 had stopped after 30. The last ~1113
  probes — **37 % of every case's ping sample** — were collected on an idle
  link and then averaged into the case's reported minimum, average and
  maximum latency and into its loss ratio, pulling the latency down and
  diluting the loss. The count is now derived from the period fping can
  actually keep, so it stops when the load stops.

  This was documented, in the Help tab and the README, as an unavoidable
  consequence of Windows' ~15.6 ms timer granularity. It is not: raising the
  system timer resolution to 1 ms with `timeBeginPeriod` leaves fping's
  pacing unchanged (3.185 s → 3.123 s over 200 probes, inside noise). The
  floor is inside fping's own Cygwin loop. Framing it as an OS limit is why
  nobody asked whether it had to cost what it cost.

- **A 20-case sweep now takes about 11 minutes instead of about 16.** Same
  cause: the case runner blocks on fping, so fping's overshoot set the pace.
  A 30 s case costs ~31 s rather than ~48. No measurement was shortened —
  the ~5½ minutes removed from a sweep were spent pinging an idle link.

- **Watching a sweep no longer competes with measuring it.** The Single-
  Client tab redrew the latency chart and appended to the live log once per
  incoming fping reply, ~63 times a second — measured at 3.05 ms per line
  offscreen, so a 30 s case spent **5.76 s of GUI thread**, ~19 % of a core
  continuously, on the machine generating and timing the traffic. Lines are
  now buffered and the curves repainted on a 100 ms timer, the same
  treatment Round-32 gave the Multi-Client log: 962 ms for the same case.
  The chart's point buffers are also flat lists rather than tuples, which
  removes a per-line rebuild that grew with the case. The Ping tab's log got
  the same buffering.

- **A control-channel read timeout now bounds the whole call.** It was
  applied once and then re-armed by every `recv`, so a peer sending a byte
  at a time could hold a `read_message(timeout_s=15)` open indefinitely
  while the sweep waited and the UI showed nothing wrong. The socket's
  previous timeout is also restored on the way out, so a later blocking read
  cannot inherit this one's.

- **Chart PNGs that fail to write now say so.** The failure was swallowed
  whole: the operator ticked the box, got a report folder with no `Charts/`
  in it, and had nothing anywhere to explain why. Still best-effort — the
  rest of the report saves — but logged. A missing matplotlib now warns once
  at import for the same reason.

- **A truncated subprocess capture is flagged rather than returned as
  complete.** If a reader thread is still delivering output when the process
  ends, what was captured is a prefix — and downstream a truncated iperf3
  document is indistinguishable from a tool that produced nothing.

- **`nan` and `inf` can no longer reach a numeric workbook cell**, where
  Excel renders them as `#NUM!` and every formula over the column inherits
  the error.

- **The Analysis tab stopped walking the reports folder every four
  seconds.** Its refresh called `Path.resolve()` — a filesystem call — once
  per loaded run plus once per sidecar found, on the GUI thread, every tick;
  on a folder of a few hundred sweeps, or any destination on a network
  share, that is a stutter with no cause visible on screen. It now skips the
  walk entirely unless the folder's stamp changed, and identifies runs
  without touching the disk.

- **Fix-all can be interrupted by closing the window.** It runs its netsh
  calls on the GUI thread with `processEvents()` between them, so a close
  request arrives *inside* the loop; it now stops at the next boundary
  instead of continuing to drive netsh against a widget being torn down.

### Security

- The updater's two metadata reads (the release JSON and the `.sha256`
  sidecar) are bounded at 4 MiB instead of reading whatever arrives. Not a
  defence against a compromised GitHub — the SHA-256 gate is what guards the
  bundle — but against the request landing on a captive portal or a
  misconfigured mirror.

### Changed

- `fping.interval_ms` values below ~16 all produce the same rate on the
  wire. The setting is unchanged and still honoured exactly from 16 ms
  upward; it simply buys no extra resolution below that, and the Help tab's
  fping reference now says so.

## [0.5.1] - 2026-08-18

### Fixed
- **The Multi-Client live log no longer stutters.** Lines do not arrive
  evenly: one iperf3 interval event renders to one row per stream plus a
  SUM row, so a 5-client run delivered 6 lines back to back once a second
  and a 30-client run delivered 31. Appended one at a time, each was its
  own scroll and repaint — the pane stepped through 7 distinct scroll
  positions a second at 5 clients and 32 at 30, which reads as a staircase
  rather than a scroll. Lines are now written in one insert per 100 ms
  tick, so the pane moves once however many arrived together.
- **The "Generated by PingPair" line now closes the report instead of
  sitting in the middle of it.** Every saved sweep embeds an Analysis
  appendix, and the attribution was written before it — so a .docx read
  "Generated by PingPair 0.1.0" and then opened a fresh "Analysis
  appendix" heading underneath. Corrected in the .docx, .pdf and .txt
  writers, for both single- and multi-segment sweeps.
- **Word documents are A4, like the PDFs of the same run already were.**
  python-docx defaults to US Letter (216 x 279 mm) and nothing overrode
  it, so each run produced a Letter .docx beside an A4 .pdf — a mismatch
  that shows the moment either is printed.
- **The multi-client workbook's Packet Loss column stays numeric.** A
  loss below 0.005 % is reported as "<0.01" so it cannot be mistaken for
  a clean run; in a spreadsheet that put a text cell in a numeric column,
  and Excel silently excludes text from AVERAGE and from charts. The cell
  now holds the true value with a number format that still displays
  "<0.01".
- **The multi-client sidecar's config snapshot describes the run it is
  filed with.** The snapshot came from the saved profile, so a 5-client
  run saved a file reading `clients_requested: 5` at the top and
  `cfg_snapshot.multi_client.clients: 30` below it.

## [0.5.0] - 2026-08-18

### Added
- **The Multi-Client tab has a live log.** Until now the tab went silent
  for the length of a run and the only way to watch a multi-client test
  happen was to sit at the server's own terminal — the per-second
  throughput rows were being printed there and nowhere else. They now
  stream into a pane on the client desktop, with a one-second latency
  summary from fping beside them so a spike can be read against the
  throughput row from the same second. The Single-Client tab has always
  had a live log; this closes the gap.

  fping and iperf3 do not run at comparable rates — fping probes about
  sixty times a second where iperf3 reports once — so the log summarises
  each second of replies into one line carrying the count and the
  min/avg/max round trip. Passed through raw (measured: 418 fping lines
  to 21 from iperf3 on a four-second run) every throughput row would be
  pushed off the pane before the next arrived, and the log would contain
  what you came for without ever showing it.

  Mechanically, iperf3 is now asked for `--json-stream` rather than
  `--json`. Both carry the `end.streams[]` block the per-client table is
  built from, but `--json` withholds every byte until the process exits.
  The runner folds the line events back into the `--json` document the
  parsers already read, so nothing downstream of it knows the difference
  and the measurement is unchanged.
- **Saving a multi-client report shows a busy indicator.** Writing five
  formats took long enough that the click looked like it had done
  nothing. The sweep tab has shown a modal "Saving report…" since it
  gained one; Multi-Client now does too, and it comes down before any
  success or failure dialog can open behind it.

### Fixed
- **The workbook's headers stopped losing their units.** Sizing a column
  to the header's longest word wrapped `Minimum Latency (msec)` onto
  three lines inside a row pinned to two, which cut off the `(msec)`.
  Headers keep the line break the shared constants already carry, and
  each column is sized to its longest line.
- **The live log leaves room for its own scrollbar.** It does not wrap,
  so the horizontal scrollbar sits inside the pane and was covering the
  newest line — the only one anybody is watching.
- **A failed run no longer signs off by saying it finished.** iperf3
  emits an empty `end` event straight after an error one, and the log
  was announcing "test finished — collecting the per-client summary" for
  it. No summary ever arrived.
- **An aborted run keeps iperf3's own words.** Reassembling a truncated
  stream produces valid JSON with an empty summary block, which reaches
  a different error path than truncated text did — one that dropped the
  stderr line saying why the run died.
- **The multi-client PDF stopped breaking its own tables.** The cover's
  run-info table was free to split wherever the page ran out, which
  stranded its last three rows — Client capacity, fping, iperf3 — alone
  on a page of their own, under nothing and above nothing. Every section
  is now one block: heading, intro and table travel together, and a
  five-client report is two pages where it was three.
- **The multi-client PDF's tables are centred and fill the page.** They
  were pinned to the left margin of a landscape page a seven-column
  table covered half of. The page is portrait now, matching the .docx it
  is usually read beside; headers wrap onto two lines as they do
  everywhere else, rows are banded, and the totals row is shaded.
- **Every multi-client document names the build that wrote it.** The
  .docx and .pdf carried no footer at all — a test record that cannot
  say which version measured it is the one thing it must never be. Both
  now end with the same "Generated by PingPair <version> ·
  www.pingpair.app" line every other PingPair document carries.
- **The five formats agree on their dress, not just their numbers.** The
  text report drew its own 78-column left-aligned band with the raw URL
  and no footer; it now uses the sweep report's centred header and
  footer helpers. The workbook opened with a bold string; it now opens
  with the same brand band — logo, two-tone wordmark, version, clickable
  site link — as the sweep's.
- **The workbook's grids are usable.** Header rows freeze, so a
  thirty-client table no longer scrolls its own headings away; numbers
  carry the format their column deserves, so a cell that reads `0.00` in
  the .docx no longer reads `0` in Excel; and columns are sized to their
  contents instead of a flat sixteen characters that truncated
  "Maximum Latency (msec)".
- **The text report's numeric columns line up.** Every column was
  left-justified, which put the decimal point in a different place on
  every row and made a column of latencies unscannable. Numbers are
  right-aligned now, labels stay left, and no row carries trailing
  padding.

## [0.4.5] - 2026-08-18

### Fixed
- **A dead server is reported once again, not twice.** 0.4.3 moved the
  one-second iperf3 test ahead of the plain port probe, and took the
  sequence's early exit with it. A server whose first port refused was then
  swept for the whole listener range anyway — up to twenty more seconds
  spent restating the red row directly above it, which is exactly the
  "three red pills for one cable" the check order exists to prevent.
- **The connectivity check stops provoking the very error 0.4.3 removed.**
  On that same path the range sweep re-probed the port the probe had just
  reported, so a check against a half-open server left *two* `unable to
  receive cookie` lines in the customer's log where 0.4.3 had promised
  fewer. The base port is now probed at most once, whatever the outcome.
- **"Last sweep" can no longer name the oldest run on disk.** Saved sweeps
  were ordered by file modification time alone, so runs written in the same
  burst — or a Reports folder that had been copied, restored or robocopied,
  which rewrites every timestamp at once — came back in an order the
  filesystem chose. Whichever landed first was then captioned "Last sweep"
  and offered to **Save report now**. The run's own name breaks the tie, so
  the newest run is the newest run, on every launch. The Analysis tab's run
  list is sorted by the same rule; it is now literally the same code.

### Changed
- **The shipped screenshots are current, and reproducible.** The Multi-Client
  figures still showed the pre-0.4.0 command layout, and the Config figures
  had never matched the input hints that shipped alongside them. Separately,
  the capture harness was writing its own randomly-named temporary directory
  into the documented "Destination folder", so three images changed on every
  capture no matter what the app did — hiding real drift in the noise and
  publishing a path no user will ever have. Two captures of unchanged code
  are now byte-identical apart from the Server event log's clock.

## [0.4.4] - 2026-08-18

### Fixed
- **A Multi-Client run now ends the moment the far end does.** Stopping the
  server mid-test was detected immediately by iperf3 but not *reported* until
  the run's remaining ping traffic had finished — so killing the server nine
  seconds into a thirty-second test left the Client showing "Running…" for
  another forty. The run is now abandoned as soon as iperf3 reports a
  failure, instead of waiting out a ping series describing a test that is no
  longer happening.
- **A lost cable is detected in about a second.** A physical disconnect sends
  no close of any kind and a UDP sender keeps transmitting regardless, so
  iperf3 can take minutes to notice one. The run now watches the local
  adapter's carrier alongside iperf3 — the same signal the 20-case sweep
  uses — and reports the lost link by name rather than as a downstream iperf3
  error. A run the operator stopped is still never reported as a cable fault.

## [0.4.3] - 2026-08-18

### Changed
- **A successful connectivity check no longer leaves an error in the server's
  log.** *iperf3 port open* used to open a plain TCP connection and abandon
  it, which made the server print `unable to receive cookie at server: Bad
  file descriptor` — in red, on the customer's machine — every single time.
  The result is now concluded from the one-second test instead, which is
  stronger evidence anyway: it proves the thing on the port really is iperf3,
  not merely that something accepts connections. The bare probe still runs
  when that test fails, because it is the only way to separate "nothing is
  listening" from "something is listening but it is not iperf3" — so the
  message now appears only beside a check that already went red. A
  multi-listener sweep also skips the port the test already proved.

## [0.4.2] - 2026-08-18

### Fixed
- **The Multi-Client page no longer jumps when you click Run or Save.**
  Both buttons disable themselves, Qt hands focus to the next control when
  that happens, and the scrolling page followed it — throwing the view
  roughly 400 px away from the button just clicked. The page now holds its
  position, and the read-only command panes are out of the tab order, where
  they never belonged.
- **Saving from the post-run dialog no longer raises a second dialog.**
  Choosing *Save* was answered with a *Report saved* box on top of it. The
  status line reports the outcome instead, matching the Single-Client sweep.
  The manual *Save report…* button still confirms, since no dialog precedes
  it, and failures are still modal everywhere.

## [0.4.1] - 2026-08-18

### Fixed
- **A finished Multi-Client run now asks where to save it.** With Auto-save
  off, the run simply sat on screen and had to be rescued with the *Save
  report…* button — contradicting the Save Options tab's own description of
  the setting, which promises a prompt after each test. The Single-Client
  save dialog is now shared by both run modes, including its *Don't ask me in
  the future* option.
- **The per-client results table no longer clips its rows.** At five clients
  the last row was sliced through the middle by the widget edge, while the
  Ping table below it kept space it had no rows to fill. Both tables are now
  sized to what they hold; past twelve clients the table scrolls.
- **The server-command blocks are readable and unambiguous.** Each *Copy*
  button now sits against the command it copies instead of out at the right
  margin, the command text is larger, every block is the same width, and a
  long line is no longer hidden behind a scrollbar drawn over it.

## [0.4.0] - 2026-08-18

### Changed
- **Reports open with a cover page.** The branding block and the run's
  identifying facts have page one to themselves in Word and PDF; the
  measurements start overleaf. Related tables now stay on one page where they
  fit, and a section heading can no longer be stranded at the foot of a page
  with its table on the next.
- **The PingPair version is named under the wordmark**, in every report format,
  instead of as a row inside the run-information table.
- **Threshold fields on the Config tab hint instead of pre-filling.** An
  unticked limit shows a muted `0.00` you type straight over, rather than a
  real zero you had to select and delete first. Ticking a limit shows its value
  literally, so an armed `0` still reads as the limit it is.
- **The Server no longer reports a finished sweep as a disconnect.** When the
  Client closes the control channel at the end of a run — which it always does
  — the Server's panel says the sweep finished, instead of "(disconnected)".

### Fixed
- **Sub-1 % ping loss no longer reports as zero.** fping rounds its own loss
  figure to a whole percent, so a Multi-Client run that lost 7 of 3,000 pings
  showed `3,000 sent / 2,993 received / 0.00 %` — three numbers that cannot all
  be true. Ping loss is now computed from the sent and received counts, in the
  table and in every saved report. The 20-case sweep was never affected: it
  takes its loss figure from iperf3, not from fping.
- **Saved test-plan profiles survive an update.** They were written inside the
  installation's `_internal` folder, which an in-app update replaces wholesale
  — so updating deleted them. Profiles now live in a `Configs` folder beside
  `PingPair.exe`, next to `Reports`; the update explicitly skips both, and any
  profiles left in the old location are moved across on the next launch.

## [0.3.0] - 2026-08-08

### Added
- **Multi-Client tab** — drive many simulated clients at once from this laptop
  against a single server, to see how the server's LAN side behaves under
  realistic multi-workstation load. One manually parameterised test case, a
  per-client results table, and a live capacity budget that tells you whether
  this machine can actually generate the load you asked for (and blocks the run
  if it can't). The server needs no PingPair — pick your Linux distribution and
  copy the generated commands.
- **Reference values** — set threshold values for throughput, jitter, packet
  loss and latency, and every result row is judged **PASS / MARGINAL / FAIL /
  NOT EVALUATED**. Reports gain a verdict banner, reference columns beside the
  results, the reference table itself, and an exceptions table listing only the
  rows that breached. Throughput limits can be a percentage of each case's
  offered bandwidth, which is the only thing that means anything across a
  10→90 Mbps sweep. Ships with a standards-backed default profile (ITU-T
  G.114 / Y.1541).
- **Threshold lines on the Analysis charts** when every selected run was judged
  against the same reference.
- **CSV export** of results, for spreadsheets and scripts.
- **Saved profiles** now carry the network settings, both run modes and the
  thresholds — a site's whole test setup in one file, with a one-click picker
  on the Config tab.
- **Help gained two sections** — *Multi-Client* and *Reference values* — plus
  refreshed screenshots throughout, matching the current interface.

### Changed
- The **Run** tab is now **Single-Client**, beside the new Multi-Client tab.
- The Config tab serves both run modes through a selector that follows the
  active run tab, and its layout was reworked into a single scrolling column.

### Fixed
- Config tab: overlapping toolbar controls, a clipped caption, and a group
  title colliding with the panel below it at smaller window sizes.

## [0.2.4] - 2026-06-26

### Changed
- Maintenance release — no functional changes from 0.2.3. Marks the updater
  fixes (0.2.2 certificate trust, 0.2.3 install/swap) as verified stable on a
  clean install, and validates the in-app update path 0.2.3 → 0.2.4.

## [0.2.3] - 2026-06-25

### Fixed
- **In-app updates now install correctly.** After downloading an update and
  confirming the restart, the app could close and reopen on the *same* version
  — the update silently didn't apply. The background updater no longer locks
  itself out of its own install step, so the new version is installed and
  launched as expected.

## [0.2.2] - 2026-06-25

### Fixed
- **Update check now works on fresh Windows machines.** "Check for updates"
  could fail with "Couldn't reach GitHub" / a TLS certificate error
  (`CERTIFICATE_VERIFY_FAILED`) on a newly set-up Windows PC or VM whose
  certificate store hadn't yet downloaded GitHub's root CA. PingPair now ships
  its own trusted CA bundle, so the update check verifies correctly regardless
  of the machine's certificate-store state — no more one-time browser/`curl`
  workaround.

## [0.2.1] - 2026-06-25

### Fixed
- Removed an erroneous "you are running the self-updated build — the updater
  worked!" banner that appeared on the About tab.
- The Excel **comparison** export now leaves a 100%-loss case's latency cells
  blank — consistent with the Word/PDF/TXT reports — instead of writing a
  `nan` value that some Excel versions flagged as a corrupt workbook.
- Hardened report generation against rare failures: a branded-header glitch can
  no longer abort a whole report, and a 100%-loss case no longer skews the
  per-case analysis charts.

## [0.2.0] - 2026-06-25

### Changed
- Report headers are now fully branded across every format. Each Word, PDF,
  TXT, and Excel report — single-sweep and Analysis comparison alike — now
  leads with the PingPair logo, a two-tone "PingPair" wordmark, and a
  clickable www.pingpair.app link above the title, with a matching site link
  in the footer.

### Fixed
- The Analysis tab's "Loaded runs" list no longer clips or overlaps the run
  entries on higher-DPI displays. The list now fills its panel, and transient
  load/parse messages appear in the main-window status bar instead of a
  cramped in-pane label.

## [0.1.0] - 2026-06-09

### Added
- Automated 20-case Server/Client LAN sweep (fping + iperf3) with Word /
  Excel / PDF / TXT reports and a JSON sidecar per run.
- Setup prerequisite checker with one-click fixes, role auto-detect, Config
  test-plan editor, Analysis comparison/diff tab, Help guide, and theming.
- **In-app updater (Feature 6).** About → Updates: "Check for updates"
  (manual), an every-launch update reminder with a "Don't remind me again"
  opt-out, and "Download & install" that downloads the new build from the
  GitHub release, verifies its SHA-256, swaps the install, and relaunches.
  Cancel button for an in-flight download; live speed/ETA readout.
- About tab reorganized into four cards (About / Updates / Credits & License /
  Diagnostics).
- The PingPair logo now appears in the header of every Word and PDF report —
  single-sweep, multi-segment, and the Analysis comparison exports.
- Packaging: top-level `LICENSE`, expanded `THIRD_PARTY_LICENSES.md`, and this
  changelog.

### Changed
- Download streamed in 1 MiB chunks (was 256 KiB) for faster transfer.
- The Analysis tab's **Export comparison report** now writes a single
  self-contained folder (`<name>/` plus `<name>/Analysis_Images/`) with
  consistently sized wide charts.
- A sweep is now blocked while Wi-Fi is connected to the test subnet (it would
  otherwise corrupt the measurement), and the Ethernet adapter is reverted to
  DHCP when the app closes.

### Security
- Update download now refuses an `https`→`http` redirect downgrade (strict-HTTPS
  opener), in addition to the existing mandatory SHA-256 check. The release
  bundle remains **unsigned** for v1 — HTTPS + SHA-256 is the integrity story;
  code-signing is a documented post-v1 item.
- All elevated system-tool calls (`netsh` / `ping` / `icacls` / `cmd`) are now
  invoked by absolute `%SystemRoot%\System32` path, removing a PATH/cwd
  binary-planting privilege-escalation surface.
- The Setup-tab NIC override is validated as an IPv4 literal before it reaches
  `netsh`; the report filename pattern is sanitized so it can't write outside
  the Reports folder.

### Fixed
- Self-update now actually applies and relaunches: the swap helper no longer
  uses `timeout` (which fails with no console); it uses `ping`-based waits,
  logs to a file, checks robocopy's exit code, and runs in its own console.
- Closing the window mid-download no longer freezes the app.
- A 100%-loss test case no longer renders the literal text `nan` in the latency
  columns (Word / PDF / TXT), writes a malformed `NaN` token into the JSON
  sidecar, or skews the summary statistics — those latencies now show as "—".
- The Analysis tab's **Export comparison report** now works in every format (it
  previously failed with an internal error for all formats).
- iperf3 results that report a connection error are now recorded as a failed
  case instead of a misleading 0 Mbps measurement.
- The server no longer stalls ~10 seconds (leaving an orphaned iperf3 process)
  when a client disconnects mid-case.
- "Reset to defaults" on the Save Options tab now restores Word + Excel only
  (the documented default), instead of also turning PDF and TXT on.
- The Analysis Diff tab's hint banner is now readable on the Light theme.

[Unreleased]: https://github.com/mhmd2520/Linkproven/compare/v1.1.5...HEAD
[1.1.5]: https://github.com/mhmd2520/Linkproven/releases/tag/v1.1.5
[1.0.0]: https://github.com/mhmd2520/Linkproven/releases/tag/v1.0.0
[0.6.3]: https://github.com/mhmd2520/PingPair/releases/tag/v0.6.3
[0.6.2]: https://github.com/mhmd2520/PingPair/releases/tag/v0.6.2
[0.6.1]: https://github.com/mhmd2520/PingPair/releases/tag/v0.6.1
[0.6.0]: https://github.com/mhmd2520/PingPair/releases/tag/v0.6.0
[0.5.1]: https://github.com/mhmd2520/PingPair/releases/tag/v0.5.1
[0.5.0]: https://github.com/mhmd2520/PingPair/releases/tag/v0.5.0
[0.4.5]: https://github.com/mhmd2520/PingPair/releases/tag/v0.4.5
[0.4.4]: https://github.com/mhmd2520/PingPair/releases/tag/v0.4.4
[0.4.3]: https://github.com/mhmd2520/PingPair/releases/tag/v0.4.3
[0.4.2]: https://github.com/mhmd2520/PingPair/releases/tag/v0.4.2
[0.4.1]: https://github.com/mhmd2520/PingPair/releases/tag/v0.4.1
[0.4.0]: https://github.com/mhmd2520/PingPair/releases/tag/v0.4.0
[0.3.0]: https://github.com/mhmd2520/PingPair/releases/tag/v0.3.0
[0.2.4]: https://github.com/mhmd2520/PingPair/releases/tag/v0.2.4
[0.2.3]: https://github.com/mhmd2520/PingPair/releases/tag/v0.2.3
[0.2.2]: https://github.com/mhmd2520/PingPair/releases/tag/v0.2.2
[0.2.1]: https://github.com/mhmd2520/PingPair/releases/tag/v0.2.1
[0.2.0]: https://github.com/mhmd2520/PingPair/releases/tag/v0.2.0
[0.1.0]: https://github.com/mhmd2520/PingPair/releases/tag/v0.1.0
