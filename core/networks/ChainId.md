# ChainId

`namespace ChainId` provides constants of Chain IDs for blockchains supported by the SDK.

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
  
  if (paramAsNum === ChainId.Fantom) {
    console.log(`${paramAsNum} is the Chain ID of the Fantom network!`);
  }
}
```

## Supported Chain IDs

All of the below are standard NodeJS `number`s, exported as constants from the `ChainId` namespace. All of these Chain IDs
are supported and useable on the Synapse Protocol.

- `Arbitrum`
  - Value: `42161`
  
- `Avalanche`
  - Value: `43114`

- `BSC`
  - Value: `56`

- `Ethereum`
  - Value: `1`

- `Fantom`
  - Value: `250`

- `Polygon`
  - Value: `137`

- `Harmony`:
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
- `supportedNetworks`
  - Returns an array of `Network` instances corresponding to all networks supported by the Synapse Protocol.
  - Returns:
    - `Network[]`
    - Array which contains the pre-defined `Network` instances `Arbitrum`, `Avalanche`, `BSC`, `Ethereum`, `Fantom`, `Polygon`, and `Harmony`.