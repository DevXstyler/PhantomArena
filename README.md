# PhantomArena 🕶️⚔️
> [!NOTE]  
> This is a concept - Nothing real YET but development is starting soon. ;D


> **PhantomArena** — a competitive, simulation-first PvP hacking arena where Hackers and Defenders face off in isolated, disposable virtual environments.  
> Realism vibes, game-first design. Play, learn, compete — within a fully controlled, simulated ecosystem.

---

## Table of Contents
- [High-Level Overview](#high-level-overview)
- [Core Design Principles](#core-design-principles)
- [Game Modes & Flow](#game-modes--flow)
- [Roles](#roles)
  - [Defender](#defender)
  - [Hacker](#hacker)
- [VM & Environment Options](#vm--environment-options)
- [Tasks, Emails & Websites (In-Game)](#tasks-emails--websites-in-game)
- [Scoring, Ranks & Rewards](#scoring-ranks--rewards)
- [Matchmaking & Session Rules](#matchmaking--session-rules)
- [Anti-Abuse & Moderation](#anti-abuse--moderation)
- [Technical Architecture (High Level)](#technical-architecture-high-level)
- [Privacy, Safety & Responsible Use](#privacy-safety--responsible-use)
- [How to Get Started (Developer Notes)](#how-to-get-started-developer-notes)
- [Contributing](#contributing)
- [License](#license)

---

## High-Level Overview
PhantomArena is a competitive simulation game that reproduces many high-level aspects of offensive and defensive cybersecurity but **never** requires or provides real-world destructive instructions. Players choose a role (Hacker or Defender) and compete in rounds inside ephemeral, provider-supplied VM environments. The core objective is to complete role-specific objectives: Defenders must finish tasks while hardening their VM; Hackers must use in-game tools to compromise a target VM state.

---

## Core Design Principles
- **Game-first, simulation-second** — Prioritize fun, clarity, and repeatable mechanics over real-world fidelity. Simulate outcomes rather than perform real destructive actions.
- **Isolation** — Every match runs from a clean snapshot; no access to the real host or external internet.
- **Deterministic results** — Attacks and defenses resolve according to game rules and telemetry, enabling fair judging and replay.
- **Auditability** — All actions in a match are logged in a tamper-evident replay format for dispute resolution and learning.

---

## Game Modes & Flow
1. **Lobby / Match Setup** — Players choose mode, select VM image (Windows / Linux / MacOS simulation), and join as Hacker or Defender.
2. **Preparation Phase (5 hours in-game timer)**  
   - *Defenders* configure their supplied antivirus (game-configurable) and choose a set of tasks to complete. Tasks are lightweight, in-game activities (e.g., copy text, run mini-game, verify file).  
   - *Hackers* design simulated attack artifacts (payload descriptors, phishing templates, delivery chains) using provided in-game toolkits and earn upgrade points.
3. **Execution Phase**  
   - Hackers trigger their prepared attack sequences against defender VM(s) via in-game channels (email, fake websites).  
   - Defenders monitor alerts, investigate, and attempt to finish remaining tasks or mitigate attacks.
4. **Resolution**  
   - If Defenders complete required tasks before irrevocable compromise, they win.  
   - If Hackers achieve the match’s compromise objective (e.g., simulated OS destruction flag, persistence token, or task file deletion), Hackers win.
5. **Post-Match** — Score, replay, telemetry snapshots, and rewards are distributed. VMs are reverted to snapshots.

---

## Roles

### Defender 🛡️
- **Goal:** Complete assigned tasks and keep your VM "intact" until the safe window expires.
- **Preparation Window:** 5 hours (game-time) during which they may:
  - Configure the provided in-game antivirus (tuning rules, false-positive thresholds).
  - Select and partially complete tasks (convert remaining time to task progress).
- **Mechanics:**  
  - Defenders receive two randomized in-game emails and a set of in-game website links to inspect.  
  - If a Defender interacts with a harmful link (falls for simulated phishing), the Defender gains a "compromised" marker and becomes targetable by more aggressive in-game follow-ups.
- **Victory Condition:** Complete the task list (or reach required progress) before a critical compromise condition triggers.

### Hacker 🕵️‍♂️
- **Goal:** Compromise targeted Defender VMs according to match objectives.
- **Team Size:** Up to 25 per group (starts smaller; can be upgraded).
- **Preparation Window:** 5 hours (game-time) to:
  - Compose simulated malware descriptors and phishing campaigns (in-game artifacts, not real binaries).
  - Earn and spend upgrade points for tools, templates, or team capacity.
- **Mechanics:**  
  - Hackers send in-game phishing emails and website lures to randomized defender inboxes (game-managed).  
  - If a Defender engages, the Hacker can execute an attack chain (resolved by game simulation rules).
- **Victory Condition:** Trigger the scenario-defined compromise (e.g., simulated OS destroyed flag, deletion of task files, gaining persistence token).

---

## VM & Environment Options
Players choose among simulated VM images:
- **Configured Windows (simulated)** — typical Windows-style environment for gameplay flavor. Gets Tasks (for Defenders) done quicker than on other OS but is the most dangerous (as most malware is for windows)
- **Custom Linux Distro (simulated)** — lean for different attack/defense vectors. Best for hackers, lots of malware tools. Safe for Defenders but also slow
- **Configured macOS (simulated)** — different toolset and signature behavior. A perfect mid for defenders, mid-secure and mid-speed but horrible for Hackers

> Note: All images are **game simulations**. Destructive effects are represented by game state changes (e.g., “Destroyed” flag), not by altering real disk or host data.

---

## Tasks, Emails & Websites (In-Game)
- **Tasks** are lightweight, discrete objectives (copy a text, install/launch an in-game app, solve a small puzzle).
- **Emails & Websites** are synthetic and exist solely in the game fabric (unique domains, non-routable addresses). They are designed to mimic phishing patterns for gameplay, not to interact with the real internet.
- **Scam Radar:** Interacting with known-scam artifacts marks the Defender on a radar available to Hackers (game mechanic) enabling additional attack vectors — balanced to avoid deterministic outcomes.

---

## Scoring, Ranks & Rewards
- **Multidimensional scoring:** Winning/losing is augmented by metrics:
  - **Time-to-Compromise / Time-to-Complete**
  - **Stealth Factor** (how silent an attack/defense was)
  - **Task Completion Rate**
  - **Resource Efficiency**
- **Ranks:**  
  - Rank ladders control matchmaking and difficulty (progressive tasks; occasional "boss" tasks).  
  - Ranks grant cosmetic rewards, utility upgrades, or sandbox access; **no pay-to-win** in core PvP.
- **Rewards:** Upgrade points, cosmetic items, and non-destructive convenience features.

---

## Matchmaking & Session Rules
- **Preparation windows** apply once per session start or after a reset following a match result.
- **Automated Kick:** Players who fail to prepare within the allocated window get removed from the active team.
- **Group Limits:** Founders start with smaller groups; group size can be upgraded via in-game progression.
- **Balance:** MMR and rank-based pairing reduce mismatch between pros and newcomers.

---

## Anti-Abuse & Moderation
- **Immutable replay logs** for adjudication.
- **Automated health checks**: abnormal behavior triggers quarantine and admin review.
- **Reputation system** with consequences for repeated abuse (temporary bans, rank reset, or permanent block).
- **No real-world attack vectors**—all actions resolve within the game domain to limit abuse and legal exposure.

---

## Technical Architecture (High Level)
- **Game Server (Authoritative):** Coordinates matches, stores game state, manages synthetic emails/sites, and handles matchmaking.
- **VM Provisioner:** Instantiates ephemeral, clean VMs from signed snapshots for each match (ephemeral by default).
- **Simulation Engine:** Interprets attack/defense descriptors and resolves effects to game state (not host state).
- **Telemetry & Replay Storage:** Append-only logs of actions and events for replays and moderation.
- **Client UI:** Presents VM UI, consoles, mail clients, and in-game toolkits; provides controlled APIs to build simulated artifacts.
- **Admin Tools:** Forensics viewer, replay playback, and manual dispute resolution.

> Implementation detail note: the README intentionally omits low-level destructive mechanics; design the Simulation Engine to translate high-level player actions into deterministic game outcomes.

---

## Privacy, Safety & Responsible Use
- All in-game emails, websites, and artifacts are synthetic. No real domain or real personal data should be used in production.
- Player data retention, telemetry, and moderation logs should comply with applicable privacy laws and internal policies.
- The system must enforce strict isolation between the game environment and the player's host machine.

---

## How to Get Started (Developer Notes)
- **Prototype first:** Implement a minimal Simulation Engine and single-match flow with deterministic outcomes.
- **Iterate on UX:** The realism feel comes from UX — fake alerts, believable mail clients, convincing replays — not real exploits.
- **Build telemetry early:** Replays and logs are core for balance, ranking, and trust.
- **Start closed:** Run internal tests and staged alphas before opening to larger audiences.

---

## Contributing
Contributions are welcome for UI, simulation rules, task design, balance, and moderation tooling. Follow the `CODE_OF_CONDUCT` and write tests for simulation determinism.

---

## License
Pick a license that matches your business and contribution model.

**Recommended:** `MIT License` (permissive) — simple and common for indie projects.  
If you want stronger copyleft to keep derivatives open, consider `AGPLv3`.  
If you prefer a closed commercial model, keep the repository private and use a proprietary license.

---

## Quick FAQ
**Q:** Is real malware created or distributed?  
**A:** No. PhantomArena exclusively uses simulated artifacts that affect only game state.

**Q:** Are emails/websites real on the internet?  
**A:** No. All are synthetic and routed within the game's controlled backend.

**Q:** Can I practice real offensive skills here?  
**A:** You can practice high-level strategy, attack sequencing, and defensive thinking — but not real-world destructive techniques.

---

## Contact / Support
For technical support, moderation appeals, or developer inquiries, contact the project admins inside the game’s admin dashboard.

---

**Play fair. Learn hard. Fight with code-shaped ideas — not real-world risk.**
