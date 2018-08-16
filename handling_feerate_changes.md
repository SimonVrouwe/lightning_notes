
Fee estimation handling for different lightning implementations:

Relevant chapter in BOLT: https://github.com/lightningnetwork/lightning-rfc/blob/master/02-peer-protocol.md#updating-fees-update_fee

What happens when a non-funding node detect a fee estimate spike:

c-lightning: passive/ignores
code link: https://github.com/ElementsProject/lightning/blob/43f31c52bfb0015df07ca140d5a00655305eb7a2/channeld/channel.c#L2138-L2148

lnd: passive/ignores
code link: https://github.com/lightningnetwork/lnd/blob/7a113d469bb902842e8c38034ba4892ca593173c/htlcswitch/link.go#L875-L905

eclair:
