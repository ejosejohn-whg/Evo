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

|Clients  |  Status	| Business	| UK-LOW | MGA-LOW	| UK-HIGH	| MGA-HIGH | <br>
|:--      |:--:     |:--:       |:--:    |:--:      |:--:     |--:       | <br>
Battlecat  High	  Unit I    NA     NA	      1000    10001        <br>
Casimba	   High	  Unit II   NA     NA       1100    11001        <br>
Branders   Low	  Unit III	79     791      NA      NA           <br>
Christ     High	  Unit IV   NA     NA       1200    12001        <br>
Forvana    Low	  Unit V    147    1471	    NA      NA           <br>
Silver     High	  Unit VI	  NA     NA       1300    13001        <br>
Vita       Low	  Unit VII	86     861	    NA      NA           <br>
Superfly   High	  Unit VIII	NA     NA       1400    14001        <br>
Yellow Day Low	  Unit IX   106    1061	    NA      NA           <br>
MrREX	     High	  Unit X    NA     NA       1500    15001        <br>
**Note: The Skin numbers are for ilustration pourpose.**

---

## ⚙️ Operations: Execution & Rollout Options

We have three proposed ways to execute this implementation. Please review and provide feedback on the preferred approach.

### Option 1: Incremental Rollout
*A step-by-step approach moving sites to production incrementally.* <br>
**A:** Ensure we have **UK - Low** and **MGA - Low** ready and available from the Evo side. <br>
  1: Get Evo environment for Stage: **Evo Provided.** <br>
  2: Configured the EVO3 and EVO04 Setup on Stage: **Juan Configured.** <br>
  3: Evo to test the Setup and Approve the Integragaion. **Evoluton Passed the tests.** <br>
  4: Evo to configure the Production Setup. **Awaiting on Evo**<br><br>
  
**B:** Test the games on **"Main Stage Bingo"** on UK Low and MGA Low <br>
  1: We need to define the Stage testing Stratagy. **To be defined by product and Operations** <br>
  2: Configure the Game on Stage. **To be defined by product and Operations** <br>
  3: Test the Games on Stage. **To be defined by product and Operations**<br><br>
  
**C:** Move sites one by one to production, making necessary changes on games and supplier configuration. <br>
  1: Prepare the script for all the games for "main stage Bingo" as Live games we will not have all games on stage. <br>
  2: Execute the script on Stage and Live. **Sprit stratagy to be defined by Game Service and Operations** <br>
  3: Test the games on Live. **Testing Stratagy to be defined by QA and Operations** <br>
  4: Plan and execute monitoring Stratagy on Live with DD. **Operations need to define, John can help setting it up** <br>
  5: Once "Main Stage Bingo" is live and stable with the Low Setup for UK and MGA. Plan for all the other sites with Low setups one by one or as a group <br>
  6: Execute the script on Stage and Live. **Sprit stratagy to be defined by Game Service and Operations** <br>
  7: Test the games on Live. **Testing Stratagy to be defined by QA and Operations** <br>
  8: Plan and execute monitoring Stratagy on Live with DD. **Operations need to define, John can help setting it up** <br>
  **General Notes:** <br>
    1: With this we will have all the Low sites of Completed. <br>
    2: We will have game levels / Stage starting from new. <br>
    3: We need to map the Jackpot respectively. (As the are using form Env01 whcih is existing there should not be any issue, if not changed) <br>
    4: If we have unfinised games they have to be made cancelled or settled. <br>
    5: All started and not completed free spins will not work as this will have gone to old Env and skin which will notbe in use after we move over. <br>
    6: We can move to have Game config to Low table for all the sites and remove the script site specific script. Or we can wait for High setups to finish and have singe game entry for all the UK and MGA sites. <br><br>
    
**D:** Ensure we have **MGA - High** ready and available from the Evo side.<br>
  1: Get Evo environment for Stage: **Evo Provided.** <br>
  2: Configured the EVO2 Setup on Stage: **Juan Configured.** <br>
  3: Evo to test the Setup and Approve the Integragaion. **Evoluton Passed the tests.** <br>
  4: Evo to configure the Production Setup. **Awaiting on Evo** <br><br>
  
