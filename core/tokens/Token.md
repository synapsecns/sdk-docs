# Token

`Token` is a class which can represent any ERC20-compatible token on any given network, and is a data class.

The Synapse Protocol SDK provides pre-defined `Token` instances for all tokens supported by the Synapse Protocol on all supported networks
in the `Tokens` namespace, which is easily imported like so:

```typescript
import {Tokens, ChainId} from "@synapseprotocol/sdk";

console.log(Tokens.USDC.address(ChainId.BSC));
```

You should not expect to ever have to instantiate a `Token` manually, and using a manually instantiated `Token` with
various functions in the SDK will likely cause undefined behavior.

## Instance variables

Unless otherwise specified, all instance variables are **read only** and set by the `constructor`.

- `name` (`string`) - The "official" name of the token. Example: "USD Circle" (for USDC)
- `symbol` (`string`) - Symbol the token is known by both on and off-chain. Examples: "USDT" (Tether's USD stablecoin), "DOGE" (for Dogecoin)
- `addresses` (`{ [chainId: number] => string }`) - This field is similarly shaped to `decimals`, in that it is a mapping with shape `chain id => address for chain (string)`,
    containing the on-chain addresses of this token for respective Chain ID values.

## Functions 

- `address`
  - Returns the on-chain address of this token respective to the passed Chain ID.
  - Params:
    - `chainId: number` - Chain ID of the chain for which you want this token's on-chain address.
  - Returns:
    - `string`
    - The on-chain address of this token respective to `network`, or `null` if the token is not supported by the Synapse Protocol on the given network. 
- `decimals`
  - Returns the on-chain decimals value of this token respective to the passed ChainID.
  - Params:
      - `chainId: number` - Chain ID of the chain for which you want this token's on-chain decimals.
  - Returns:
      - `number`
      - The on-chain decimals value of this token respective to `network`, or `null` if the token is not supported by the Synapse Protocol on the given network. 