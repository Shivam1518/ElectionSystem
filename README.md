Blockchain-based E-Voting System

Short description
A private-blockchain electronic voting system implementing voter registration, candidate enrollment, and auditable vote transfer via smart contracts. Designed for high security, transparency, and scalability in institutional elections. This README synthesizes the implementation, data model, smart-contract API, deployment notes, and security considerations from the project paper. 

271

Table of contents

Project overview

Key features

High-level architecture

Data model (ER highlights)

Smart contract interface & flow

Installation & local run (developer guide)

API & frontend integration (examples)

Security considerations & limitations

Testing & verification suggestions

Contribution

References

Project overview

This repository implements a permissioned (private) blockchain e-voting system where:

election lifecycle has three phases: Upcoming, Current, Past;

users (voters/candidates) provide identity and KYC-like data (Aadhaar, PAN, address) to support election criteria;

voters enroll with a certificate authority to obtain crypto identity;

votes are submitted as blockchain transactions signed by the voter's private key and immediately visible on the ledger for client-side verification. 

271

Key features

Permissioned blockchain (private network) for controlled access and faster transactions. 

271

Smart-contract driven vote transfer (immutability + on-chain tallying).

Voter authentication with OTP and server-side validation before enrollment.

Vote verification by transaction ID returned to voter after commit.

Election lifecycle management (create, start/stop phases, dispose).

Candidate application during Upcoming phase; no registration changes during Current phase. 

271

High-level architecture

Components:

Frontend: voter/candidate/admin UI (React/Vue) — forms for registration, candidate application, voting, results.

Backend / API server: user management, OTP service, gateway to blockchain network, certificate authority client.

Certificate Authority (CA): issues cryptographic identity to enrolled users.

Blockchain network: permissioned peers (e.g., Hyperledger Fabric or private Ethereum Consortium).

Smart contract(s): election contract implementing transferVote, getElectionState, disposeElection.

Local DB: stores user profiles, election metadata, and off-chain copies of final results for easy queries. 

271

Sequence (vote flow):

User logs in → server validates credentials + OTP.

Server requests enrollment from CA → receives crypto identity.

Frontend submits vote (electionID, voter cryptoID, candidate cryptoID).

Gateway signs and submits blockchain transaction → returns txID.

Peers validate & commit → ledger updated; client can query txID for proof. 

271

Data model (ER highlights)

Minimal entities:

User: { id, name, email, username, password_hash, role, crypto_id, address, aadhaar, pan, city, state, zip, gender }. 

271

Election: { election_id, name, description, phase (upcoming|current|past), criteria, start_time, end_time }.

Candidate: { candidate_id, user_id, crypto_id, election_id, manifesto }.

VoteTransaction (on-chain): { tx_id, election_id, voter_crypto_id, candidate_crypto_id, timestamp } (stored on ledger).

Off-chain results stored in local DB after Past phase for reporting.

Smart contract interface & flow
Core functions (as implemented / recommended)

transferVote(electionID, voterCryptoID, voterLocalID, candidateCryptoID, candidateLocalID) -> txID

Validates election is in Current phase.

Validates voter eligibility (meets election criteria & not already voted).

Records vote and emits event. 

271

getElectionState(electionID) -> {phase, start, end, tally}

Returns phase and optionally current tallies (if allowed by privacy policy). 

271

disposeElection(electionID)

Finalizes election, triggers off-chain result persist & any cleanup. 

271

Additional recommended helpers

registerCandidate(electionID, candidateCryptoID, metadata) (only during Upcoming).

enrollVoter(voterLocalID, publicKey) — typically done by CA/gateway off-chain.

Event VoteCast(txID, electionID, candidateCryptoID) for realtime client updates.

Installation & local run (developer guide)

Assumptions: you will use a permissioned ledger (Hyperledger Fabric recommended for private deployments). Adjust commands for your chosen stack.

Clone

git clone <this-repo>
cd <this-repo>


Prerequisites

Node.js >= 16, npm/yarn

Docker & Docker Compose (for local Fabric network)

Go / Fabric samples if using Hyperledger Fabric CLI tools

Start a local permissioned network (Fabric example)

Use Fabric test network or scripts under infra/:

