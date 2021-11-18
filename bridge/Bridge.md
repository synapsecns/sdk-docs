# Bridge

`namespace Bridge` contains the class `SynapseBridge`, which wraps specific interactions with Synapse Bridge contracts on 
supported chains.

Currently, `Bridge.SynapseBridge` wraps the following interactions:

- Determining if a swap/bridge between one chain/token to another chain/token is supported
- Output estimation from one chain/token to another chain/token
- Initiating Bridge transactions from any supported chain/token to any supported chain/token


## Notes
Note that for transactions on the Boba Mainnet, the SDK will automatically populate the `gasLimit` and `gasPrice` fields.
This is not the case for any other chain at this time. 

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
import {ContractTransaction, PopulatedTransaction} from "@ethersproject/contracts";
import {Signer} from "@ethersproject/abstract-signer";

// Use your normal web3 provider here. 
// PROVIDER is for the purposes of this example only. 
const PROVIDER = new JsonRpcProvider("https://api.avax.network/ext/bc/C/rpc");

const NETWORK = Networks.AVALANCHE;

const SYNAPSE_BRIDGE = new Bridge.SynapseBridge({
    network:  NETWORK,
    provider: PROVIDER
});

const
    TOKEN_IN   = Tokens.NUSD,
    TOKEN_OUT  = Tokens.USDT,
    CHAIN_OUT  = ChainId.BSC,
    INPUT_AMOUNT: BigNumber = parseUnits("1000", tokenIn.decimals(NETWORK.chainId)); // 1000 nUSD in Wei format

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
function estimateBridgeTokenOutput() {
    SYNAPSE_BRIDGE.estimateBridgeTokenOutput({
        tokenFrom:  TOKEN_IN,      // token to send from the source chain, in this case nUSD on Avalanche
        chainIdTo:  CHAIN_OUT,     // Chain ID of the destination chain, in this case BSC
        tokenTo:    TOKEN_OUT,     // Token to be received on the destination chain, in this case USDC
        amountFrom: INPUT_AMOUNT,
    })
        .then(({ amountToReceive, bridgeFee }) => {
            let amountOutFormatted = formatUnits(
                amountToReceive,
                TOKEN_OUT.decimals(chainOut)
            );
            console.log(`${amountOutFormatted} USDT will be received on the output chain`)
        })
        .catch((err) => throw new Error(err.toString()))
}

/**
 * Use the following flow if manually constructing and sending transactions is desired.
 * 
 * Note that in this flow, both of the constructed transactions must be sent/executed manually on behalf of the user.
 * 
 * If your project maintains some sort of storage for contract approvals such that it's able to determine that an approval
 * transaction isn't necessary in a given context, then there's obviously no need to build or send an otherwise unnecessary 
 * approval transaction.
 */
async function doBridgeTransaction_manual() {
    // get minimum desired output
    const { amountToReceive } = await SYNAPSE_BRIDGE.estimateBridgeTokenOutput({
        tokenFrom:  TOKEN_IN,      // token to send from the source chain, in this case nUSD on Avalanche
        chainIdTo:  CHAIN_OUT,     // Chain ID of the destination chain, in this case BSC
        tokenTo:    TOKEN_OUT,     // Token to be received on the destination chain, in this case USDC
        amountFrom: INPUT_AMOUNT,  // Amount of `tokenFrom` being sent
    });
    
    let
        populatedApproveTxn:     PopulatedTransaction,
        populatedBridgeTokenTxn: PopulatedTransaction;

    // build an ERC20 Approve transaction to have the user send so that the Synapse Bridge contract
    // can do its thing.
    // If desired, `amount` can be passed in the args object, which overrides
    // the default behavior of "infinite approval" for the token.
    try {
        populatedApproveTxn = await SYNAPSE_BRIDGE.buildApproveTransaction({token: TOKEN_IN});
    } catch (e) {
        // handle error if one occurs
    }
    
    // insert whatever method(s) used by your project for sending the populated ERC20 Approve transaction
    
    // Now, build the actual Bridge transaction to send via web3 (or ethers, or whatever).
    try {
        populatedBridgeTokenTxn = await SYNAPSE_BRIDGE.buildBridgeTokenTransaction({
            tokenFrom:  TOKEN_IN,        // token to send from the source chain, in this case nUSD on Avalanche
            chainIdTo:  CHAIN_OUT,       // Chain ID of the destination chain, in this case BSC
            tokenTo:    TOKEN_OUT,       // Token to be received on the destination chain, in this case USDC
            amountFrom: INPUT_AMOUNT,    // Amount of `tokenFrom` being sent
            amountTo:   amountToReceive, // minimum desired amount of `tokenTo` to receive on the destination chain
        });
    } catch (e) {
        // handle error if one occurs
    }

    // insert whatever method(s) used by your project for sending the populated Bridge transaction
    
    // !!! You're done!
}

