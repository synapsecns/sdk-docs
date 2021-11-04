# Example interactions with the Synapse Bridge

## How to check if "Token A" on "Network X" can be bridged to "Token B" on "Network Y"

The following shows an example of checking the "bridgeability" of various tokens too and from various networks.

```typescript
import { 
    Token, 
    Tokens, 
    ChainId, 
    Networks, 
    checkSwapSupported 
} from "@synapseprotocol/sdk";

const 
    networkX: Networks.Network = Networks.Ethereum,
    networkY: Networks.Network = Networks.Fantom,
    networkZ: Networks.Network = Networks.Polygon;

const
    tokenA: Token = Tokens.DAI,
    tokenB: Token = Tokens.MIM,
    tokenC: Token = Tokens.USDT;


// Determine if tokenA (DAI) on networkX (Ethereum mainnet) can be bridged to tokenB (MIM) on networkY (Fantom)
let [swapAllowed, errorReason] = checkSwapSupported({
   chainIdFrom: networkX.chainId,
   tokenFrom:   tokenA,
   chainIdTo:   networkY.chainId,
   tokenTo:     tokenB 
});

console.log(swapAllowed); // will output "true", as DAI is supported on Ethereum, MIM is supported on Fantom, and both tokens share a Swap Type.

// This call will return false, and the string "Token not supported on 'from' network",
// since the MIM token is not supported on the Polygon network. 
console.log(checkSwapSupported({
    chainIdFrom: networkZ.chainId,
    tokenFrom:   tokenB,
    chainIdTo:   networkX.chainId,
    tokenTo:     tokenC
}));

// This call will return false, and the string "Token swap types don't match",
// since the swap type of the SYN token is "SYN" while the swap type of USDT is "USD".  
console.log(checkSwapSupported({
    chainIdFrom: networkX.chainId,
    tokenFrom:   Tokens.SYN,
    chainIdTo:   networkY.chainId,
    tokenTo:     tokenC
}));
```

## How to get the estimated output of a Bridge from one network and token to another.

```typescript
import { 
    Token, 
    Tokens, 
    ChainId, 
    Networks,
    checkSwapSupported
    getEstimatedBridgeOutput 
} from "@synapseprotocol/sdk";

import {parseUnits} from "@ethersproject/units";
import {BigNumber} from "@ethersproject/bignumber";

const 
    networkX: Networks.Network = Networks.Polygon,
    networkY: Networks.Network = Networks.Avalanche;

const
    tokenA: Token = Tokens.USDC,
    tokenB: Token = Tokens.USDT;

// using parseUnits here to make the math a little easier.
const bridgeAmount: BigNumber = parseUnits("155", tokenA.decimals[networkX.chainId]);

const bridgeSwapParams = {
    chainIdFrom: networkX.chainId,
    tokenFrom:   tokenA,
    chainIdTo:   networkY.chainId,
    tokenTo:     tokenB
};

// always check if a swap is supported first!
let [swapSupported, notSupportedReason] = checkSwapSupported(bridgeSwapParams);

if (!swapSupported) {
    throw new Error(notSupportedReason);
}

try {
    let bridgeEstimate = await getEstimatedBridgeOutput({
        ...bridgeSwapParams,
        amountFrom:  bridgeAmount
    });
    
    // getEstimatedBridgeOutput returns an object containing the estimated output amount of the desired bridge transaction,
    // as well as the estimated fee the Synapse Bridge would take for it.
    // Do note that `amountToReceive` reflects the bridge fee already; `bridgeFee` is for showing users. 
    let { amountToReceive, bridgeFee } = bridgeEstimate;
} catch (e) {
    throw new Error(e);
}

```