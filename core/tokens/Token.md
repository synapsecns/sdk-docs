# Token

`Token` is a class which can represent any ERC20-compatible token on any given network, and is a data class.

The Synapse Protocol SDK provides pre-defined `Token` instances for all tokens supported by the Synapse Protocol on all supported networks
in the `Tokens` namespace, which is easily imported like so:

```typescript
import {Tokens} from "@synapseprotocol/sdk";
```

You should not expect to ever have to instantiate a `Token` manually, and using a manually instantiated `Token` with
various functions in the SDK will likely cause undefined behavior.

## Instance variables

Unless otherwise specified, all instance variables are **read only** and set by the `constructor`.

- `name` (`string`) - The "official" name of the token. Example: "USD Circle" (for USDC)
- `symbol` (`string`) - Symbol the token is known by both on and off-chain. Examples: "USDT" (Tether's USD stablecoin), "DOGE" (for Dogecoin)
- `decimals` (`{ [chainId: number] => number }`) - `decimals` is a mapping of the shape `chain id => decimals for chain (number)`, and is populated with the number of decimals
    used for this token on each chain for which the Synapse Protocol supports it. Generally speaking, this value will be the same across chains,
    though there are some exceptions: for example, USDC and USDT both use 18 decimal places for precision on the Binance Smart Chain, but 6 decimals
    on all other chains.
- `addresses` (`{ [chainId: number] => string }`) - This field is similarly shaped to `decimals`, in that it is a mapping with shape `chain id => address for chain (string)`,
    containing the on-chain addresses of this token for respective Chain ID values.

## Functions 

- `address`
  - Returns the on-chain address of this token respective to the passed Chain ID or [Network](../networks/Networks.md).
  - Params:
    - `network: Networks.Network | number` - This parameter can be either the Chain ID of the chain for which you want this token's on-chain address, or a `Network` instance.
  - Returns:
    - `string`
    - The on-chain address of this token respective to `network`, or `null` if the token is not supported by the Synapse Protocol on the given network. 