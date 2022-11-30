# Bridge Security Checklist: Client Side

_Contact @cbym from Speabit for more information. Actively looking for feedback._

The information hereby filled by the project will serve as additional documentation to the security review.

- [About the bridge](#About-the-bridge)
- [Economic](#Economic)
- [Security Development Process](#Security-Development-Process)
- [Attack Surface](#Attack-Surface)
- [Incident Response](#Incident-Response)
- [Monitoring Systems](#Monitoring-Systems)

<br>

## To be filled by the project:

### About the bridge

#### Bridge type

- [ ] **Lock and mint:** User locks tokens in a smart contract on the source chain, then wrapped versions of those locked tokens are minted on the destination chain as a form of IOU.
- [ ] **Burn and mint:** User burns tokens on the source chain, then the same native tokens are re-issued (minted) on the destination chain.
- [ ] **Liquidity Pool:** A user locks tokens on the source chain, then unlocks the same native tokens from a liquidity pool on the destination chain.
- [ ] **Hybrid:** For some tokens it acts as Lock/Burn-and-mint, for others acts as a Liquidity Pool.

#### Centralization

- [ ] **Trusted:** Trusted bridges depend upon a central entity or system for their operations. They have trust assumptions with respect to the custody of funds and the security of the bridge. Users mostly rely on the bridge operator's reputation. Users need to give up control of their crypto assets.
- [ ] **Trust-Minimized:** Trust-minimized bridges operate using smart contracts and algorithms. They are trustless, i.e., the security of the bridge is the same as that of the underlying blockchain. Through smart contracts, trust-minimized bridges enable users to remain in control of their funds.

#### What does the bridge connect into?

- [ ] L1 <> L1.
- [ ] L1 <> L2.
- [ ] L2 <> L2.

#### With what chain(s) does it interact with?

- [ ] List chains:

#### How are messages sent from the bridge validated

- [ ] **Externally verified:** Third party (EOA, MultiSig, intermediary blockchain with their own set of validators).
- [ ] **Natively Verified:** By originating chain Validators (light client; if src chain validator say msg are valid, they are considered valid on Destination chain).
- [ ] **Optimistically verified:** Message considered valid until proven otherwise by $1/n$ watchers during the time of the fault proof window.
- [ ] **Cannonical:** Messages are validated by Ethereum smart contracts or possibly via the protocol itself) i.g., Optimistic Rollups, zkRollups..

#### Who is managing the validators / EOA / multisig / attester?

- [ ] **The bridge project itself:** The team runs and maintains nodes.
  - Is it Permissioned?
  - Describe what actions can third parties take in the event of a an issue (e.g., censorphip, technical malfunction, etc.. )
- [ ] **A third party:** outsourcing RPC providers and message relays.
  - Who is the 3rd party? Describe:

#### What could go wrong with validators? Describe the situation under which this can happen.

- [ ] **Censor** individual messages.
  - Describe.
- [ ] **Freeze / Pause** messages indefinitly.
  - Describe.
- [ ] **Forge** messages / steal funds.
  - Describe.
- [ ] **Validator keys** can be compromised.
  - Describe.
- [ ] **Exposure to** attacks, reorgs and slashing.
  - Describe.
- [ ] **Other event**, describe:

<br>
<br>

### Economic

#### How much value is currently locked in the bridge (estimation)?

- [ ] < 1 million.
- [ ] < 10 million.
- [ ] < 100 million.
- [ ] < 500 million.
- [ ] < 1 billion.

#### How much $value$ has been secured over $t$ period of time (e.g., in **11** months **7 million** have remained secured without any compromise)? Please fill in: $value$ $*$ (now - $t$)

- [ ] During the period of $t$ months, $value$ has remained secure without any compromise.

#### Economic Security

- [ ] How much would it cost to corrupt (validators) your system? Describe:

#### What would happen to funds if the bridge gets compromised (can users still withdraw? Do tokens becomes worthless?)?

- [ ] Describe:

#### Environment

- [ ] How does the system handle underlying domains with low economic security? Describe:

#### How much exposure does the bridge have to the ecosystem? (what is the economic impact if compromised)

- [ ] High.
- [ ] Medium.
- [ ] Low.
- [ ] Other, describe:

#### Are there mechanisms in place to ensure the sustainability and decentralization of relayers / sequencers?

- Describe.

<br>
<br>

### Security Development Process

#### Amount of extensive documentation and defined specifications?

- [ ] High.
- [ ] Medium.
- [ ] Low.
- [ ] None.

#### Level of extensive Testing and Fuzzing coverage as part of the CI/CD pipeline?

- [ ] High.
- [ ] Medium.
- [ ] Low.
- [ ] None.

#### Use of Static and Dynamic analysis tools (slither, echidna, Certora Prover, etc..)?

- [ ] High.
- [ ] Medium.
- [ ] Low.
- [ ] None.

#### Updated and patched packages/dependencies?

- [ ] Yes.
- [ ] No.
- [ ] N/A, describe:

#### Deployment tests?

- [ ] Yes.
- [ ] No.
- [ ] N/A, describe:

<br>
<br>

### Attack Surface

#### **Underlying blockchain**

#### What happens to the bridge if a critical chain malfunction occurs?

- [ ] Describe:

#### What trust assumptions exist about the underlying blockchain the bridge is built on?

- [ ] The chain can never freeze.
- [ ] Gas prices cannot go very high.
- [ ] Chain malfunctions or exploit in native token/message bridge.
- [ ] Chain reorgs within a certain time window.
- [ ] Chain reverts due to a fraud proof on optimistic rollups.
- [ ] Data availability issues: chain progresses without revealing data during the bridge's optimistic fraud proof window.
- [ ] Other, describe:

<br>

#### **Contracts**

#### Are contracts open source and accessible to the public?

- [ ] Yes.
- [ ] No.

#### Software license (i.g., [spdx.org](https://spdx.org))

- [ ] MIT.
- [ ] Proprietary.
- [ ] Other, describe:

#### Are contracts Upgradeable?

- [ ] Yes, upgraded by $m$-of-$n$ multisig.
- [ ] Upgraded by $m$-of-$n$ multisig after delay (e.g., 48 hours).
- [ ] No.

#### Are contracts using proxy patterns?

- [ ] Diamond Pattern.
- [ ] Transparent Proxy Pattern.
- [ ] UUPS Proxy Pattern.
- [ ] BeaconProxy.
- [ ] No.

#### How complex is the system's implementation (How hard it is for an external party to undertand)?

- [ ] High.
- [ ] Medium.
- [ ] Low.

#### Security

- [ ] Audited once.
- [ ] Audited multiple times.
- [ ] Bug Bounty program in place.
- [ ] Incident Response plan in place.
- [ ] None.
- [ ] Other, describe:

<br>

#### **Keys and Back-End**

#### Are signer keys secured using best practices (e.g., not in plain text and accessible by low permissioned users) ?

- [ ] Yes.
- [ ] No.
- [ ] Describe:

#### Has the back end been audited?

- [ ] Yes.
- [ ] No.
- [ ] N/A, describe:

#### Has the project conducted penetration testing / Red Team engagements?

- [ ] Yes.
- [ ] No.

#### Has the project suffered any kind of hack (either web2 or smart contract related)?

- [ ] Yes.
- [ ] No.

<br>

#### **Third party assumptions**

#### Is the bridge trusting 3rd parties?

- [ ] Yes.
  - Which? Describe:
- [ ] No.

#### How secure are those 3rd parties?

- [ ] High.
- [ ] Medium.
- [ ] Low.
- [ ] N/A, describe:

#### Bridge risk if 3rd party gets compromised?

- [ ] High.
- [ ] Medium.
- [ ] Low.
- [ ] N/A, describe:

<br>
<br>

### Incident Response

#### Are all organization assets and human capital identified and categorized? Reference in [Appendix A](#Appendix-A-Asset-management)

- [ ] Yes.
- [ ] No.

#### Time of the challenge window is long enough (e.g., 30 min) to provide a human level response to an incident? Note that this challenge window depends on the security of the weakest chain, therefore it must be considered per chain.

- [ ] Describe:

#### Can pause contracts as a defensive action during a compromise?

- [ ] Yes.
- [ ] No.
- [ ] Describe. (how to pause contracts, level of preparedness, tools used, team members with access to execute, etc..)

#### Clear IR Communication plan

- [ ] War room channels.
- [ ] Defined participants and responsibilities.
- [ ] Tools ready to debug exploit transactions.
- [ ] Can update User Interfaces to reflect current status.
- [ ] Available list of security partners (and projects to freeze funds e.g., USDT, USDC) .
- [ ] PR and communication plan (Twitter, Discord, Telegram, etc..).
- [ ] None.
- [ ] Other, describe:

#### How fast is the project's response time to an incident?

- [ ] <= 30 min.
- [ ] > 30 min.
- [ ] > 24h.
- [ ] N/A, describe.

#### Relationship with White hats?

- [ ] Yes.
- [ ] No.
- [ ] N/A, describe.

<br>

### Monitoring Systems

#### Network Health monitoring? e.g., monitoring the health of all chains involved, ensure liveness, data availability, fraud proofs (in optimistic rollups), exploits of canonical bridges that could compromise the value of all assets on a network, etc..

- [ ] Yes.
- [ ] No.
- [ ] Describe.

#### Smart Contract monitoring (checking broken invariants, large sums of capital flow, suspicious transactions/addresses, etc..) ?

- [ ] Yes.
- [ ] No.

#### Clear reporting guidelines available to report security issues (website information, dedicated email / Telegram, etc..)?

- [ ] Yes.
- [ ] No.

#### Automated Transaction Simulations (simulate transactions before execution)

- [ ] Yes.
- [ ] No.

<br>
<hr>

## Appendix A: Asset management

Keep an inventory of physical devices, systems, software and human resources within the organization. Each category should be classified by the organization based on criticality and business value.

- **Human Resources**
  - List of employees, collaborators, contractors, roles, access levels and responsibilities within the organization.
  - List of third party stakeholders interacting with the organization.
- **Physical devices**
  - Personnel computers and mobile devices.
  - Cold / Hardware wallets.
- **Wallets and Keys**
  - List with public keys, their purpose and their assigned manager/signer (human or bot).
- **Deployed Smart Contracts**
  - Updated list with all smart contracts and their dependencies, as well as their interaction between each other and other external services.
- **Nodes/Relays**
  - List with all nodes/relays/oracles used by the bridge.
  - Updated network health status information.
  - Event log and list of submitted proofs to target chain.
- **Servers / WebApps**
  - List of servers describing their purpose, version, running applications, hot wallets(if applicable), API endpoints and tech stack.
  - Front End tech stack and relationship with the back end.
- **WWW**
  - List of domains, sub-domains, IP's used, assigned Autonomous Systems, etc..
- **User private accounts/handles**: (In case accounts are compromised.)
  - Usernames on Discord, Twitter, Telegram, Slack, Email, etc..
  - Github Personal Access Tokens

<br>

## Appendix B: Abbreviations

APT = _Advanced Persistent Threat_
dst = _Destination_
src = _Source_
msg = _Message_
IR = _Incident Response_

<br>

## Appendix C: Bridge Security Verification Requirements

_Source: [security-verification-requirements](https://github.com/securing/SCSVS/blob/prerelease/SCSVSv2/2.0/0x200-Components/0x206-C6-Bridge.md#security-verification-requirements)_

| #        | Description                                                                                                                                      |
| -------- | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| **C6.1** | Verify that bridge requires all necessary values to be included in the message and signed: token type, chain ids, receiver, amount, nonce, etc.. |
| **C6.2** | Verify that used signatures are invalidated to protect bridge from replay attacks.                                                               |
| **C6.3** | Verify that message hash generation algorithm is resistant to collision attacks.                                                                 |
| **C6.4** | Verify that bridge includes source and destination chains identifiers in the signed message and correctly verifies them.                         |
| **C6.5** | Verify that bridge does not allow to spoof chain identifier.                                                                                     |
| **C6.6** | Verify that bridge uses a nonce parameter to allow the same operation (the same sender, receiver and amount) to be executed multiple times.      |
| **C6.7** | Verify signed message cannot be used in a differenct context (use domain separator from EIP-712).                                                |

<br>

## Appendix D: Dashboards and Links of interest

- Dashboard

  - [L2BEAT, Bridge Risk Dashboard](https://l2beat.com/?view=risk)
  - [Hop Exchange Explorer Dashboard](https://explorer.hop.exchange/)
  - [DefiLlama Bridges Dashboard](https://defillama.com/bridges)

- Toolkits, Frameworks and Guidelines

  - [Simple Security Toolkit](https://github.com/nascentxyz/simple-security-toolkit/blob/main/incident-response-plan-template.md)
  - [L2BEAT, Bridge Risk Framework](https://gov.l2beat.com/t/l2bridge-risk-framework/31)
  - [Yearn Finance Emergency Procedures](https://docs.yearn.finance/developers/v2/EMERGENCY)
  - [EEA CIW - Crosschain Security Guidelines](https://entethalliance.github.io/crosschain-interoperability/crosschainsecurityguidelines.html)

- Talks

  - [DeFi Security Summit Day 2 Session 11: Bridges](https://www.youtube.com/watch?v=OIY2mcppo14)
  - [Rollups Are the Most Secure Bridges - DevCon Bogota](https://www.youtube.com/watch?v=10welw1d_Bc)
  - [Nuts and Bolts of Cross-Chain Communication - Devcon 5](https://www.youtube.com/watch?v=Ndwm4o4F6Xk)
  - [Lessons from the Nomad Hack - DevCon Bogota](https://www.youtube.com/watch?v=Ow8YXfv02Jo)
    summary: https://twitter.com/odysseas_eth/status/1583095789424848899 / https://typefully.com/odysseas_eth/XQskWEh
  - [Securing Cross-chain Communication - DevCon Bogota](https://www.youtube.com/watch?v=sAVhW_xqrvY)

- Standards
  - [EIP-5164-cross-chain-execution](https://ethereum-magicians.org/t/eip-5164-cross-chain-execution/9658)
  - [OpenZeppelin cross-chain support](https://docs.openzeppelin.com/contracts/4.x/crosschain)
