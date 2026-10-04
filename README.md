<div align="center">

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/SaphiraLinux/brand/main/logos/saphira-linux-brand-icon-logo-1200.webp">
  <img alt="Saphira Linux" src="https://raw.githubusercontent.com/SaphiraLinux/brand/main/logos/saphira-linux-brand-icon-light-logo-1200.webp" width="600">
</picture>

# It's so Simple it Hurts.

**A small, auditable Linux distribution and software ecosystem, built around native, self-hosted infrastructure.**

[Website](https://saphira.vm2.uk/) · [Downloads](https://saphira.vm2.uk/downloads) · [Packages](https://saphira.vm2.uk/rtfm/packages) · [Why Saphira?](https://saphira.vm2.uk/why-saphira) · [Roadmap](https://saphira.vm2.uk/roadmap) · [Status](https://saphira.vm2.uk/status)

</div>

---

<table>
<tr>
<td width="50%" valign="top"><a href="https://saphira.vm2.uk/why-saphira"><img src="https://raw.githubusercontent.com/SaphiraLinux/brand/main/artwork/saphira-proof-320.webp" alt="A developer holding a slate showing the musl libc logo, sitting beside Saphira the blue dragon, Tux the penguin and a laptop" width="300"></a><br><sub><b>Saphira</b> — musl + OpenRC. The release line people run.</sub></td>
<td width="50%" valign="top"><a href="https://saphira.vm2.uk/roadmap#saphira-d"><img src="https://raw.githubusercontent.com/SaphiraLinux/brand/main/artwork/saphira-d-proof-320.webp" alt="A developer holding a book titled 'the usual way, huh?' labelled systemd, beside Saphira the blue dragon, Tux the penguin and a laptop" width="300"></a><br><sub><b>Saphira-d</b> — musl + systemd. In development.</sub></td>
</tr>
</table>

---

## What Saphira is

A from-scratch Linux distribution. Not a respin of Debian, Ubuntu, Alpine or Arch.

| | |
|---|---|
| **musl libc** | A small, strict, standards-focused C library. Predictable behaviour, small binaries. |
| **OpenRC** | Dependency-based init and service management written in shell. Scripts and dependencies, no hidden state machine. |
| **APK** | Fast, atomic package management with signed indexes and strong package boundaries. |
| **Linux + GRUB** | An ordinary upstream kernel and an ordinary, inspectable bootloader. |
| **x86-64-v3** | The modern default baseline: CPUs from roughly 2015 onwards, AVX2 class. Not for every ancient x86-64 chip. |

It is built in stages from source, so you can follow it from toolchain to boot to
service startup without the machine disappearing behind layers of machinery:

**Stage0** bootstrap toolchain → **Stage1** native toolchain → **Stage2** root
filesystem → **Stage3** base build image → **Stage4** packages and image
generation. Every input is pinned and hash-verified. No floating upstream
tarballs. Images are booted and exercised before publication.

We build software to run because it is useful, understandable and under your
control.

## Release

**Saphira Linux Earlybird Beta** — a real beta, marked pre-release. Not the full
release.

- Released 15 August 2026, shipped with **GCC 16.1.0**
- The development tree is migrating to **GCC 16.2.0** before a clean full rebuild
- **481 APK packages** in the release repository, signed indexes, served over TLS
- QCOW2 disk image for KVM/QEMU, distributed as an xz-compressed tar archive with
  a published SHA-256
- Live numbers are on the [status page](https://saphira.vm2.uk/status) — we do not
  put vanity counters on this page

```sh
# /etc/apk/repositories
https://packages.akadata.ltd/saphira/main
```

```sh
apk update
apk search nginx
apk add nginx
```

KVM/QEMU with VirtIO disk and network is the primary tested target. Xen, VMware
and VirtualBox are expected to work where VirtIO devices are available. None are
certified.

## Saphira is hosting Saphira

`https://saphira.vm2.uk/` is not a demonstration environment. It is the
distribution doing a real job — a real Node.js SSR application on musl, managed
by OpenRC, installed from the same APK repository you can use.

```console
$ curl -sI https://saphira.vm2.uk/ | grep -iE '^(server|x-(powered|saved|hosted|designed)-by)'
Server: Saphira Linux
X-Powered-By: Saphira the Dragon
X-Saved-By: Grace
X-Hosted-By: AKADATA LIMITED
X-Designed-By: AKADATA LIMITED
```

## Public repositories

Everything below is public and clonable today.

| Repository | Language | What it is |
|---|---|---|
| [`recipes`](https://github.com/SaphiraLinux/recipes) | Shell | Every Saphira-written package publishes its `recipe.sh` in the open. No hidden sources, no archives. |
| [`saphira-llm`](https://github.com/SaphiraLinux/saphira-llm) | C | Native C11 CPU inference runtime for BitNet b1.58 GGUF models. MIT licensed. |
| [`saphira-tokenizer`](https://github.com/SaphiraLinux/saphira-tokenizer) | C, Rust | Exact, portable LLM tokenization with cross-checked Rust and C implementations and deterministic token IDs. |
| [`saphira-sjev-c`](https://github.com/SaphiraLinux/saphira-sjev-c) | C | Native implementation of the SJEV decision and reranking model, with trainer, runtime and reproducible experiments. |

The Saphira Linux distribution source itself is **not public yet**. We would rather
say so here than imply otherwise.

## The Dragons

Feature families for software running on infrastructure you control.

| | |
|---|---|
| **mailDragon** | Self-hosted Postfix, Dovecot, Rspamd, ClamAV and Roundcube. Operated from the shell, not a dashboard. |
| **webDragon** | The nginx, PHP-FPM and Node.js layout, plus site and TLS certificate workflows. |
| **dnsDragon** | Authoritative BIND 9 and DNSSEC: zones, records and the delegation chain of trust. |
| **vpnDragon** | WireGuard on your own infrastructure, from one client to routed IPv6 networks. |
| **aiDragon** | An agent-friendly Saphira workspace: OpenCode, Codex, Claude Code, MCP tools and persistent project context. |
| **databaseDragon** | SQLite, MariaDB and PostgreSQL, with SQL permissions and nftables exposure explained together. |

## Saphira-d is in development

Not a replacement, a question. A **musl + systemd** sibling with its own build
tree, so an init experiment cannot silently redefine the distribution people
already run. It is the **non-usr-merged** edition, keeping the traditional
filesystem layout and proper FHS support.

It booted systemd 261.2 under `systemd-nspawn` on 24 August 2026 and reached
`graphical.target`. `systemd-logind` failed on that first boot and the systemd
packages are still awaiting signing. **It is not a complete distribution.** The
systemd packages and their musl dependency chain — Linux-PAM, libxcrypt,
libucontext, Jinja2, MarkupSafe, libmnl, libbpf — are built from source.

## Principles

- Open protocols
- Self-hosted where practical
- Minimal dependencies
- Evidence before claims
- IPv6 first-class
- Native software rather than SaaS dependency

## Bugs and disclosure

We publish SVE (Saphira Vulnerability Entry) records for defects with security,
reliability or operational significance in Saphira-developed software. An SVE is
not a CVE — it is our own public record, and the first entry,
SVE-2026-0001, is the proxyto idle-child defect: what was observed, what it
affected, how it was reproduced, how it was fixed.

- [Bugs and SVE entries](https://saphira.vm2.uk/bugs)
- [Report a bug](https://saphira.vm2.uk/bugs/report)
- [Security and coordinated disclosure](https://saphira.vm2.uk/security)

## Elsewhere

- **Website** — <https://saphira.vm2.uk/>
- **Package repository** — <https://packages.akadata.ltd/saphira/main/>
- **Checksums** — <https://saphira.vm2.uk/checksums>
- **Gopher** — `gopher://saphira.vm2.uk/` — no pictures, no noise, just the words
- **Brand assets** — [`SaphiraLinux/brand`](https://github.com/SaphiraLinux/brand) — logo and dragon artwork, [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)
- **Vendor** — [AKADATA](https://akadata.ltd)

## Licence and provenance

Made in England. British software. Free software.

Saphira remains free, including commercial use. No subscription, no rental
licence, no compulsory SaaS. Run a business on it, host customers on it, sell
services that run on it — none of that costs a licence fee.

Code AKADATA wrote specifically for Saphira is **BSL-1.1**, with each released
version transitioning to **GPL-2.0-or-later** four years after release, so it
cannot be lifted out and rebadged into somebody else's commercial product
without talking to us first. Previously released MIT versions remain MIT, and
upstream software keeps its upstream licence.

---

<div align="center"><sub>Saphira Linux is a project of <a href="https://akadata.ltd">AKADATA</a>, built in Hampshire, England.<br>
Logo and dragon artwork by AKADATA, used under <a href="https://creativecommons.org/licenses/by/4.0/">CC BY 4.0</a> — trademark rights excluded, no endorsement implied.</sub></div>