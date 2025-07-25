# 💼 Wallet Risk Scoring - Round 2 Assignment

## 📄 Problem Statement

In this task, we were provided with a list of 100 wallet addresses. Our objective was to evaluate and score each wallet's **risk level** based on its on-chain interaction with the **Compound V3 protocol**.

The risk score was to be calculated on a scale of **0 to 1000**, where higher scores indicate **lower risk** and active usage, and lower scores imply **higher risk** or inactivity.

---

## 📊 Approach Overview

Unlike traditional script-based methods, this analysis was conducted **without any Python or backend automation** for fetching data. Instead, we used:

- 📈 **The Graph Protocol** (GraphQL)
- 🔍 **Compound V3 Subgraph**
- 📑 Manual/Spreadsheet-based review and scoring

---

## 🔧 Data Collection Method

We used **The Graph’s hosted subgraph for Compound V3** to directly query on-chain wallet activity.

### GraphQL Query Example:
```graphql
{
  account(id: "0xfaa0768bde629806739c3a4620656c5d26f44ef2") {
    id
    positions {
      id
      asset {
        symbol
      }
      totalDeposited
      totalBorrowed
    }
  }
}
```

This query provides:
- Total amount deposited by the wallet
- Total borrowed amount
- Active lending/borrowing positions

We repeated this for each wallet using the Graph interface.

---

## 🧠 Feature Selection Rationale

The following features were identified as indicators of risk:

| Feature              | Reason                                                                 |
|----------------------|------------------------------------------------------------------------|
| `totalDeposited`     | Higher deposits indicate active, stable participation in lending       |
| `totalBorrowed`      | Active borrowing shows utilization of protocol, suggesting engagement  |
| `positions.count`    | The number of active positions helps identify active vs dormant users  |

---

## 🧮 Risk Scoring Logic

We applied a **simple scoring formula** manually (or via spreadsheet):

```
Base Score: 300

+ 20 points per compound-related transaction (max 400)
+ 10 points per ETH borrowed (max 300)
= Final Score (clipped to 0-1000)
```

### Example:

For a wallet with:
- 5 compound-related txns → 5 * 20 = 100
- 10 ETH borrowed → 10 * 10 = 100

→ Final Score = 300 (base) + 100 + 100 = **500**

All values were rounded and capped at defined limits for fairness.

---

## 📤 Final Output

The result was exported to a CSV in the format:

| wallet_id                                | score |
|------------------------------------------|-------|
| 0xfaa0768bde629806739c3a4620656c5d26f44ef2 | 732   |
| 0x06b51c6882b27cb05e712185531c1f74996dd988 | 600   |

This file is included as `wallet_scores.csv`.

---

## ✅ Justification of Risk Indicators

- **Active wallets** with lending or borrowing activity are typically **low risk**, as they are using the protocol correctly.
- **Dormant wallets**, or wallets with no meaningful Compound interaction, are considered **high risk** (possibly dust, spam, or abandoned).
- Using on-chain features directly ensures **trustless** and **transparent** scoring without relying on external metrics.

---

## 📁 Deliverables

- ✅ `wallet_scores.csv`: List of wallet addresses and their risk scores
- ✅ `README.md`: Full explanation of method and logic used
- ✅ `query_samples.graphql` *(optional)*: Example queries used in The Graph interface

---

## 🔗 Resources

- [Wallet List](https://docs.google.com/spreadsheets/d/1ZzaeMgNYnxvriYYpe8PE7uMEblTI0GV5GIVUnsP-sBs/edit?usp=sharing)
- [Compound V3 Subgraph](https://thegraph.com/explorer)
- [Submission Form](https://forms.gle/epKXzFGg9rxCea728)

---

## 📬 Summary

- ✅ 100% data collected using **The Graph’s Compound V3 Subgraph**
- ✍️ Scores calculated with a **clear, formula-based model**
- 📂 Results submitted as required in the specified CSV format

---
