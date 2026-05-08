# Cell A — Azure Fundamentals

**A reproducible Azure foundation: subscription + budget governance, VNet with segmented subnets, NSG controls, and a validated VM deployment with lifecycle discipline. The prerequisite for the rest of the Cloud Cells.**

This is **Cell A** of the Cloud Cells series — see [Related projects](#related-projects).

---

## What this is

A from-zero Azure environment built and documented like a real engineering exercise: every architectural decision is recorded, every step is screenshot-proven, and cost governance is treated as a first-class requirement. The environment is the **foundation layer** that subsequent cells (security hardening, monitoring, detection, automation) build on top of.

If a recruiter wants to see whether I can stand up a clean Azure environment, document the decisions, and avoid surprise charges, this is the answer.

---

## What was built

### Phase 1 — Foundation (complete)
- Azure subscription activated with budget guardrails enabled day 1
- Region selected: **West US 2**
- Resource Group created as a clean lifecycle boundary
- Virtual Network with **enterprise-style subnet segmentation**
- Network Security Groups created and attached at the subnet level

### Phase 2 — Compute (complete)
- Ubuntu VM deployed inside the segmented network
- Least-privilege NSG rules added (key-only SSH, allowlisted IPs)
- Connectivity validated end-to-end
- VM stopped + deallocated after testing (cost discipline)

### Phase 3 — Handed off to later cells
- Logging, SIEM forwarding, detections — see [Cell B](https://github.com/RealPhantomLee/azure-security-monitoring-lab)
- Policies, governance automation — future cells

---

## Public exposure status

**None.** No public IPs, no inbound 0.0.0.0/0 rules, no static endpoints. All access is gated by NSG allowlists and key-only SSH.

---

## What's in this repo

| Path | Purpose |
|------|---------|
| [`build-notes.md`](build-notes.md) | Full step-by-step build narrative with rationale for every decision |
| [`network-design.md`](network-design.md) | VNet / subnet / NSG architecture |
| [`lessons-learned.md`](lessons-learned.md) | Mistakes, surprises, and what I'd do differently |
| `architecture/` | drawio diagram of the deployed topology |
| `screenshots/` | 40+ Azure portal screenshots — proof of every step (RG, VNet, subnets, NSGs, VM creation, MFA, budget) |
| `configs/` | Resource configurations |
| `logs/` | Deployment / validation output |
| `notes/` | Working notes from the build session |

---

## Why "Cloud Cells"?

The series is structured like cells in a cell tower — each cell is a self-contained domain that snaps onto the others. Cell A is the foundation cell; Cell B layers cloud security on top; subsequent cells extend into SOC-style detection and automation governance. Each cell can be read on its own, but they compose into a coherent cloud-engineering portfolio.

---

## Cost governance

A budget alert was the **first** thing configured after subscription activation, before any compute resource existed. The build treats cost awareness as a discipline, not an afterthought — this is the same posture I'd take in a paid environment.

---

## Related projects

- **azure-fundamentals-cell** (this repo) — Cell A: foundation
- [azure-security-monitoring-lab](https://github.com/RealPhantomLee/azure-security-monitoring-lab) — Cell B: hardening, Log Analytics, KQL detections, alerts

---

## Author

**RealPhantomLee Tucker** — [github.com/RealPhantomLee](https://github.com/RealPhantomLee)
