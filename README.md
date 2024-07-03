# Chainlink VRF Automation

## Introduction
This project demonstrates how to use Chainlink's Verifiable Random Function (VRF) and Automation features. Chainlink VRF provides a secure source of on-chain randomness, and Chainlink Automation allows for the creation of automated, decentralized workflows. Together, these features can be used to build robust and reliable decentralized applications (dApps).

## Overview
There are two contracts. We start with `FortuneTeller.sol` which uses `Chainlink VRF`, and then move to `FortuneSeeker.sol` which uses `Chainlink Automation`.

As a prerequisite, we need to have a token for the blockchain test network to use. This project is configured to be deployed and run on the `Avalanche Fuji Network`, but we can deploy the code on any EVM network supported by Chainlink.

### `FortuneTeller.sol`
This contract implements functions that seek a cryptographically verifiable (i.e., provably random) number from the `Chainlink Oracle Network VRF (Verifiable Random Function)` service.

Once that random number is stored in the contract's storage, we can call `seekFortune()` which uses that random number to generate a random fortune from a stored array of fortunes.

The contract is designed such that it can be invoked by another smart contract and uses an interface to callback that contract with the fortune. When being called by the client contract, it expects to get paid before it will tell a fortune.

You will need to `register a VRF subscription` and fund it with LINK tokens to compensate the `Chainlink Decentralized Oracles` for the computation work required to generate and submit a cryptographically provable random number.

### `FortuneSeeker.sol`
This contract is a "client" to `FortuneTeller`. It calls `FortuneTeller` and pays it some `ETH`. Therefore, this contract must store a balance.

This contract also implements the interface required to be automatically callable by `Chainlink's Automation` service - a decentralized oracle network that automates the execution of specified functions in your smart contract. The contract to be automated is called an "Upkeep", and you must register and fund your Upkeep here.

You can have a time-based (cron-like) automation schedule or you can have custom logic that tells the Chainlink Decentralized Network whether or not your contract needs to have its target function invoked. We use the custom logic approach in this project.

As long as FortuneTeller is invoked to generate new random numbers, FortuneSeeker will receive a different (except by coincidence) fortune from FortuneTeller. The frequency depends on the interval that you tell the Chainlink Automation Network you want your contract invoked and will continue for as long as FortuneSeeker can pay FortuneTeller and your Chainlink Automation Upkeep registration has enough LINK balance.

## Features
* Secure Randomness: Use Chainlink VRF to generate verifiable, tamper-proof random numbers on the blockchain.
* Automated Workflows: Utilize Chainlink Automation to create automated, decentralized workflows without needing constant manual intervention.

## Getting started
* Install the NPM packages
* Fill in the Environment Variables needed in hardhat.config.js to connect your wallet, and other API keys.
* Run yarn hardhat test to run the tests.
* Run yarn hardhat to see the Hardhat Tasks available. Two custom tasks: deploy-teller and deploy-seeker have been included in hardhat.config.js

### Tooling used
* Hardhat
* JavaScript/ NodeJs
* Metamask Browser Wallet
* Avalanche Fuji Network
* Chainlink Decentralized Oracle Services

