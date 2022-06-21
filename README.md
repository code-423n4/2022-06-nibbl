# ✨ So you want to sponsor a contest

This `README.md` contains a set of checklists for our contest collaboration.

Your contest will use two repos: 
- **a _contest_ repo** (this one), which is used for scoping your contest and for providing information to contestants (wardens)
- **a _findings_ repo**, where issues are submitted. 

Ultimately, when we launch the contest, this contest repo will be made public and will contain the smart contracts to be reviewed and all the information needed for contest participants. The findings repo will be made public after the contest is over and your team has mitigated the identified issues.

Some of the checklists in this doc are for **C4 (🐺)** and some of them are for **you as the contest sponsor (⭐️)**.

---

# Contest setup

## ⭐️ Sponsor: Provide contest details

Under "SPONSORS ADD INFO HERE" heading below, include the following:

- [ ] Name of each contract and:
  - [ ] source lines of code (excluding blank lines and comments) in each
  - [ ] external contracts called in each
  - [ ] libraries used in each
- [ ] Describe any novel or unique curve logic or mathematical models implemented in the contracts
- [ ] Does the token conform to the ERC-20 standard? In what specific ways does it differ?
- [ ] Describe anything else that adds any special logic that makes your approach unique
- [ ] Identify any areas of specific concern in reviewing the code
- [ ] Add all of the code to this repo that you want reviewed
- [ ] Create a PR to this repo with the above changes.

---

# Contest prep

## ⭐️ Sponsor: Contest prep
- [ ] Make sure your code is thoroughly commented using the [NatSpec format](https://docs.soliditylang.org/en/v0.5.10/natspec-format.html#natspec-format).
- [ ] Modify the bottom of this `README.md` file to describe how your code is supposed to work with links to any relevent documentation and any other criteria/details that the C4 Wardens should keep in mind when reviewing. ([Here's a well-constructed example.](https://github.com/code-423n4/2021-06-gro/blob/main/README.md))
- [ ] Please have final versions of contracts and documentation added/updated in this repo **no less than 24 hours prior to contest start time.**
- [ ] Ensure that you have access to the _findings_ repo where issues will be submitted.
- [ ] Promote the contest on Twitter (optional: tag in relevant protocols, etc.)
- [ ] Share it with your own communities (blog, Discord, Telegram, email newsletters, etc.)
- [ ] Optional: pre-record a high-level overview of your protocol (not just specific smart contract functions). This saves wardens a lot of time wading through documentation.
- [ ] Delete this checklist and all text above the line below when you're ready.

---

# Nibbl contest details
- $28,500 USDC main award pot
- $1,500 USDC gas optimization award pot
- Join [C4 Discord](https://discord.gg/code4rena) to register
- Submit findings [using the C4 form](https://code4rena.com/contests/2022-06-nibbl-contest/submit)
- [Read our guidelines for more details](https://docs.code4rena.com/roles/wardens)
- Starts June 21, 2022 20:00 UTC
- Ends June 24, 2022 19:59 UTC

This repo will be made public before the start of the contest. (C4 delete this line when made public)

[ ⭐️ SPONSORS ADD INFO HERE ]

Nibbl is a fractionalization protocol that creates ERC20 tokens representing fractional ownership of an ERC721.

Nibbl uses a Bonding curve to facilitate trading of the ERC20 tokens.

The protocol also implements a valuation-based buyout mechanism so that the ERC721 isn’t locked in a contract/vault forever. A user can initiate a buyout and they need to pay an upfront cost for that. The cost is decided based on the current valuation of tokens. If the valuation goes above a certain level within a predefined duration the buyout is rejected. Therefore, the community can buy more tokens in order to reject a buyout. If the buyout isn’t rejected it is automatically considered successful and the user who initiated the buyout can withdraw the asset.

# Important Links

Code Repository: https://github.com/NibblNFT/nibbl-code4arena-june-2022

Documentation: https://github.com/NibblNFT/nibbl-code4arena-june-2022/blob/master/README.md

Code and Architecture Walkthrough: https://youtu.be/dJFOgo58qVg

Product Beta: http://beta.nibbl.xyz/

# To run tests
To run tests, run following commands:
```
$ npm install
$ npx hardhat test
```
# Contracts in Scope
| File | LoC | External Calls | Description |
| --- | --- | --- | --- |
| NibblVaultFactory.sol | 70 | 0 | Vault Factory that deploys vault and handles governance and access control. |
| NibblVault.sol | 290 | 0 | Vault which holds NFT and has logic for trading and buyout |
| Basket.sol | 80 | 0 | Basket that can be used to fractionalize multiple NFTs. |
| Twav.sol | 25 | 0 | Implements time-weighted valuation to be consumed in NibblVault for buyouts |
| ProxyVault.sol | 17 | 0 | Proxy contract that gets deployed with implementation as NibblVault |
| ProxyBasket.sol | 17 | 0 | Proxy contract that gets deployed with implementation as Basket |
| AccessControlMechanism.sol | 19 | 0 | Inherited in NibblVaultFactory for access control on certain actions |
| EIP712Base.sol | 21 | 0 | To implement permit functionality with EIP712 signing. |


# Out of scope

1. Admin can pause and change certain parameters of the contract.
2. BancorFormula.sol: Implementation of Bancor Formula.

# Areas of concern
1. Trade Accounting
  - The secondary and primary reserve balances (including fictitious primary reserve balance) should update correctly in all 3 trade scenarios - trade in primary curve, trade in secondary curve, trade in both primary and secondary curve.
  - Admin fee, curator fee and curve fee should update correctly.

2. Upgradablity, pausability and access control

3. TWAV
  - Values get updated and computed correctly

4. Buyout
  - Game theoretical issues in the buyout game
  - Accounting issues while initiating buyout and token redemption
  - Possible manipulation scenarios with TWAV array
  - Potential issue with minimum buyout time as that is recently added feature
  - Only successful bidder should be able to withdraw NFT

# Previous Audit
1. https://github.com/NibblNFT/nibbl-code4arena-june-2022/tree/master/Previous%20Audits