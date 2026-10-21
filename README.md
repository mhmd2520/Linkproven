# LinkProven

**Automated network latency and throughput testing for Windows.** LinkProven
drives hands-free, configurable test sweeps between two Windows machines using
[`fping`](https://github.com/schweikert/fping) (latency / loss) and
[`iperf3`](https://github.com/esnet/iperf) (throughput / jitter / loss), then
produces a Word + Excel report (PDF / TXT optional) plus the JSON config that
produced it.

The two endpoints can be physical laptops, Windows VMs on a private LAN
segment, or embedded computers on a train's on-board network — the tool
treats them the same as long as both sides share a layer-2 broadcast domain.

---

## What it does

For every `(payload, bandwidth)` pair in a 4 × 5 grid — payloads
`200 / 600 / 1000 / 1300 B`, bandwidths `10 / 30 / 50 / 70 / 90 Mbps` —
LinkProven runs one ~30 s test case, then moves to the next. All **20 cases**
run in sequence with no manual intervention.

```
Laptop A — Server  192.168.1.1   ──  Ethernet  ──  Laptop B — Client  192.168.1.2
   iperf3 -s                                          iperf3 -c  +  fping  (per case)
```

Each run produces a per-sweep folder under `Reports/` containing a Word
document, an Excel workbook, and a `.json` sidecar — with the 8-column
results table (throughput, jitter, loss, min/avg/max latency).

## Features

Ten pages, in the order you work through them.

- **Setup** — one-glance prerequisite checker (admin, tools, NIC IP, firewall
  rules, Wi-Fi) with copy-paste fixes and opt-in "Fix this for me" buttons.
  Never modifies the host without confirmation. Also holds the role switcher
  (Server / Client / Loopback) and the Light / Dark / System theme.
- **Ping** — a quick reachability check with a role-aware default target,
  before you commit to a sweep.
- **Config** — full test-plan editor: payloads, bandwidths, duration, protocol,
  ports and IPs, edited by form or raw JSON with live two-way sync, and kept as
  a library of reusable `.json` profiles. Also where **reference values** are
  armed — per-metric limits (throughput, jitter, loss, min/avg/max latency,
  with a percent-of-offered mode for throughput) that turn every report into a
  pass / marginal / fail judgement.
- **Plan** — the test campaign: train set, location, procedure, who tests and
  who witnesses, and every test point with its derived status (PASS, FAIL,
  REPEAT, not run) read from the saved reports.
- **Single-Client** — drives the 20-case sweep. Per-case subset selection,
  continuous multi-segment mode, a whole-sweep ETA, and live charts plus a
  live `iperf3` / `fping` log as it runs. Works against a remote Server or
  entirely on `127.0.0.1` in Loopback mode.
- **Multi-Client** — loads one server with N simulated clients in a single
  parameterised case, with a capacity budget that refuses a load this machine
  could not honestly generate, generated server-side setup commands per Linux
  distribution, and a preflight that completes a real `iperf3` handshake first.
- **Save Options** — Word / PDF / Excel / TXT / CSV output, auto-save or
  prompt-per-run, filename patterns, and test-record metadata (technician,
  customer, hardware serial, environment, record ID).
- **Analysis** — load past runs back in to overlay, compare and diff them, and
  export a comparison report. Runs judged against the same reference values
  get a threshold line on the throughput charts.
- **Help** and **About** — a generated quick-start walkthrough with diagrams
  and screenshots, plus the in-app updater.

Every run also writes a `.json` sidecar carrying every number the other formats
render, so a report can be re-read by machine as well as by a person.

## Download & install

1. Open the [**latest release**](https://github.com/mhmd2520/Linkproven/releases/latest)
   or the download page at [www.linkproven.com](https://www.linkproven.com/).
2. Download the `.zip` (or installer) **and** its `.sha256` file.
3. Optional: check the download. In PowerShell,
   `Get-FileHash .\<file>.zip -Algorithm SHA256` must print the hash in the
   `.sha256` file.
4. Unzip, then run `LinkProven.exe`. It asks for administrator rights because
   it sets the test IP and firewall rules.

No Python install is required: the bundle ships its own runtime plus `fping`
and `iperf3`. **LinkProven updates itself** from this repository's Releases
feed (About → Check for update). Each update is verified against its SHA-256
before it is applied.

**Requirements:** Windows 10 or 11 (64-bit), administrator rights, and a wired
Ethernet port on each of the two machines.

## What this repository holds

This repository is the public **release channel** for LinkProven. It holds no
source code, only:

- **Releases**: the packaged builds and their `.sha256` checksums.
- `CHANGELOG.md`: what changed in every version.
- `LICENSE`: the terms of use.
- `THIRD_PARTY_LICENSES.md`: notices for the bundled open-source components.

Questions and bug reports: [www.linkproven.com](https://www.linkproven.com/).

## License

LinkProven is **proprietary**, © 2026 Mohamed Khaled, all rights reserved,
and free to use under the terms in [`LICENSE`](LICENSE). That file states the
use grant, the restrictions and the warranty disclaimer.

Bundled third-party components (`fping`, `iperf3`, the Cygwin runtime,
Qt/PySide6, Python libraries) remain under their own licenses. See
[`THIRD_PARTY_LICENSES.md`](THIRD_PARTY_LICENSES.md) for the full notices and
the corresponding-source offer for the LGPL/Cygwin parts.