**E:** Test and verify that Env 1 (UK - High & MGA High) Main Stage Bingo. <br>
  1: We need to define the Stage testing Stratagy. **To be defined by product and Operations**<br>
  2: Configure the Game on Stage. **To be defined by product and Operations** <br>
  3: Test the Games on Stage. **To be defined by product and Operations** <br><br>
  
**F** Continue moving sites one by one to production, applying changes to games and supplier configurations. <br>
  1: Now that we have all sites live on the Low Site reconfigure "Main Stage Bingo" to High Stakes of UK and MGA <br>
  2: Execute the script on Stage and Live. **Sprit stratagy to be defined by Game Service and Operations** <br>
  3: Test the games on Live. **Testing Stratagy to be defined by QA and Operations** <br>
  4: Plan and execute monitoring Stratagy on Live with DD. **Operations need to define, John can help setting it up** <br>
  5: Once "Main Stage Bingo" is live and stable with the Low Setup for UK and MGA. Plan for all the other sites with Low setups one by one or as a group <br>
  6: Execute the script on Stage and Live. **Sprit stratagy to be defined by Game Service and Operations** <br>
  7: Test the games on Live. **Testing Stratagy to be defined by QA and Operations** <br>
  8: Plan and execute monitoring Stratagy on Live with DD. **Operations need to define, John can help setting it up** <br>
   **General Notes:** <br>
    1: With this we will have all the Low sites of Completed. <br>
    2: We will have game levels / Stage starting from new. <br>
    3: We need to map the Jackpot respectively. (As the are using form Env01 whcih is existing there should not be any issue, if not changed) <br>
    4: If we have unfinised games they have to be made cancelled or settled. <br>
    5: All started and not completed free spins will not work as this will have gone to old Env and skin which will notbe in use after we move over. <br>
    6: We now have All the sites on were we need, we can clean up the game config on UK and MGA have one game code per game for entire 4 combinations we have.<br>

---

### Option 2: Big Bang Rollout
*Prepare all environments and execute the configuration move on a single agreed-upon date.*

**A** Ensure we have **UK - Low** and **MGA - Low** ready and available from the Evo side. <br>
**B**Test the games on **"Main Stage Bingo Stage and Production, we will need scripts for Stage and Live. <br>
**C** Ensure we have **UK - High** and **MGA - High** ready and available from the Evo side. <br>
**D** Test the games again on **"Main Stage Bingo Stage and Production, we will need scripts for Stage and Live. <br>
**E** Agree on a rollout date and move all configurations in a single day rather than doing it one by one. <br>
 **General Notes:** <br>
    1: Testing Stratagy, Spript Stratagy and Operational stratagy needs to be added as int the increment model, just that we do big bang in this one with the confidence we get this "Main Stage Bingo Stage and production"
    2: With this we will have all the Low sites of Completed. <br>
    2: We will have game levels / Stage starting from new. <br>
    3: We need to map the Jackpot respectively. (As the are using form Env01 whcih is existing there should not be any issue, if not changed) <br>
    4: If we have unfinised games they have to be made cancelled or settled. <br>
    5: All started and not completed free spins will not work as this will have gone to old Env and skin which will notbe in use after we move over. <br>
    6: We now have All the sites on were we need, we can clean up the game config on UK and MGA have one game code per game for entire 4 combinations we have.<br>

  ---

## 📝 Important Notes & Action Items

> **DB Scripts:** The Game config map changes will be written via database scripts to manage the different mappings and values. We will need to run these DB scripts to properly update the game config map.

> **Testing Requirement:** We must test the games in **"Main Stage Bingo"** to verify that everything works correctly with the new game config map across all 4 setups.

> **Feedback Requested:** Please feel free to add any other ideas or suggestions that can improve the Operations rollout process.

> **New Game Release** Need to plan the New Game release when there is changes on stage with syncing with Live.
