
Fee estimation handling for different lightning implementations:

Relevant chapter in BOLT: https://github.com/lightningnetwork/lightning-rfc/blob/master/02-peer-protocol.md#updating-fees-update_fee

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
