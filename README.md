# Loan shark
Fully collateralized bitcoin only loans without custodians, escrows, or margin calls

# How to try it
Click here: https://supertestnet.github.io/loan-shark/

# Video explainer

[![](https://supertestnet.github.io/loan-shark/loan-shark-thumbnail.png)](https://www.youtube.com/watch?v=EJICv6P48wU)

# Info about old version that used tether

Suppose Alice wants to loan Bob some bitcoins and get them back later with interest. She also wants to secure herself against Bob running away with the bitcoins by means of collateral deposited into a contract by Bob, where the collateral is double the current value of the principle of the loan. Alice and Bob can do this with a multisig bitcoin address and some omni usdt. Bob and Alice must exchange two signatures apiece, which will be explained in a moment, and when they do, Alice and Bob do an atomic swap: Alice sends $100 BTC to Bob and Bob puts $200 USDT into the multisig address.

The first pair of signatures Alice and Bob exchanged does this: they are only valid for a transaction that sends the $200 USDT to one of Alice’s addresses after a timelock of 6 months. The second pair of signatures Alice and Bob exchanged is more complex: they use the sighash flag sighash_all | anyone_can_pay and they are only valid for a transaction that commits to the following outputs: $200 USDT goes to Bob and an amount of bitcoin currently worth $100 goes to Alice.

Ordinarily, if Bob tried to use the second pair of signatures in a bitcoin transaction, it would not be valid. The transaction they commit to has more in its outputs than in its inputs: it sends 100% of the timelocked money to Bob and leaves the extra output to Alice (currently worth $100) unfunded. You can’t create unfunded ouputs in bitcoin because that would be free money. So it looks like Bob can’t use these signatures, and thus Alice will necessarily get access to all 200 USDT when the timelock expires, right?

Wrong: the second pair of signatures uses sighash_anyone_can_pay, meaning they only sign one input to the transaction. Bob can freely add additional inputs to make the transaction valid without invalidating the pair of signatures. Bob can thus use these signatures to broadcast a valid transaction as long as he adds more inputs to actually fund the outputs that give Alice a payout according to the terms of their contract.

If Bob waits til the last minute to pay Alice back, and bitcoin’s price rises 10%, Bob will essentially owe Alice $110 BTC. If bitcoin's price rises by more than 200% during the term of six months, the amount of money Bob is meant to pay Alice will exceed the collateral he deposited, so his incentive is to forfeit the collateral and keep the bitcoin. To mitigate this risk, Alice can require Bob to post large amounts of collateral.

This type of loan does not have margin calls. If the price of bitcoin rises or falls substantially, neither party is in a position where they have to put up more money to avoid losing the money they already put up earlier. Instead, the contract is essentially a collateralized futures contract where Bob has an option to buy back his collateral. By agreeing to a contract where $200 is the collateral and $100 of bitcoin is the principle, Alice effectively agrees that if the price of bitcoin increases by a factor of 200% (or whatever the agreed upon ratio is), she is fine with just getting Bob's $200 collateral. Thus Bob effectively has the option to either keep Alice's bitcoins and forfeit his $200 or *buy back* his $200 by paying Alice the principle plus interest.

Thus Alice and Bob entered a loan contract that is also an options contract and a futures contract. They did not require any intermediary, there was no escrow, and by structuring the loan as an options contract, the need for margin calls is also eliminated. The only counterparty besides Alice and Bob is the issuer of the USDT, who may not even realize Alice and Bob entered this contract.
