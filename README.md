# Deploying your first Cairo 1 contract to Starknet

[Version 0.11 of Starknet](https://starkware.medium.com/starknet-alpha-v0-11-0-the-transition-to-cairo-1-0-begins-30442d494515) has been released with support for Cairo 1 contracts. This is fantastic news!

If you're wondering how to deploy your first Cairo 1 contracts on Starknet, here's a rough guide to get you started. Please note that this guide is not comprehensive and is subject to change as we refine our documentation.

## Before You Begin

Ensure that you have Rust installed on your machine. If you do not have Rust installed, you can follow the [Rust installation documentation](https://www.rust-lang.org/tools/install).

## The Flow

Here's what you'll learn and do in this guide:

- Clone the Cairo repository at a specific height. This repository is the tool used to compile your Cairo 1 contract.
- Install the latest version of Cairo-lang, which is the tool used to interact with Starknet.
- Use Cairo to compile your contract from Cairo to Sierra.
- Declare your Sierra contract using Cairo-lang.
- Deploy your contract using Cairo-lang.
- Brag to your friends on Twitter!

## Step-by-step guide

### 1. Clone this repository

To clone this repository, run the following command:

```solidity
git clone https://github.com/starknet-edu/deploy-cairo1-demo
```

Then, navigate to the directory using:

```solidity
cd deploy-cairo1-demo
```

### 2. Instal the Cairo 1 compiler

To install Cairo for the first time:

```
git clone https://github.com/starkware-libs/cairo/
cd cairo
git checkout 9c190561ce1e8323665857f1a77082925c817b4c
cargo build --all --release
```

If you already have Cairo installed:

```
cd cairo
git fetch && git pull
git checkout 9c190561ce1e8323665857f1a77082925c817b4c
cargo build --all --release
```

At this point, Cairo will be installed in this repository.

### Step 3: Install Cairo-lang

1. Go back to the root folder of the repository.
    
    ```
    cd ..
    ```
    
2. Set up a Python virtual environment.
    
    ```
    python3.9 -m venv ~/cairo_venv_v11
    source ~/cairo_venv_v11/bin/activate
    ```
    
3. If you had Cairo-lang installed previously, uninstall it.
    
    ```
    pip3 uninstall cairo-lang
    ```
    
4. Install Cairo-lang.
    
    ```
    pip3 install ecdsa fastecdsa sympy
    pip3 install cairo-lang
    ```
    
5. Check that you have it installed correctly.
    
    ```
    starknet --version
    ```
    

You should see the version `starknet 0.11.0.1` or later.

### Step 4: Compile your contract using Cairo

To complete this test, you can use [this small demo contract](https://github.com/starknet-edu/deploy-cairo1-demo/blob/master/hello_starknet.cairo), which is a basic event logger.

To compile your contract from Cairo to Sierra, follow these steps:

1. In your terminal, navigate to the folder where Cairo is installed.
2. Use the following command to compile your contract:

```
cd cairo
cargo run --bin starknet-compile -- ../hello_starknet.cairo ../hello_starknet.json --replace-ids
```

Congratulations, you have successfully compiled your contracts using Cairo!

### 5. Declare your contract using Cairo-lang

**5.1. Set up environment variables**

To set environment variables, use the following commands:

```
export STARKNET_NETWORK=alpha-goerli
export STARKNET_WALLET=starkware.starknet.wallets.open_zeppelin.OpenZeppelinAccount
```

**5.2. Set up a new account for the tutorial**

To ensure that your StarkNet CLI is set up with a proper account contract and funds, you need to create a new one just for this tutorial.

- Run the following command to create a new account with version_11:

```solidity
starknet new_account --account version_11
```

- You should receive your account address in your terminal as a return.
- Send 0.1 ETH to it using your Argent or Braavos wallet.
- Monitor the transfer transaction status. Once it has passed the "pending" status, proceed to deploy your account by invoking the following command:

```solidity
starknet deploy_account --account version_11
```

Monitor the transaction status of the deployment. Once it has moved past the "pending" status, you can proceed with declaring your contract.

**5.3. Declare the contract**

To declare the contract, go back to the root folder of the tutorial and execute the `starknet declare` command:

```
cd ..
starknet declare --contract hello_starknet.json --account version_11
```

After executing the command, you should receive the hash of the newly declared contract.

**Troubleshooting**

When using the `starknet declare` command, you may encounter the following error:

```
Error: OSError: [Errno 8] Exec format error: '~/cairo_venv_11/lib/python3.9/site-packages/starkware/starknet/compiler/v1/bin/starknet-sierra-compile'
```

This error occurs because cairo-lang needs to compile your Sierra code, and it uses an imported Rust binary called *starknet-sierra-compile* for that purpose. However, sometimes the imported binary is not built for your architecture.

Take the *starknet-sierra-compile* from the *cairo* repository that you built locally and replace it in the Python package. Here is the command:

```solidity
cp cairo/target/release/starknet-sierra-compile ~/cairo_venv_v11/lib/python3.9/site-packages/starkware/starknet/compiler/v1/bin/starknet-sierra-compile
```

After that, try declaring again. It should work!

### 6. Deploy your contract using Cairo-lang

At this point, you are back in the flow that you are used to. Use the class hash you received earlier to deploy the contract (step 5.3.):

`starknet deploy --class_hash CLASS_HASH --account version_11`

Monitor your transaction. If it fails due to fee estimation, retry. Once your transaction is `accepted_on_l2`... Congratulations! You’ve successfully deployed your first Cairo 1 contract!

### 7. Brag on Twitter!

Don't forget to share your achievements! Post a tweet about your deployed contract [here](https://twitter.com/henrlihenrli/status/1638468939939282945).

Also, consider adding cool functionalities to your contract. The Starknet edu team will soon release the Cairo 1 version of [Cairo-101](https://github.com/starknet-edu/starknet-cairo-101). In the meantime, you can get ideas on what to do with Cairo 1 from [Starklings-cairo1](https://github.com/shramee/starklings-cairo1).

## Hey, want to learn more?

### Join our next Basecamp!

If you're interested in:

- Developing cool stuff on Starknet,
- Understanding how Starknet works,
- Learning about Cairo,
- Understanding how Stark proofs work,

you should join Basecamp, our free 6-week training program for aspiring Starknet developers. The next two cohorts start in April. You can register [here](https://forms.reform.app/starkware/starknet-basecamp-registration-starting-jan-4th-copy-2/itvk4e) for an English cohort, and [aqui](https://forms.reform.app/starkware/starknet-basecamp-registration-starting-apr-11th-copy/itvk4e) para una cohorte en Español.

### Play with Cairo 1 code

If you are interested in:

- Learning how to read Cairo 1 code,
- Reading Starknet smart contracts, and
- Interacting with them using a browser wallet and a block explorer,

you should try the [Starknet Cairo 101 Automated Workshop](https://github.com/starknet-edu/starknet-cairo-101/tree/cairo1)!
