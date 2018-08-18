
GOAL: We want stable channels in adverse scenarios

Fee estimation handling for different lightning implementations:

Relevant chapter in:
- BOLT#2: https://github.com/lightningnetwork/lightning-rfc/blob/master/02-peer-protocol.md#updating-fees-update_fee
- BOLT#3: https://github.com/lightningnetwork/lightning-rfc/blob/master/03-transactions.md#fees

What happens when a non-funding node detect a fee estimate spike:


c-lightning: passive/ignores
code link: https://github.com/ElementsProject/lightning/blob/43f31c52bfb0015df07ca140d5a00655305eb7a2/channeld/channel.c#L2138-L2148
poll-interval = 30s



lnd: passive/ignores
code link: https://github.com/lightningnetwork/lnd/blob/7a113d469bb902842e8c38034ba4892ca593173c/htlcswitch/link.go#L875-L905
poll-interval = 



eclair: NOT passive, check for 
abs((local feerate - remote fee rate) / (local fee rate + remote fee rate)/2) > 1.5
hmm. not sure what happens then, close?

code link: https://github.com/ACINQ/eclair/blob/82973cdb76a982011a67d995c42311169049f6cb/eclair-core/src/main/scala/fr/acinq/eclair/channel/Channel.scala#L751-L760

action code: https://github.com/ACINQ/eclair/blob/82973cdb76a982011a67d995c42311169049f6cb/eclair-core/src/main/scala/fr/acinq/eclair/channel/Helpers.scala#L145-L152

and: https://github.com/ACINQ/eclair/blob/82973cdb76a982011a67d995c42311169049f6cb/eclair-core/src/main/resources/reference.conf#L62-L64

poll-interval = 600s, code: https://github.com/ACINQ/eclair/blob/82973cdb76a982011a67d995c42311169049f6cb/eclair-core/src/main/scala/fr/acinq/eclair/Setup.scala#L154

Incentives:
BOLT#3 states:
> The fee calculation for both commitment transactions and HTLC transactions is based on the current feerate_per_kw

and

> Base commitment transaction fees are extracted from the funder's amount

Both nodes want timeply processing of commitment transactions, so both nodes care that transaction fees are not too low. The channel-funding node pays the fees in the commitment transaction, so it also cares that fees are not too high. Fees in HTLC-Timeout and HTLC-Succes transactions are payed by the recipient of their output, which can be either node in any case (spend via penalty-clause or delayed-clause). So when a commitment transaction contains an HTLC output, the non-funding node also cares that fees are not too high.
Simplified: when a commitment transaction contains HTLC's, both nodes care that channel feerate is not too low and not too high! 

#TODO: Feerate estimation spike scenarios:
- for example the fee situation in dec 2018 (https://p2sh.info/dashboard/db/fee-estimation?orgId=1&var-source=bitcoind&from=1509530451002&to=1517479251002)
- either node has not enough balance in channel to affort fees -> unilateral_close gets expensive in terms of fees
- strategies to wait for feerate storm to blow over
