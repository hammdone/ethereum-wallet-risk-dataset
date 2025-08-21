# Ethereum Wallet Risk Dataset


## Overview
This dataset contains Ethereum wallet-level features designed for analyzing wallet behavior and risk. Each wallet is labeled as **Dodgy** or **Legit** based on internal risk signals such as transaction patterns, clustering with risky wallets, contract interactions, and token holdings. This dataset can be used for blockchain analytics, wallet risk scoring, fraud detection, and machine learning experiments.

---

## Disclaimer

This dataset is compiled from publicly available Ethereum blockchain data and is intended solely for research, educational, or analytical purposes. No private keys, personal information, or non-public data have been included.

## Dataset Features

| Column | Description |
|--------|-------------|
| `address` | Ethereum wallet address (hashed or anonymized for privacy). |
| `in_cluster_with_risky` | Binary flag indicating if the wallet is in a cluster with previously identified risky wallets. |
| `max_daily_tx_count` | Maximum number of transactions made by the wallet in a single day. |
| `small_transfer_count` | Count of small-value transfers. |
| `idle_days` | Number of days the wallet was inactive. |
| `contract_interactions_count` | Number of smart contract interactions. |
| `time_since_last_tx` | Time in seconds since the wallet's last transaction. |
| `token_diversity` | Number of distinct token types held. |
| `token_holdings_count` | Count of individual token holdings. |
| `balance_eth` | Wallet balance in ETH. |
| `tx_count` | Total number of transactions. |
| `tx_in_count` | Number of incoming transactions. |
| `tx_out_count` | Number of outgoing transactions. |
| `avg_tx_value` | Average value per transaction (ETH). |
| `max_tx_value` | Maximum value of a single transaction (ETH). |
| `activity_days` | Total active days with transactions. |
| `label` | Risk label: `Dodgy` or `Legit`. |
| `is_mixer` | Binary flag indicating interaction with known mixers (e.g., Tornado Cash). |

---

## Data Collection

The dataset was derived from Ethereum blockchain data using programmatic extraction via the **Etherscan API**. The following workflow was used:

1. **Wallet Selection**  
   Wallet addresses were selected from publicly available sources of risky and normal wallets. The dataset contains both wallets flagged as dodgy and legitimate.

2. **Transaction Extraction**  
   For each wallet, up to 100 recent transactions were retrieved using the Etherscan API. Key metrics such as `tx_count`, `tx_in_count`, `tx_out_count`, `avg_tx_value`, and `max_tx_value` were computed.

3. **Behavioral Feature Computation**  
   Features such as `max_daily_tx_count`, `small_transfer_count`, `idle_days`, `activity_days`, and `contract_interactions_count` were calculated based on transaction history.  

4. **Token Features**  
   Token holdings and diversity were calculated using token balance data. Wallets interacting with multiple tokens and contracts were annotated with `token_diversity` and `token_holdings_count`.

5. **Cluster and Mixer Detection**  
   Wallets were checked for clustering with known risky wallets (`in_cluster_with_risky`) and interaction with mixers (`is_mixer`), using internal graph analysis and publicly known mixer addresses.

6. **Labeling**  
   Wallets were labeled **Dodgy** if they exhibited suspicious behavior patterns, including high activity in small transfers, clustering with risky wallets, or mixer usage. Legitimate wallets were labeled otherwise.

---

## Example Usage

```python
import pandas as pd

# Load the dataset
df = pd.read_csv("ethereum_wallet_risk_dataset.csv")

# Inspect the first few rows
print(df.head())

# Filter wallets interacting with mixers
mixers = df[df['is_mixer'] == True]
print(mixers)
