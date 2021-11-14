# ChainId

The `ChainId` namespace provides constants of Chain IDs for blockchains supported by the SDK.

All Chain IDs defined in this namespace are exported as the standard JS `number` type. This is to enable simple interoperability of these values between various codebases. 

## Import
The ChainId namespace can be imported like so:

```typescript
import { ChainId } from "@synapseprotocol/sdk";

// or, for potentially better tree shaking:
import { ChainId } from "@synapseprotocol/sdk/common";
```

## Example Usage
```typescript
import { ChainId } from "@synapseprotocol/sdk";
import { BigNumberish } from "@ethersproject/bignumber";

// Check a normal `number` argument against a known Chain ID.
function example(chainId: number) {
  if (chainId === ChainId.BSC) {
    console.log(`${chainId} is the Chain ID of the Binance Smart Chain!`);
  }
}

// Check a BigNumberish (which could be a whole host of types)
// against a known Chain ID.
function example2(chainId: BigNumberish) {
  let paramAsNum = ChainId.asNumber(chainId);
  
  if (paramAsNum === ChainId.FANTOM) {
    console.log(`${paramAsNum} is the Chain ID of the Fantom network!`);
  }
}
```

## Supported Chain IDs

All of the below are standard NodeJS `number`s, exported as constants from the `ChainId` namespace. All of these Chain IDs
are supported and useable on the Synapse Protocol.

- `ETH` (Ethereum mainnet)
  - Value: `1`

- `BSC` (Binance Smart Chain mainnet)
  - Value: `56`

- `POLYGON` (Polygon Mainnet)
  - Value: `137`

- `FANTOM` (Fantom mainnet)
  - Value: `250`

- `BOBA` (Boba mainnet)
  - Value: `288`

- `ARBITRUM` (Arbitrum mainnet)
  - Value: `42161`
  
- `AVALANCHE` (Avalanche C-Chain mainnet)
  - Value: `43114`
  
- `HARMONY` (Harmony mainnet)
  - Value: `1666600000`

## Functions

- `asBigNumber`
  - Converts the passed `BigNumberish` into a `BigNumber`. 
  - Params:
    - `n: BigNumberish`
  - Returns: 
    - `BigNumber`
    - `n` casted into a `BigNumber` instance.

- `asBigNumber`
  - Converts the passed `BigNumberish` into a `number`. 
  - Params:
    - `n: BigNumberish`
  - Returns: 
    - `number`
    - `n` returned as a standard NodeJS `number`.

- `supportedChainIds`
  - Returns an array of `numbers` corresponding to the chain ids of all networks supported by the Synapse Protocol.
  - Returns:
    - `number[]`
    - Array which contains the pre-defined chain id constants `ETH`, `BSC`, `POLYGON`, `FANTOM`, `BOBA`, `ARBITRUM`, `AVALANCHE`, and `HARMONY`.