# Anon Aadhaar

Anon Aadhaar is a protocol for proving ownership of an Aadhaar identity (Indian Residence ID) in a privacy-preserving manner. It lets users generate a ZK proof of their identity by only revealing the information they want to share (to an application).

Developers can use the AnonAadhaar SDK to integrate this into their applications to verify the identity of users without asking them to reveal more personal data than necessary. The proof generated can also be verified on EVM-based blockchains making Anon Aadhaar suitable for on-chain applications.

<br /> 


## How it works
Aadhaar data is signed by the government. Anon Aadhaar uses [zero-knowledge](https://en.wikipedia.org/wiki/Zero-knowledge_proof) circuits to verify this signature and generate proof of it. 

A "verifier" (an app or a smart contract) verifying the proof can be sure the "prover" had a valid Aadhaar containing the information (like age, state, gender) they revealed.

Anon Aadhaar works on the client side (browser, mobile app) and does not require the user to send their Aadhaar details to any server.

<br />

## Building and Running locally

Below steps are for building Anon Aadhaar circuits locally and generating proof with it.

For production, always use the published npm packages.

#### Requirements:

- Node JS (v18 or higher)
- Yarn

#### Install dependencies

```
yarn install
```

#### Build libraries

```sh
yarn build:libraries
```

#### Build circuit and generate zkey

```sh
# PWD = packages/circuits

yarn dev-install
yarn build-circuit
yarn dev-setup
```

This will generate the `build` folder with the compiled circuit and artifacts. The generated `zkey` is only meant for testing and should not be used in production.

⚠️ This will take a couple of minutes to finish.

#### Generate test data

```sh
# PWD = packages/circuits

yarn gen-test-data
```

This will generate dummy Aadhaar data and save it to `packages/circuits/assets/test.json`

The generated test data is verified using a test public/private key pair.

You can also use your real Aadhaar by setting env `REAL_DATA=true` and `QR_DATA=<QR_DATA>` (the large number in the Aadhaar QR code) in the `gen-proof` script in the next section.

#### Generate Witness and proof
```sh
# PWD = packages/circuits

yarn gen-witness
yarn gen-proof
```

This will generate and save the proof to `packages/circuits/build/proofs/proof.json` and the public signals to `packages/circuits/build/proofs/public.json`


#### Verify the proof
```sh
yarn verif-proof
```
This will verify the generated proof and print the result to the console.

#### Verify on-chain

You can also generate the solidity verifier contract using `yarn gen-contracts` and deploy it to a blockchain to verify the proof on-chain. You can use [this](https://github.com/anon-aadhaar/anon-aadhaar/blob/main/packages/core/src/utils.ts#L47) method to convert the generated proof to a format that can be used in the contract.


