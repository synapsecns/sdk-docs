# SwapType

`namespace SwapType` provides six constant string values which denote "Swap Types". "Swap Types" are a method of categorizing 
tokens so as to determiine which other tokens they can be swapped or bridged to. 

As a rule, tokens can only be bridged to tokens with the same "Swap Type". For example:

- USDC (Swap Type: "USD") on network A -> USDT (Swap Type: "USD") on network B: **ALLOWED**
- MIM (Swap Type: "USD") on Fantom -> NUSD (Swap Type: "USD") on Avalanche: **ALLOWED**
- SYN (Swap Type: "SYN") on BSC -> HIGH (Swap Type: "HIGHBRIDGE") on Ethereum: **NOT ALLOWED**

## Swap Types

- USD
  Most Protocol-supported tokens which are "stablecoins" -- such as nUSD, DAI, USDC, and so forth -- are given the **USD** swap type.
- SYN
  This swap type applies exclusively to the SYN token. The SYN token cannot be bridged to a different token, only to itself,
- ETH
  "Wrapped ETH" (WETH), nETH (Synapse's Ethereum LP), and the native currencies of the Ethereum and Arbitrum networks are given this swap type.
  The ETH swap type is used for bridging between the Arbitrum and Ethereum networks.
- HIGHSTREET
  This swap type is given exclusively to the HIGH (Highstreet) token. 
- JUMP
  This swap type is given exclusively to the JUMP (HyperJump) token.
- DOG
  This swap type is given exclusively to the DOG (Doge NFT).

