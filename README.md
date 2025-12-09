# 🗳️  Blockchain-Based E-Voting System

## 🔥 Introduction

It is time to secure and transparently modernize the backbone of democracy: **Voting**, using the power of cutting-edge technology! `SecureChain Vote` is a state-of-the-art e-voting system that leverages the immutable and decentralized nature of a **Private Blockchain**.

Our goal is to address the flaws of existing voting systems, such as Security concerns, Centralization, and Limited Audibility. Through this system, we ensure high security, complete transparency, and efficiency, so that every citizen's vote is safe and the integrity of election results is maintained.

## ✨ Key Features

* **🛡️ Enhanced Security:** Private blockchains have more control over the network and who has access to it, making them more secure than public blockchains.
* **🔒 Voter Privacy:** Private blockchains can be designed to ensure that the identity of voters is kept confidential, which is not possible in public blockchains where transactions are visible to everyone.
* **🚀 Superior Scalability and Speed:** Private blockchains can handle a large number of transactions, making them suitable for large-scale e-voting systems. Transactions can be processed much faster, which is important for real-time election results.
* **✍️ Smart Contracts:** Programmed agreements that go into effect automatically when certain criteria are met. These contracts automate transactions and eliminate the need for a middleman. The smart contract will contain features to make voting safe, transparent, and client-side compatible.
* **✅ Real-Time Auditability:** With the transaction ID, a user can verify their vote immediately after the commit is complete.

## 💡 How It Works (Methodology)

Our system is implemented using **Smart Contracts** on a private blockchain, as this is the only way to interact with ledger data.

### 3 Main Phases

1.  **Upcoming Phase:**
    * The election is created by a trusted admin.
    * Users must apply to be candidates in this phase.
2.  **Current Phase:**
    * Voters select their candidate.
    * The smart contract's **Transfer Vote** function is executed to generate a blockchain transaction.
    * The transaction will be confirmed and added to the blockchain ledger.
3.  **Past Phase:**
    * Election results are calculated, candidate votes are tallied, and delete transactions are initiated.
    * Calculated results are sent back to the server for local database storage.

### ➡️ Vote Transfer Flow (Transactions Flow)

| Step | Description |
| :--- | :--- |
| **A. Vote Form** | Voter fills in the election ID, selects a candidate, and enters credentials for authentication. |
| **B. User Validation** | Server accepts the request and authenticates the voter using database credentials. Voter must also enter an OTP sent to their mobile or email. |
| **C. Network Enrollment** | Request is passed to the blockchain network gateway client to enroll the user with the Certificate Authority and obtain their cryptographic information. After enrollment, the user is eligible to interact with the blockchain. |
| **D. Create Transaction** | Gateway client raises a transaction, signed with the user's private key, passing cryptographic IDs, user-ID, and election-ID to the smart contract function. |
| **E. Committing to Blockchain** | A transaction ID is returned to the user. The transaction is verified and validated by peers, then added to the blockchain, completing the cycle. |

## 🛠️ Data Model Overview (ER Diagrams)

### 1. Election and Candidates

The relationship between elections and candidates is captured, where an Election has a list of candidates competing.

| Entity: Election | Entity: Candidates |
| :--- | :--- |
| `+id` (Primary Key) | `+id` (Primary Key) |
| `+name` | `+name` |
| `+discription` | `+electionID` |
| `+phase` (Upcoming, Current, Past) | |
| `+candidates` | |

### 2. User/Voter and Additional Details

Each voter or candidate is a user. Additional information is used to define criteria for the election.

| Entity: User/Voter | Entity: Additionals |
| :--- | :--- |
| `+userID` | `+address1` |
| `+cryptoID` | `+address2` |
| `+name` | `+state` |
| `+email` | `+city` |
| `+username` | `+zip` |
| `+password` | `+adhar no.` |
| `+roles` | `+pan no.` |
| `+gender` | |
| `+additionals` | |

## 🤝 Contribution

We welcome contributions to improve this project! If you find a bug or want to add a feature, please:

1.  Open an Issue.
2.  Fork the repository.
3.  Add your features and submit a Pull Request.

## Output 

## 1. Voter Dashboard (Current Phase)
This image shows a voter logged in and ready to cast their ballot in an active election.
<img width="1024" height="893" alt="opf1" src="https://github.com/user-attachments/assets/604b5b22-8322-4000-8994-74090ae98b0b" />

## 2. Post-Vote Transaction Confirmation
This image confirms that the vote has been successfully recorded as an immutable transaction on the blockchain.
<img width="1024" height="938" alt="opf2" src="https://github.com/user-attachments/assets/96d9af01-2675-46a5-b792-e138e2a64bc9" />

## 3. Final Election Results Screen
This image displays the audited and transparent results after the voting phase has concluded.
<img width="1024" height="947" alt="opf3" src="https://github.com/user-attachments/assets/a592a320-3bb4-4b58-9785-68e9a5a1e6a9" />



