# Networks

`namespace Networks` provides constants and functions for working with blockchain networks supported by the SDK. 

## Import
The Networks namespace can be imported like so:

```typescript
import { Networks } from "@synapseprotocol/sdk";

// or, for potentially better tree shaking:
import { Networks } from "@synapseprotocol/sdk/common";
```

## Example Usage
```typescript
import { Networks, ChainId } from "@synapseprotocol/sdk";
import { BigNumberish } from "@ethersproject/bignumber";

// Print the name of a network respective to the passed chainId param.
function printNetworkNameFromChainId(chainId: number) {
  const n: Networks.Network = Networks.fromChainId(chainId);
  
  console.log(n.name);
}

printNetworkNameFromChainId(ChainId.POLYGON); // outputs "Polygon"

const nativeCurrency: string = Networks.AVALANCHE.chainSymbol;

console.log(`The native currency symbol for the Avalanche network is ${nativeCurrency}`); // nativeCurrency will output as "AVAX"
```

## The `Network` class

The `Network` class defined in `Networks` is primarily used as a "data wrapper", in that each instance of a `Network`
is useful for retrieving information about the defined network, such as its name, nativee currency symbol, and Chain ID. 

For 99.9% of use cases, SDK users should only ever need the provided, pre-defined `Network` instances.

A network contains the following read-only attributes:

- `name` (`string`) - The primary or official name of the network, for example "Binance Smart Chain"
- `names` (`string[]`) - "Unofficial" or shorthand names for the network
- `chainSymbol` (`string`) - The native currency symbol for this network. Examples: "BNB" on Binance Smart Chain, "AVAX" on Avalanche.
- `chainId` (`number`) - The network's Chain ID. For pre-defined `Network` instances, this will use a constant defined in the [ChainId](./ChainId.md) namespace.
- `tokens` ([`Token[]`](../tokens/Token.md)]) - An array containing [Token](../tokens/Token.md) instances for all tokens which the Synapse Protocol supports on this network.
- `tokenAddresses` (`string[]`) - An array of strings containing the actual, on-chain address of all entries in `tokens`.

The `Network` class also provides the following functions:

- `supportsToken`
  - Returns `true` if the `Token` passed to it is supported by the Synapse Protocol on the given network.
  - Params:
    - `token: Token` - A [Token](../tokens/Token.md) instance 
  - Returns: 
    - `boolean`
    - `true` if `token` is supported for use on the network by the Synapse Protocol, `false` otherwise
- `usesMetaBridgeZap`
  - Returns `true` if the Synapse Bridge Zap contract for this network is a `NerveMetaBridgeZap`, rather than a `NerveBridgeZap`. 
    Currently, Bridge Zaps for all networks **except** the Ethereum mainnet are `NerveMetaBridgeZap` contracts.

## Supported Networks

All of the below are `Network` instances, exported as constants from the `Networks` namespace. All of these networks 
are supported and useable on the Synapse Protocol.

- `ARBITRUM`
  - Constructor params:
    - `name:        "Arbitrum"`
    - `names:      ["arbi", "arb"]`
    - `chainId:      ChainId.ARBITRUM`
    - `chainSymbol: "ETH`

- `AVALANCHE`
  - Constructor params:
    - `name:        "Avalanche C-Chain"`
    - `names:      ["avalanche", "avax"]`
    - `chainId:      ChainId.AVALANCHE`
    - `chainSymbol: "AVAX"`

- `BSC`
  - Constructor params:
    - `name:        "Binance Smart Chain"`
    - `names:      ["smart chain", "bsc"]`
    - `chainId:      ChainId.BSC`
    - `chainSymbol: "BNB"`

- `ETH`
  - Constructor params:
    - `name:        "Ethereum Mainnet"`
    - `names:      ["eth", "mainnet"]`
    - `chainId:      ChainId.ETH`
    - `chainSymbol: "ETH"`

- `FANTOM`
  - Constructor params:
    - `name:        "Fantom"`
    - `names:      ["ftm"]`
    - `chainId:      ChainId.FANTOM`
    - `chainSymbol: "FTM"`

- `POLYGON`
  - Constructor params:
    - `name:        "Polygon"`
    - `names:      ["poly", "matic"]`
    - `chainId:      ChainId.POLYGON`
    - `chainSymbol: "MATIC"`

- `HARMONY`
  - Constructor params:
    - `name:        "Harmony"`
    - `chainId:      ChainId.HARMONY`
    - `chainSymbol: "ONE"`


## Exported functions

- `fromChainId`
  - Returns a `Network` instance based on the passed Chain ID, if it corresponds to a network which is known and supported by the Synapse Protocol.
  - Params:
    - `chainId: BigNumberish`
  - Returns:
  - `Network`
  - `Network` instance if it corresponds to a supported network, or null.
- `tokensForNetwork`
  - Returns the array of `Token`s which are supported by the Synapse Protocol on a given network, based on the passed Chain ID.
  - Params:
    - `chainId: BigNumberish`
  - Returns: 
    - `Token[]` 
    - Array of Tokens supported for use on the network respective to `chainId` (if `chainId` corresponds to a supported network), or null.
- `networkSupportsToken`
  - Returns true if the passed token is supported by the Synapse Protocol on the passed network.
  - Params:
    - `network: Network | BigNumberish` - This param can be either a Network instance, or the Chain ID of a Synapse Protocol-supported network.
    - `token: Token` - A [Token](../tokens/Token.md) instance to check network support for.
  - Returns: 
    - `boolean`
    - `true` if `token` is supported by the Synapse Protocol on `network`, `false` otherwise.
- `supportedNetworks`
  - Returns an array of `Network` instances corresponding to all networks supported by the Synapse Protocol.
  - Returns:
    - `Network[]`
    - Array which contains the pre-defined `Network` instances `ARBITRUM`, `AVALANCHE`, `BSC`, `ETH`, `FANTOM`, `POLYGON`, and `HARMONY`.