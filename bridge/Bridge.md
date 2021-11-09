# Bridge

`namespace Bridge` contains the class `SynapseBridge`, which wraps specific interactions with Synapse Bridge contracts on 
supported chains.

Currently, `Bridge.SynapseBridge` wraps the following interactions:

- Determining if a swap/bridge between one chain/token to another chain/token is supported
- Output estimation from one chain/token to another chain/token
- Initiating Bridge transactions from any supported chain/token to any supported chain/token

## Examples

```typescript
import {
    Bridge, 
    Tokens, 
    ChainId, 
    Networks
} from "@synapseprotocol/sdk";

import {JsonRpcProvider} from "@ethersproject/providers";
import {parseUnits, formatUnits} from "@ethersproject/units";
import {BigNumber} from "@ethersproject/bignumber";
import {ContractReceipt} from "@ethersproject/contracts";

// Use your normal web3 provider here. 
// PROVIDER is for the purposes of this example only. 
const PROVIDER = new JsonRpcProvider("https://api.avax.network/ext/bc/C/rpc");

const NETWORK = Networks.Avalanche;

const SYNAPSE_BRIDGE = new Bridge.SynapseBridge({
    network:  NETWORK,
    provider: PROVIDER
});

const
    TOKEN_IN   = Tokens.NUSD,
    TOKEN_OUT  = Tokens.USDT,
    CHAIN_OUT  = ChainId.BSC,
    INPUT_AMOUNT: BigNumber = parseUnits("1000", tokenIn.decimals[NETWORK.chainId]); // 1000 nUSD in Wei format

function checkSwapSupported() {
    let [swapSupported, notSupportedReason] = SYNAPSE_BRIDGE.swapSupported({
        tokenFrom:  TOKEN_IN,  // token to send from the source chain, in this case nUSD on Avalanche
        chainIdTo:  CHAIN_OUT, // Chain ID of the destination chain, in this case BSC
        tokenTo:    TOKEN_OUT, // Token to be received on the destination chain, in this case USDC
    });
    
    // swapSupported will be `true` if the swap/bridge between tokens on networks is possible,
    // along with an empty string.
    // If a swap/bridge ISN'T supported, swapSupported will be `false`, and `notSupportedReason`
    // will provide a short reason for why the swap/bridge isn't possible.
}

// get the estimated output for a bridge from nUSD on Avalanche
// to USDT on Binance Smart Chain.
function getOutputEstimate() {
    SYNAPSE_BRIDGE.getOutputEstimate({
        tokenFrom:  TOKEN_IN,      // token to send from the source chain, in this case nUSD on Avalanche
        chainIdTo:  CHAIN_OUT,     // Chain ID of the destination chain, in this case BSC
        tokenTo:    TOKEN_OUT,     // Token to be received on the destination chain, in this case USDC
        amountFrom: INPUT_AMOUNT,
    })
        .then(({ amountToReceive, bridgeFee }) => {
            let amountOutFormatted = formatUnits(
                amountToReceive,
                tokenOut.decimals[chainOut]
            );
            console.log(`${amountOutFormatted} USDT will be received on the output chain`)
        })
        .catch((err) => throw new Error(err.toString()))
}

async function doBridgeTransaction() {
    // get minimum desired output
    const { amountToReceive } = await SYNAPSE_BRIDGE.getOutputEstimate({
        tokenFrom:  TOKEN_IN,      // token to send from the source chain, in this case nUSD on Avalanche
        chainIdTo:  CHAIN_OUT,     // Chain ID of the destination chain, in this case BSC
        tokenTo:    TOKEN_OUT,     // Token to be received on the destination chain, in this case USDC
        amountFrom: INPUT_AMOUNT,  // Amount of `tokenFrom` being sent
    });
    
    // initiateBridgeTransaction requires an ethers Signer instance to be 
    // passed to it in order to actually do the bridge transaction.
    // An optional field `addressTo` can be passed, which will send tokens
    // on the output chain to an address other than the address of the Signer instance.
    let txReceipt: ContractReceipt = await SYNAPSE_BRIDGE.initiateBridgeTransaction({
        tokenFrom:  TOKEN_IN,        // token to send from the source chain, in this case nUSD on Avalanche
        chainIdTo:  CHAIN_OUT,       // Chain ID of the destination chain, in this case BSC
        tokenTo:    TOKEN_OUT,       // Token to be received on the destination chain, in this case USDC
        amountFrom: INPUT_AMOUNT,    // Amount of `tokenFrom` being sent
        amountTo:   amountToReceive, // minimum desired amount of `tokenTo` to receive on the destination chain
        signer:     null, // this should NOT be null when being used for real
    });
    
    // txReceipt is a fully populated ethers ContractReceipt instance.
}
```