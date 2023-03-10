# BAL Interest Rate Curve Upgrade

This repository contains the payload to update the AAVE V2 Mainnet BAL Liquidity Pool interest rate strategy, AAVE V2 Polygon BAL Liquidity Pool interest rate strategy and the AAVE V3 Polygon BAL Liquidity Pool interest rate strategy as well as the borrow cap on this pool.

## Specification

The code is split in two proposals, one for Mainnet and one for Polygon. The Proposal Payload does the following:

1. Sets the new interest rate strategy address.

The new interest rate strategy is deployed here: https://etherscan.io/address/0x04c28D6fE897859153eA753f986cc249Bf064f71

The Proposal Payload for Polygon does the following:

1. Sets the new interest rate strategy address for v2.

The new interest rate strategy is deployed here: https://etherscan.io/address/0x80cb7e9E015C5331bF34e06de62443d070FD6654

2. Sets the new interest rate strategy address for v3.

The new interest rate strategy is deployed here: https://etherscan.io/address/0x4b8D3277d49E114C8F2D6E0B2eD310e29226fe16

3. Sets the borrow cap for v3 at: 256,140. The supply cap does not change.

The interest rate changes are as follows:

```
==========================================
| Parameter	   Current (%)	Proposed (%)
==========================================
| Uoptimal	  |   45       |  80     |
------------------------------------------
| Base	          |   0        |  3      |
------------------------------------------
| VARIABLE RATES                         |
------------------------------------------
| Slope1	  |   7.0      |   14.0  |
------------------------------------------
| Slope2	  |   300      |   150   |
------------------------------------------
```

\*\* Please note the BAL pools do not offer stable rate borrowing and those rates have not been updated from their previous strategies.

The function used to set the strategy comes from the `@aave-address-book` library

```
  /**
   * @dev Sets the interest rate strategy of a reserve
   * @param asset The address of the underlying asset of the reserve
   * @param rateStrategyAddress The new address of the interest strategy contract
   **/
  function setReserveInterestRateStrategyAddress(
    address asset,
    address rateStrategyAddress
  ) external;
```

2. Sets the new borrow cap for the liquidity pool:

The function used to set the borrow cap comes from the `@aave-address-book` library

```
/**
   * @notice Updates the borrow cap of a reserve.
   * @param asset The address of the underlying asset of the reserve
   * @param newBorrowCap The new borrow cap of the reserve
   **/
  function setBorrowCap(address asset, uint256 newBorrowCap) external;
```

## Installation

It requires [Foundry](https://github.com/gakonst/foundry) installed to run. You can find instructions here [Foundry installation](https://github.com/gakonst/foundry#installation).

### GitHub template

It's easiest to start a new project by clicking the ["Use this template"](https://github.com/llama-community/aave-governance-forge-template).

Then clone the templated repository locally and `cd` into it and run the following commands:

```sh
$ npm install
$ forge install
$ forge update
$ git submodule update --init --recursive
```

### Manual installation

If you want to create your project manually, run the following commands:

```sh
$ forge init --template https://github.com/llama-community/aave-governance-forge-template <my-repo>
$ cd <my-repo>
$ npm install
$ forge install
$ forge update
$ git submodule update --init --recursive
```

## Setup

Duplicate `.env.example` and rename to `.env`:

- Add a valid mainnet URL for an Ethereum JSON-RPC client for the `RPC_MAINNET_URL` variable.
- Add a valid Private Key for the `PRIVATE_KEY` variable.
- Add a valid Etherscan API Key for the `ETHERSCAN_API_KEY` variable.

### Commands

- `make build` - build the project
- `make test [optional](V={1,2,3,4,5})` - run tests (with different debug levels if provided)
- `make match MATCH=<TEST_FUNCTION_NAME> [optional](V=<{1,2,3,4,5}>)` - run matched tests (with different debug levels if provided)

### Deploy and Verify

- `make deploy-payload` - deploy and verify payload on mainnet
- `make deploy-proposal`- deploy proposal on mainnet

To confirm the deploy was successful, re-run your test suite but use the newly created contract address.
