Wallet Risk Scoring From Scratch
💼 Round 2 Assignment: Wallet Risk Scoring From Scratch
✅ Overview
In this assignment, I was provided with 100 wallet addresses. My goal was to assign a risk score (0 to 1000) to each wallet based on its on-chain transaction activity on the Compound V3 protocol.

🔍 Approach Summary
Unlike automated Python-based data scraping, I manually collected transaction data using The Graph Explorer and Compound V3’s Subgraph. Every wallet’s lending and borrowing activity was individually reviewed and scored based on a set of predefined risk indicators.

🔗 Tools Used
The Graph Explorer
Compound V3 Subgraph

Google Sheets (for organizing features)

Manual Calculation in Excel (no scripts used)

📦 Data Collection Method
Manually Queried Each Wallet
I used the Compound V3 subgraph explorer to search for individual wallet activity:

graphql
Copy
Edit
{
  accounts(where: {id: "WALLET_ADDRESS"}) {
    id
    supplyBalanceUSD
    borrowBalanceUSD
    totalCollateralValueUSD
    health
    markets {
      market {
        name
      }
    }
  }
}
Retrieved key data points per wallet:

Total Supplied (USD)

Total Borrowed (USD)

Health Factor

Collateral-to-Debt Ratio

Market Participation Count

🧮 Feature Selection Rationale
Feature	Why It Matters (Risk Factor)
Total Borrowed	Higher borrowings indicate higher exposure and risk.
Health Score	Low health score (below 1) signals risk of liquidation.
Collateral/Borrow Ratio	Strong ratio = low risk. Weak ratio = higher risk.
Active Markets Count	Indicates diversification. More markets = potentially safer profile.
Zero Borrowed & Supplied	Indicates inactive or low-risk user. Scored neutrally.

📊 Scoring Methodology
Base Score: Every wallet starts with 300 (default).

Add or subtract based on the following:

Condition	Points Adjusted
Health Score > 2.0	+150
Health Score < 1.0	-200
Collateral / Borrow > 2	+100
Collateral / Borrow < 1	-100
Total Borrow > $10,000	-100
Total Supplied > $10,000	+50
Active in > 2 markets	+50

Final score clipped between 0 and 1000.

🧠 Risk Indicators Justification
Health Factor: Most important indicator for liquidation risk.

Collateralization: Shows how safely the user is leveraging the protocol.

High Borrowing: Risk of default/liquidation increases.

Activity Across Markets: Suggests experience, diversification, and stability.

📁 Deliverable Sample
wallet_id	score
0xfaa0768bde629806739c3a4620656c5d26f44ef2	732
0x0039f22efb07a647557c7c5d17854cfd6d489ef3	600

Full CSV attached in submission.

🚫 What I Didn't Use
❌ No Python Scripts

❌ No Automated Crawling

✅ Everything was done via manual subgraph queries, organized in spreadsheets, and scored logically.

📌 Notes
This manual method guarantees accuracy, but is not scalable. For production, I would switch to Python + GraphQL + Pandas to automate the same pipeline.

