# Deploying your first Cairo 1 contract to Starknet
With [version 0.11](https://starkware.medium.com/starknet-alpha-v0-11-0-the-transition-to-cairo-1-0-begins-30442d494515) rolling out on Starknet, you can now deploy your first Cairo 1 contracts on Starknet! Fantastic!

... But how ser?

Here is a very rough guide, before we get our doc in order

## The flow
You will need to:
- Clone the Cairo repository (the tool to compile your Cairo 1 contract) at a specific height
- Install the latest version of Cairo-lang (the tool to interact with Starknet)
- Compile your contract from Cairo to Sierra, using cairo 
- Declare your Sierra contract, using cairo-lang
- Deploy your contract, using cairo-lang
- Brag to your friends, using Twitter

## Clone this repo
```bash
git clone https://github.com/starknet-edu/deploy-cairo1-demo
cd deploy-cairo1-demo
```

## Installing the Cairo one compiler
If you're installing Cairo for the first time:
```bash
git clone https://github.com/starkware-libs/cairo/
cd cairo
git checkout 9c190561ce1e8323665857f1a77082925c817b4c
cargo build --all --release
```

If you already have Cairo installed:
```bash
cd cairo
git fetch && git pull
git checkout 9c190561ce1e8323665857f1a77082925c817b4c
cargo build --all --release
```

At this point you have Cairo installed in this repository. 

## Installing Cairo-lang
Go back to the root folder of the repo
```bash
cd ..
```
Set up a python virtual environment
```bash
python3.9 -m venv ~/cairo_venv_v11
source ~/cairo_venv_v11/bin/activate
```
If you had cairo-lang installed previously, uninstall it
```bash
pip3 uninstall cairo-lang
```
Download the latest release of Cairo-lang [here](https://github.com/starkware-libs/cairo-lang/releases/tag/v0.11.0.1) in the root folder of this repo. Install it
```bash
pip3 install cairo-lang-0.11.0.1.zip
```
Check that you have it installed correctly
```bash
starknet --version
```

## Compile your contract using Cairo
You can use [this smol demo contract](hello_starknet.cairo) to complete this test. It's a very basic event logger.

From your terminal, in the folder you installed Cairo in
```bash
cd cairo
cargo run --bin starknet-compile -- ../hello_starknet.cairo ../hello_starknet.sierra --replace-ids	
```
Congratulations, you have compiled your contracts from Cairo to Sierra!

## Declare your contract using Cairo-lang
### Setting environment variables
```bash
export STARKNET_NETWORK=alpha-goerli
export STARKNET_WALLET=starkware.starknet.wallets.open_zeppelin.OpenZeppelinAccount
```

### Setting a new account for the tutorial

You need to make sure your starknet CLI is set up with a proper account contract, and funds. For safety, I'll create a new one just for this tutorial.
```bash
starknet new_account --account version_11_test
```
- You should get your expected contract address
- Send 0.1 ETH to it
- Monitor the transfer transaction. Once it has passed "pending", proceed
```bash
starknet deploy_account --account version_11
```
Monitor the deploy transaction. Once it has passed "pending", proceed

### Declaring the contract
Go back to the root folder of the tutorial then try to declare
```bash
cd ..
starknet declare --contract integration/hello_henri.sierra --account version_11
```
You should receive your newly declared class hash!

### Troubleshooting
Ok so here I had a problem. Using declare sent back:
```bash
Error: OSError: [Errno 8] Exec format error: '/Users/henrilieutaud/cairo_venv_11/lib/python3.9/site-packages/starkware/starknet/compiler/v1/bin/starknet-sierra-compile'
```
The reason is that cairo-lang needs to compile your sierra code, and it uses an imported rust binary 'starknet-sierra-compile' for that. But in my case, the imported binary was not built for my achitecture :-(.

So, simple: I took the 'starknet-sierra-compile' from the cairo repo, that I built locally, and replaced it in the python package. Here is a command that should work:

```bash
cp cairo/target/release/starknet-sierra-compile ~/cairo_venv_v11/lib/python3.9/site-packages/starkware/starknet/compiler/v1/bin/starknet-sierra-compile
```

Then try again declaring. It should work!

## Deploy your contract using Cairo-lang
At this point you are back in the flow you are used to. Using the class hash you received earlier:
```bash
starknet deploy --class_hash <class_hash> --account version_11
```
Monitor your transaction. If it fails because of fee estimation, retry. Once your transaction is accepted_on_l2.... Congratulations! You've deployed your first Cairo 1 contract!

## Bragging on Twitter
Don't forget to brag! And don't forget to add cool functionnalities to your contract.

The Starknet edu team will release soon the Cairo 1 version of [Cairo-101](https://github.com/starknet-edu/starknet-cairo-101). In the meanwhile, you can get ideas of what to do with Cairo 1 with [Starklings-cairo1](https://github.com/shramee/starklings-cairo1)

## Do you want to learn more?
If you are interested in 
- Learning how to develop cool stuff on Starknet
- How Starknet works
- What Cairo is
- How Stark proofs work
You should join Basecamp, our free 6 weeks training program for aspiring Starknet devs. The next two cohorts start in April and you register [here](https://forms.reform.app/starkware/starknet-basecamp-registration-starting-jan-4th-copy-2/itvk4e) for a cohort in English, and [aqui](https://forms.reform.app/starkware/starknet-basecamp-registration-starting-apr-11th-copy/itvk4e) para una cohort en Español.