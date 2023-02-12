# Loan shark
Fully collateralized bitcoin loans without custodians, escrows, or margin calls

# This project is half baked, don't expect much yet

Suppose Alice and Bob want to enter a futures contract where Alice goes long and Bob goes short. They can do this with a multisig bitcoin address and some omni usdt. Bob and Alice must exchange two signatures apiece, which will be explained in a moment, and when they do, Alice and Bob do an atomic swap: Alice sends $100 BTC to Bob and Bob puts $200 USDT into the multisig address.

The first pair of signatures Alice and Bob exchanged does this: they are only valid for a transaction that sends the money to one of Alice’s addresses after a timelock of 6 months. The second pair of signatures Alice and Bob exchanged is more complex: they use the sighash flag sighash_all | anyone_can_pay and they are only valid for a transaction that waits 5 months and 29 days before committing to the following outputs: $200 USDT goes to Bob and an amount of bitcoin currently worth $100 goes to Alice.

Ordinarily, if Bob tried to use the second pair of signatures in a bitcoin transaction, it would not be valid. The transaction they commit to has more in its outputs than in its inputs: it sends 100% of the timelocked money to Bob and leaves the extra output to Alice (currently worth $100) unfunded. You can’t create unfunded ouputs in bitcoin because that would be free money. So it looks like Bob can’t use these signatures, and thus Alice will necessarily get access to all 200 USDT when the timelock expires, right?

Wrong: the second pair of signatures uses sighash_anyone_can_pay, meaning they only sign one input to the transaction. Bob can freely add additional inputs to make the transaction valid without invalidating the pair of signatures. Bob can thus use these signatures to broadcast a valid transaction as long as he adds more inputs to actually fund the outputs that give Alice a payout according to the terms of their contract.

Importantly, only the *bitcoin value* that Bob has to pay is set in stone. Its USD value is likely to fluctuate over the course of 6 months. If, during that time, bitcoin’s price rises 10%, Bob will still owe Alice the same amount *of bitcoin* (say, .1 BTC), but its value *in USD* will be higher than the amount Alice contributed to the deposit. He will, in fact, have to obtain $110 BTC to pay her. Since he contributed $100 at the beginning plus $110 at the end, and only gets back 200 USDT, he effectively lost $10 of value, whereas Alice contributed $100 at the beginning and gets back $110 BTC at the end, thus gaining $10.

Conversely, if bitcoin’s price fell by 10%, Bob will only owe Alice $90 BTC. Since Alice contributed $100 and gets back $90 BTC, she effectively lost $10, whereas Bob contributed $100 initially plus $90 BTC at the end, but got back 200 USDT, for a total gain of $10. Alice gains if bitcoin’s price rises, so she effectively went long, whereas Bob gains if bitcoin’s price falls, so he effectively went short.

Thus Alice and Bob entered a contract for difference with no intermediary. The only other counterparty is the issuer of the USDT, who probably does not even know Alice and Bob entered this contract.
