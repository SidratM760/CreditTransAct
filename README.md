# CreditTransAct: A Profile-Driven Credit Card Transaction Dataset for Scalable Fraud Detection 
A large-scale synthetic dataset of 15 million credit card transactions is generated for fraud detection research. It is divided into four segments: A_Established, B_Regular, C_New, and D_Guest, with 95.10% legitimate and 4.90% fraudulent transactions. Segments A-C use persistent customer profiles, while Segment D represents anonymous transactions without history. Each record contains 33 features covering transaction, geographic, device, network, authentication, and behavioral attributes. Fraud is modeled using four clusters: Card-Not-Present, Bot/Card Testing, Account Takeover, and Geo-Velocity, with controlled label noise added to borderline cases. The dataset is generated using a reproducible and memory-efficient pipeline built with Python, NumPy, Pandas, and PyArrow. 

## Dataset Design Overview 

### Customer Segments
The dataset is divided into four segments with different fraud rates and customer distributions:
| Segment         | Share | Fraud Rate | Customer Pool       |
| --------------- | ----- | ---------- | ------------------- |
| A_Established | 40%   | 2.5%       | 120,000 customers   |
| B_Regular     | 30%   | 4.0%       | 150,000 customers   |
| C_New         | 20%   | 7.0%       | 200,000 customers   |
| D_Guest       | 10%   | 12.0%      | No persistent users |

Segments A, B, and C use repeat customers with stable profiles, while Segment D represents one-time guest transactions without history.

### Customer Profile
Each customer in segments A–C has a fixed profile including spending habit, merchant preference, account age, chargeback history, credential update history, location, and device usage. These profiles are used to generate behaviorally consistent transactions, where fraud is modeled as deviation from normal behavior.

### Fraud Clusters
Fraud is modeled using four correlated patterns:
1. **CNP (Card-Not-Present) fraud**: CVV mismatch, 3DS failure, proxy/Tor usage
2. **Bot activity**: fast sessions, emulator devices, datacenter IPs, burst transactions
3. **Account Takeover (ATO)**: recent credential change, unusual merchant, late-night activity
4. **Geo anomalies**: impossible travel speed and high-risk locations
   
A fraud score is computed from these signals, and a small amount of label noise is added to borderline cases.

## Steps to Reproduce
1. Install the required dependencies.  
2. Set `OUTPUT_DIR` in the script to a preferred local path.  
3. Run the script.
   
The script:
- Creates customer profiles for segments A, B, and C, while segment D uses guest transactions.
- Generates transactions in 250,000-row chunks and saves them as Parquet part files to reduce memory usage.
- Assigns fraud labels, generates behavior-based features, computes fraud scores, and adds controlled label noise.
- Merges all part files into a final `credit_card_fraud.parquet` file and removes temporary files.
  
**Output:** 15 million synthetic credit card transaction records with fraud patterns.

### Key Features
- 30+ behavioral, transactional, and device-based features
- Consistent customer-level behavior modeling
- Correlated fraud patterns for realistic learning
- Label noise for real-world ambiguity
- Fully synthetic and reproducible (SEED = 42)


*A 10,000-row sample from the full dataset is provided, containing 1,250 fraud and 1,250 legitimate transactions from each segment (A-D). This sample is intended only for understanding the dataset design and does not reflect the original class balance or segment distribution.*


