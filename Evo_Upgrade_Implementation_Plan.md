# Evo Upgrade Implementation & Rollout Plan

This document outlines the proposed strategy, goals, and rollout phases for the Evolution (Evo) Upgrade. It is intended for review and alignment across **Product Managers, Developers, Testers, and Operations**.

---

## 🎯 Goal & Strategic Objectives
The primary objective of this upgrade is to make Evolution integrations faster and simpler when launching a new site. Secondly, we aim to unify our configuration by using a **single gamecode** for the same game across different sites.

---

## 🏗️ Current Architecture & Setup
- **Environments & Skins:** Evolution utilizes "Environments," which contain multiple "Skins."
- **Current Structure:** For the WHG EU setup, we currently use a single Environment. We rely on creating different skins to support our various websites.
- **Jurisdiction Management:** Our EU setup supports two jurisdictions (Malta and UK). Because Evolution does not support jurisdictions natively, we currently manage this by creating multiple skins in Evolution.

---

## ⚠️ Problem Statement
The current setup introduces several bottlenecks:
1. **Slow Release Process:** Creating skins is a complex process for Evolution, which inherently delays our website release timelines.
2. **Duplicated Effort:** Supporting two jurisdictions effectively doubles the workload on Evolution's side.
3. **Complex Mapping:** Because of jurisdictional differences, the *same* game currently has *different* values and game codes from the supplier. This forces us to create complicated mapping logic on our end.
4. **Client Fragmentation:** Different clients have different needs for the same game, forcing us to maintain multiple game codes to manage them.

---

## 💡 Proposed Solution
To resolve the bottlenecks, we are restructuring how we interact with Evolution's infrastructure:
1. **Environment Reorganization:** Create Environments in Evolution based strictly on **Jurisdiction** and the **Risk Ability** of the clients.
2. **Skin Restructuring:** Create skins inside the Environment **per client** rather than per site.

### ✅ Key Benefits
- **Evolution Efficiency:** Greatly reduces the workload on Evolution's side for creating environments and skins.
- **Internal Efficiency:** Significantly reduces our internal workload for maintaining complex game code mappings.
- **Speed to Market:** Leads to a much faster and smoother website release process.

---

## 🛠️ Implementation Strategy

**Current State:** 
- We have 1 Environment.
- The number of skins equals the number of sites (this is doubled if a site operates in both MGA and UK).

**Target State:**
We will shift to a 4-Environment setup based on Jurisdiction and Risk:
1. **UK - High** *(We will repurpose our current existing Environment for this)*
2. **MGA - High** *(New Environment)*
3. **UK - Low** *(New Environment)*
4. **MGA - Low** *(New Environment)*

---

## ⚙️ Operations: Execution & Rollout Options

We have three proposed ways to execute this implementation. Please review and provide feedback on the preferred approach.

### Option 1: Incremental Rollout
*A step-by-step approach moving sites to production incrementally.*

1. **A:** Ensure we have **UK - Low** and **MGA - Low** ready and available from the Evo side.
2. **B:** Test the games on **"Main Stage Bingo"**.
3. **C:** Move sites one by one to production, making necessary changes on games and supplier configuration.
4. **D:** Ensure we have **MGA - High** ready and available from the Evo side.
5. **E:** Test and verify that Env 1 (UK - High) is working properly with the specific values for a given site.
6. **F:** Continue moving sites one by one to production, applying changes to games and supplier configurations.

### Option 2: Big Bang Rollout
*Prepare all environments and execute the configuration move on a single agreed-upon date.*

1. **A:** Ensure we have **UK - Low** and **MGA - Low** ready and available from the Evo side.
2. **B:** Test the games on **"Main Stage Bingo"**.
3. **C:** Ensure we have **UK - High** and **MGA - High** ready and available from the Evo side.
4. **D:** Test the games again on **"Main Stage Bingo"**.
5. **E:** Agree on a rollout date and move all configurations in a single day rather than doing it one by one.

### Option 3: Risk-Mitigated Big Bang (Requires Evo Approval)
*Similar to Option 2, but creates new skins to reduce risk on the currently used Env 1 (UK - High).*

1. **A:** Ensure we have **UK - Low** and **MGA - Low** ready and available from the Evo side.
2. **B:** Test the games on **"Main Stage Bingo"**.
3. **C:** Ensure we have **MGA - High** ready and available from the Evo side.
4. **D:** Test the games on **"Main Stage Bingo"**.
5. **E:** Get **new Skins** for Env 1 for **UK - High** and configure the games according to those values.
6. **F:** Test the games thoroughly in **"Main Stage Bingo"**.
7. **G:** Agree on a rollout date and move all configurations in a single day.

---

## 📝 Important Notes & Action Items

> [!NOTE]  
> **Strategic Difference:** The main difference between Option 2 and Option 3 is risk mitigation. In Option 3, since Env 1's current skins are actively in use, we propose creating *new* skins on Env 1 that won't be used immediately. This means less impact if a revert is necessary. **Note:** This approach is currently *not approved* by Evo and is just a proposal.

> [!IMPORTANT]  
> **DB Scripts:** The Game config map changes will be written via database scripts to manage the different mappings and values. We will need to run these DB scripts to properly update the game config map.

> [!IMPORTANT]  
> **Testing Requirement:** We must test the games in **"Main Stage Bingo"** to verify that everything works correctly with the new game config map across all 4 setups.

> [!TIP]  
> **Feedback Requested:** Please feel free to add any other ideas or suggestions that can improve the Operations rollout process.