# Lessons Learned — Azure Foundation Build (Cell A)

**Project:** Cloud Storm Cells — Cell A  
**Region:** West US 2  
**Date:** February 2026  

---

## 1) Subscription & Billing Insights

- Azure Free Trial can transition to Pay-As-You-Go if not monitored.
- Budget alerts are not “automatic” — they must be configured intentionally.
- Billing scope affects where budgets can be created and applied.

**Key takeaway:** Configure budgets immediately after subscription creation to prevent accidental overspend.

---

## 2) Cost Management Lessons

- A low monthly cap (example: $50) keeps experimentation safe.
- Multiple thresholds (20%, 50%, 80%, 100%) provide progressive awareness.
- Email alerts are fine for a lab, but enterprise environments should route alerts into action groups / ticketing / chatops.

**Future improvement:**
- Add Azure Monitor action groups
- Integrate with Teams/Slack
- Implement a tagging strategy to track cost by workload/cell

---

## 3) Resource Organization

- Resource Groups act as lifecycle containers, not networking objects.
- Naming conventions matter early; they reduce confusion when deployments scale.
- Keeping everything in one region simplifies cost tracking and reduces operational friction.

Suggested convention:

---

## 4) Network Architecture Decisions

- Selecting **10.0.0.0/16** enables growth without redesign.
- Using **/24** subnets provides clean segmentation and predictable capacity.
- Separating management from workstation/workload subnets prepares the environment for future least-privilege routing and controlled admin ingress.

**Key networking lesson:** Plan IP addressing for future growth before deploying compute. Changing CIDR blocks later is disruptive.

---

## 5) Security Observations

- NSGs were created and attached at the **subnet level**, providing a centralized enforcement point for any VM placed into those subnets.
- Avoiding broad inbound rules early keeps the environment safe by default.
- “Secure by design” matters even in lab environments.

**Future improvement:**
- Add explicit least-privilege inbound rules only when required
- Consider Bastion for controlled access
- Explore private endpoints as capabilities expand

---

## 6) Identity & Access

- Security Defaults and MFA enforcement help establish a baseline quickly.
- Identity should be treated as a first-class security control, even for personal labs.

---

## 7) VM Deployment Lessons (Phase 2)

- Free trial subscriptions may restrict certain VM sizes in specific regions.
- If a preferred size is unavailable (example: Standard_B1s in West US 2), choose the closest low-cost alternative available (example used: **Standard_B2ats_v2**).
- Serial Console is extremely useful for validation when you don’t want to rely on public ingress.

---

## 8) Public IP / Access Lessons

- During VM creation, Azure may assign a Public IP depending on selected options.
- Even if a Public IP exists as a resource, inbound access remains constrained unless NSG rules explicitly allow it.
- “No inbound rules opened” is a valid security posture for early foundation builds.

---

## 9) Operational Maturity Gaps (Intentional for Cell A)

Not implemented in Cell A (by design):
- Centralized logging / diagnostic settings
- Log Analytics workspace
- Alerting/action groups for security events
- Backup strategy

These belong in later cells once the foundation is stable.

---

## 10) Strategic Takeaways

1. Cost control must come first.
2. IP planning prevents future redesign.
3. Subnet segmentation improves clarity and security.
4. Serial Console provides resilient validation access.
5. Deallocating compute after validation is real cloud-discipline behavior.

