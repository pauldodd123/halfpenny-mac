# Strategy — how this makes money

Plain-language notes on positioning, pricing, and margin. Update as you learn.

---

## 1. The real customers (ranked by willingness to pay)

Don't pitch to one narrow niche. There are four distinct buyers for rented Mac minis, and they pay very differently.

| Customer | Why they need a Mac | Typical spend | How to reach them |
|---|---|---|---|
| **iOS / macOS dev teams** | Xcode CI/CD builds — you literally cannot do App Store builds anywhere else | £80–300/mo per agent, often multiple agents | iOS dev newsletters, indie Hackers, Swift communities, cold DM to agencies |
| **AI tinkerers & indie builders** | Apple silicon is unusually good for local LLM inference (llama.cpp, MLX, Ollama) on an M4 with 16–64 GB | £60–200/mo | r/LocalLLaMA, HN, X/AI Twitter, AI newsletters |
| **Browser-automation operators** | Gmail/OAuth flows hate headless Chromium; they need a real logged-in desktop browser | £50–150/mo | No-code/automation communities, scraping/automation Discord servers |
| **"I just want a Mac I can SSH into"** | Devs without a personal Mac, students, tinkerers | £30–60/mo | Low-effort SEO, Reddit, word of mouth |

**Biggest margin:** iOS CI. Teams will pay £150–300/mo per agent without blinking because the alternative (MacStadium, AWS EC2 Mac at ~$1.50/hr = £1,000/mo) is obscene. If even 30% of your fleet is iOS CI, you're making real money.

**Easiest to acquire:** AI tinkerers. They're loud, post about their setups, and convert from Twitter/Reddit at high rates.

---

## 2. Competitive landscape (April 2026)

| Provider | Positioning | Price (dedicated M2/M4, ~monthly) | Your opening |
|---|---|---|---|
| **MacStadium** | Enterprise-grade, US-focused | $149–400+ | Expensive; overkill for indies; not UK-native |
| **MacinCloud** | Consumer/small dev | $30–120 | Often shared, not dedicated; dated UX |
| **AWS EC2 Mac** | AWS ecosystem | ~$1.08/hr = ~£550/mo minimum (24-hr min billing) | Wildly expensive, inflexible |
| **Mac Mini Vault** | Smaller US provider | $59–120 | Solid but US-only |
| **Scaleway / Hetzner Mac offerings** | EU-native but limited stock | €90–180 | Often out of stock; thin support |
| **You** | **UK-based, managed, indie-friendly, AI-forward** | £49–149 | — |

**Your angles:**
1. **UK data residency** — real for some buyers (compliance, latency)
2. **Indie-friendly** — simpler pricing, no enterprise BS
3. **AI workloads first** — lean into the Apple silicon LLM story
4. **Managed, not just rented** — fewer "my Mac is wedged" tickets because you own the base image

---

## 3. Unit economics (back-of-envelope)

Let's check this is actually worth doing. Numbers are illustrative — tune to your reality.

### Per-machine costs (M4 mini, 16 GB / 512 GB)

| Item | Monthly |
|---|---|
| Hardware amortisation (£799 retail / 36 months) | £22 |
| Electricity (~10 W avg × £0.28/kWh × 730 hrs) | £2 |
| Internet + bandwidth allocation | £8 |
| Colo rack space (if using a facility) | £10 |
| Insurance / replacement reserve | £5 |
| Tailscale / tooling / monitoring | £1 |
| **Total cost** | **~£48/mo** |

### At £89/mo "Pro" tier

- Gross margin per machine: **~£41/mo (~46%)**
- Break-even per machine on hardware: **~20 months**
- At 20 machines: ~£820/mo gross profit
- At 100 machines: ~£4,100/mo gross profit
- **This scales well only if you automate onboarding.** Every manual provisioning kills the margin.

### Where the margin actually comes from

1. **iOS CI customers paying Pro+ tier** for multiple machines (50%+ margin)
2. **Annual prepays** — cash flow up front, churn locked in
3. **Add-ons** — setup fees, managed deployments, extra storage (pure margin)
4. **Hardware refresh cycles** — your M2s age into the Starter tier after 2 years at 60% of new cost

---

## 4. Pricing principles

- **Three tiers, not one.** One price anchors too low. Three tiers let buyers self-sort and the middle tier sells most (classic decoy pricing).
- **Round numbers ending in 9.** £49, £89, £149. Not £47.50. This is a B2B/prosumer product; decimals look cheap.
- **Annual discount = 2 months free**, not 10% off. "2 months free" converts better and locks in a year.
- **Charge for setup on complex configs.** £100 setup for a custom CI pipeline is free money and signals quality.
- **Don't undercut MacStadium on their turf.** You can't win enterprise on price; win on simplicity.

---

## 5. Growth plan (first 12 months)

### Months 1–3: Prove the model
- Buy 3–5 Mac minis. Host them yourself (home office is fine at this scale if your internet is solid).
- Run the waitlist. Hand-pick first 5 customers.
- Learn where the pain is: provisioning, support, abuse, bandwidth. Write runbooks.
- Goal: 5 paying customers, zero incidents, case study for each.

### Months 4–6: First scale step
- Move to colo (Custodian, Equinix, or a UK regional DC — Milton Keynes / Manchester / London).
- Automate provisioning with Apple Configurator + MDM (Mosyle is ~£1/device/mo and worth it).
- Public launch. Paid ads on X, IndieHackers sponsor, maybe Hacker News "Show HN".
- Goal: 20–30 machines, ~£1.5k MRR.

### Months 7–12: Vertical lock-in
- Pick ONE vertical to dominate. Probably **iOS CI for UK agencies** — highest spend, clearest pitch.
- Build a "GitHub Actions self-hosted Mac runner" product on top — one-click, £129/mo, done.
- Partner with 2–3 UK iOS dev consultancies as resellers.
- Goal: £10k MRR, 60%+ of revenue from iOS CI tier.

---

## 6. Risks and moats

**Risks**
- Apple could clamp down on macOS virtualisation or commercial rental (they've done this before)
- An ISP outage or colo incident can take out your whole fleet — multi-site from month 6 onward
- Abuse: a single bad actor mining crypto or sending spam can get your IPs blocklisted
- Hardware failure: keep 10% spare capacity on-hand

**Defensible moats (build these deliberately)**
- **UK data residency** as a compliance story
- **Pre-configured iOS CI runners** with known-good images (saves customers hours)
- **Brand** — "the indie Mac host" is a real market position nobody owns in the UK
- **Community** — a Slack or forum of tenants who help each other reduces your support load

---

## 7. What not to do (yet)

- Don't build custom management dashboards in year one. SSH + a Notion page is fine.
- Don't offer Windows or Linux. Stay focused.
- Don't accept crypto. Chargeback / fraud risk too high for a hardware business.
- Don't race to the bottom on price. You'll only attract the worst customers.
- Don't skip the TOS and abuse policy. Write it on day one. See [lockdown.md](lockdown.md).