/**
 * Use the following flow if having the SDK take care of Approval and Bridge "magically" is desired.
 * 
 * Note that in this flow, a valid ethersjs Signer instance must be passed to executeApproveTransaction
 * and executeBridgeTokenTransaction so that they're able to send/execute their respective transactions on behalf 
 * of the user. 
 * 
 * If your project maintains some sort of storage for contract approvals such that it's able to determine that an approval
 * transaction isn't necessary in a given context, then there's obviously no need to send an otherwise unnecessary
 * approval transaction.
 */
async function doBridgeTransaction_magic() {
    // Obviously this should not be null in practice, but it is such here for the purposes of example documentation.
    let signer: Signer = null; 
    
    // get minimum desired output
    const { amountToReceive } = await SYNAPSE_BRIDGE.estimateBridgeTokenOutput({
        tokenFrom:  TOKEN_IN,      // token to send from the source chain, in this case nUSD on Avalanche
        chainIdTo:  CHAIN_OUT,     // Chain ID of the destination chain, in this case BSC
        tokenTo:    TOKEN_OUT,     // Token to be received on the destination chain, in this case USDC
        amountFrom: INPUT_AMOUNT,  // Amount of `tokenFrom` being sent
    });
    
    try {
        // build and execute an ERC20 Approve transaction so that the Synapse Bridge contract
        // can do its thing.
        // If desired, `amount` can be passed in the args object, which overrides
        // the default behavior of "infinite approval" for the token.
        let approveTxn: ContractTransaction = await SYNAPSE_BRIDGE.executeApproveTransaction({
            token: TOKEN_IN
        }, signer);

        // Wait for at least one confirmation on the sending chain, this is an optional
        // step and can be either omitted or implemented in a custom manner.
        await approveTxn.wait(1);

        console.log(`ERC20 Approve transaction hash: ${approveTxn.hash}`);
        console.log(`ERC20 Approve transaction block number: ${approveTxn.blockNumber}`);
    } catch (err) {
        // deal with the caught error accordingly
    }
    
    try {
        // executeBridgeTokenTransaction requires an ethers Signer instance to be 
        // passed to it in order to actually do the bridge transaction.
        // An optional field `addressTo` can be passed, which will send tokens
        // on the output chain to an address other than the address of the Signer instance.
        //
        // NOTE: executeBridgeTokenTransaction performs the step of actually sending/broadcasting the signed
        // transaction on the source chain.
        let bridgeTxn: ContractTransaction = await SYNAPSE_BRIDGE.executeBridgeTokenTransaction({
            tokenFrom:  TOKEN_IN,        // token to send from the source chain, in this case nUSD on Avalanche
            chainIdTo:  CHAIN_OUT,       // Chain ID of the destination chain, in this case BSC
            tokenTo:    TOKEN_OUT,       // Token to be received on the destination chain, in this case USDC
            amountFrom: INPUT_AMOUNT,    // Amount of `tokenFrom` being sent
            amountTo:   amountToReceive, // minimum desired amount of `tokenTo` to receive on the destination chain
        }, signer);

        // Wait for at least one confirmation on the sending chain, this is an optional
        // step and can be either omitted or implemented in a custom manner.
        await bridgeTxn.wait(1);

        console.log(`Bridge transaction hash: ${bridgeTxn.hash}`);
        console.log(`Bridge transaction block number: ${bridgeTxn.blockNumber}`);
    } catch (err) {
        // deal with the caught error accordingly
    }
    
    // !!! You're done! That was easy
}
```