cd infra/fabric
./network.sh up createChannel -c mychannel
# install chaincode, instantiate, approve etc (scripts provided)


Backend

cd backend
cp .env.example .env
# set CA, wallet, connection profile paths
npm install
npm run start:dev


Frontend

cd frontend
npm install
npm run dev


Smart contract (chaincode)

Build and deploy per Fabric chaincode lifecycle.

Example pseudo path: contracts/election/contract.js (or go/solidity depending on platform).

Local test data

Provide a seed script to create admin, a few voters/candidates, and an election in Upcoming phase.

API & frontend integration (examples)
Example REST endpoints (backend)

POST /api/auth/login — returns session token.

POST /api/auth/otp — request/validate OTP for vote operation.

POST /api/enroll — enroll user with CA (returns cryptoID).

POST /api/elections — admin: create election.

GET /api/elections/:id — get election metadata & state.

POST /api/elections/:id/vote — cast vote (body contains voterLocalID, voterCryptoID, candidateCryptoID).

Example vote payload
{
  "electionID": "election-2025-01",
  "voterLocalID": "user-123",
  "voterCryptoID": "0xABCDEF...",
  "candidateCryptoID": "0xCANDID1..."
}

Client behavior after successful transaction

Receive txID → display to voter with clear instructions: "Copy this txID to verify your vote on the ledger."

Provide a link / UI to query GET /api/ledger/tx/:txID for verification.

Security considerations & limitations

Security improvements vs risks (from paper):

Private blockchain reduces unauthorised access and improves throughput. 

271

BUT blockchains are not a panacea: smart-contract bugs, CA compromise, and insider threats remain major risks. 

271

Specific concerns & mitigations

Authentication: rely on multi-factor (password + OTP) and strong session management.

CA security: protect CA private keys; use HSM and multi-admin operations.

Smart-contract correctness: formal verification or rigorous audits; minimize on-chain logic complexity.

Privacy: do not store personally identifiable info (Aadhaar/PAN) on-chain. Keep PII off-chain and only store cryptographic identifiers on ledger. 

271

Scalability: private networks scale better, but plan for transaction volume, endorsement policies, and peer capacity. 

271

Replay / double-vote prevention: enforce one-vote per voter per election in chaincode and track voting state atomically.

Limitations called out in the source

Energy, interoperability, and 51% attack concerns for public blockchains — less relevant for permissioned but still cited as considerations. 

271

Testing & verification suggestions

Unit tests for chaincode functions (transferVote, getElectionState).

Integration tests: simulate enrollment → vote → ledger commit → result match in off-chain DB.

Penetration tests: focus on backend endpoints, CA, and smart-contract interfaces.

Audit trail verification: cross-check on-chain tallies vs. off-chain stored results after Past phase.

Third-party review: have independent security audit of smart contracts and CA setup; replicate the PwC-style audit referenced in literature for large deployments. 

271

Contribution

Fork repository

Create feature branch feature/<name>

Run tests & linters locally

Open pull request with design notes and test results

Please include: smart contract test coverage, load-testing results for your target network, and security review notes.

License

Choose an appropriate license for your code base (e.g., MIT, Apache-2.0). Add LICENSE file to repository.

References

Primary source summarised in this README: Blockchain based e-voting system (project paper). 

271

Other references and literature are recommended for implementation decisions (consensus choice, best CA practices, smart-contract audits), but are not embedded here.

Appendix — Quick smart-contract pseudocode (readable)
// pseudo-chaincode
function transferVote(electionID, voterCryptoID, voterLocalID, candidateCryptoID) {
  let election = readState('election-'+electionID);
  if (election.phase !== 'current') throw Error('Election not accepting votes');
  if (!eligibilityCheck(voterLocalID, election.criteria)) throw Error('Not eligible');
  if (hasVoted(electionID, voterCryptoID)) throw Error('Already voted');

  // create vote record
  let vote = {
    txID: generateTxID(),
    electionID,
    voterCryptoID,
    candidateCryptoID,
    timestamp: now()
  };
  // persist vote (on-chain)
  putState('vote-'+vote.txID, vote);
  // increment candidate tally (on-chain or via separate aggregate)
  emitEvent('VoteCast', vote);
  return vote.txID;
}
