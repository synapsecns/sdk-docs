# Synapse Protocol SDK Documentation

This repository contains documentation for the Synapse Protocol NodeJS SDK.

## Notes

At any given time, this documentation can -- and likely will -- expand. If a doc for something doesn't seem to be there,
hit us up via Discord or Telegram and let us know. We're likely already working on it and it'll be here soon, but 
it is very helpful to know what to prioritize. 

## Contents

- [Core](./core)
  - [Installation/General usage of the NPM module](./core/installation)
  - [Entities](./core/entities)
    - [General usage](./core/entities/synapse-entities.md)
    - Individual Contracts
      - [Synapse Bridge](./core/entities/SynapseBridge.md)
      - [NerveBridgeZap](./core/entities/NerveBridgeZap.md)
      - [NerveMetaBridgeZap](./core/entities/NerveMetaBridgeZap.md)
      - [Synapse ERC20](./core/entities/SynapseERC20.md)
  - [Networks](./core/networks)
    - [ChainId namespace](./core/networks/ChainId.md)
    - [Networks namespace](./core/networks/Networks.md)
  - [Tokens](./core/tokens)
    - [General usage, as well as pre-defined/supported tokens](./core/tokens/general.md)
    - [Token class](./core/tokens/Token.md)
    - [Tokens namespace](./core/tokens/Tokens.md)
    - [SwapType namespace](./core/tokens/SwapType.md)
  - [General usage of the SDK](./core/core.md)
- [Bridge](./bridge)