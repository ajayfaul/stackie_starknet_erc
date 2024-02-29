# Stackie Starknet ERC

This source code are tools to deploying smart contract to Starknet blockchain network. this code mainly using cairo language *Cairo* is A STARK-based Turing-complete *language* for writing provable programs on blockchain. *Cairo* enables developers to use proof technology.

### Find more about STARK and Cairo

[Starknet | Secure scaling technology bringing Ethereum’s benefits worldwide  - Starknet](https://www.starknet.io/en)

[Cairo - a language for writing provable programs](https://www.cairo-lang.org/)

---

### Environtment Setup

For Windows users, you are advised to use a cloud development environment like [Google Cloud Shell](https://shell.cloud.google.com/). Ensure you are logged in to your Google account before you click on the link. You should then see a familiar interface, similar to VS Code. Click on the Explorer icon to open a folder to the default directory. If you prefer to do this locally, you should install [Windows Subsystem for Linux](https://learn.microsoft.com/en-us/windows/wsl/install).

To get started with Cairo, first, we have to install a package manager called **Scarb**. Scarb is the project management tool for the Cairo language. Scarb manages dependencies and compiles projects. Run the following command in your terminal, which will install a specific release of Scarb.

```other
curl --proto '=https' --tlsv1.2 -sSf https://docs.swmansion.com/scarb/install.sh | sh -s -- -v 2.3.1
```

Then, to verify that you have successfully installed it, run the following command in a new terminal window.

```other
scarb --version
```

You will get an output that looks like this:

```other
scarb 2.3.1 (0c8def3aa 2023-10-31)
cairo: 2.3.1 (https://crates.io/crates/cairo-lang-compiler/2.3.1)
sierra: 1.3.0
```

Sierra is an intermediary representation layer that will offer greater long-term stability for Cairo programs.

To create your first Cairo program, run on any directory of your choice with the following commands.

```other
scarb new try_stark
cd try_stark
```

We just created a new folder called quest_one using `scarb new try_stark`. Then we changed our current working directory to **try_stark**.

The folder contains a **src** folder which houses all your project files. **Scarb.toml** manages dependencies that you can use in your program.

**Installing Starkli**

Starkli is a Starknet CLI used for declaring, deploying and interacting with deployed contracts. It is also used to get Starknet data such as block information, transaction details and more.

First, we want to install **Starkliup**, the installer for the Starkli environment. Enter the following command in your terminal.

```other
curl https://get.starkli.sh | sh
```

Following the output of the installation, open a new terminal and enter the following command to run the installer. ⚠️ If you receive the error message `starkliup: command not found`, do ensure you followed the instructions thus far.

```other
starkliup
```

You can then verify the successful installation using the following command.

```other
starkli --version
```

---

### Setting up Account for Starknet

To create an account to be used with Starknet, we will install the Argent X wallet extension [here](https://chromewebstore.google.com/detail/argent-x/dlcobpjiigpikoobohmabehhmhfoodbb). Argent X is a popular Starknet wallet and is fully non-custodial, which means that you have control over your tokens. Once the extension is installed, a new tab will be opened. Follow the instructions to create a new wallet.

At the top right-hand corner, click on Mainnet and change it to **Testnet or Goerli**. Create an account that is a standard account. At this point, notice that our wallet has no funds. As such, let's fund our account with ETH.

Click on your address to copy it.

![image.png](https://res.craft.do/user/full/55fbf0f6-b802-cfae-f9ae-874a9e8e032d/doc/7d274b90-7216-40bf-a4e6-87ccd0f194ee/ae8fa409-e257-4e93-8481-67c8091e45b5)

Then, head to the [Starknet Goerli faucet](https://faucet.goerli.starknet.io/) and request ETH.

Once the request is completed, you can check your Argent X extension and you should now see 0.002 ETH in your Account 1.

On Argent X, **making an outgoing transaction from the account will automatically deploy your account on-chain**. A simple way to achieve this is to send a few tokens to another account. On Argent X, at the top left corner, click on the account dropdown and the plus icon. Select the standard account option and Account 2 is created. In Account 1, click on Send. Input Account 2 as the recipient, and send over 0.001 ETH. You will notice that the gas fees indicate that it is inclusive of a one-time activation fee (the fee for deploying the account).

---

### Guide to Deploying a Smart Contract on Testnet

In this step, we will be deploying an ERC-20 token on Starknet. Do ensure that you already have your developer environment set up and Argent X accounts created.

Use the same cloud development environment like [Google Cloud Shell](https://shell.cloud.google.com/) or local code editor, where you had already installed Scarb and Starkli.

First, let’s set up the project folder by running the following commands in your terminal or clone this repo.

```other
scarb new stackie_starknet_erc
cd stackie_starknet_erc
```

Then open the newly created stackie_erc folder in your code editor. Next, copy the code [here](https://github.com/ajayfaul/stackie_starknet_erc/blob/53b23cc02130809fcd2e477399135af701189d17/src/lib.cairo) and replace the content of **src/lib.cairo**

The code that you have just pasted does the following:

- The contract interface defines a set of functions typical for an ERC-20 token
   - get_name, get_symbol, and get_decimals for token metadata
   - get_total_supply to return the total token supply
   - balance_of to get the balance of a specific account
   - allowance, transfer, transfer_from, approve, increase_allowance, and decrease_allowance for managing and executing token transfers and approvals
   - mint and burn functions to increase or decrease the token supply, respectively, under certain conditions
- Contract Implementation BWCERC20Token
   - Storage: Defines the contract's storage variables, including token name, symbol, decimals, total supply, and mappings for balances and allowances.
   - Events: Declares Transfer and Approval events for logging token transfers and approvals.
   - Constructor: Initializes the contract with a name, symbol, decimals, initial supply, and assigns the initial supply to a recipient address.
   - Token Operations: Implements the ERC-20 interface with functions to read token information, perform transfers, approvals, minting, and burning. It includes checks for zero addresses, sufficient balances, and allowance limits.
   - Helper Functions: Provides internal utility functions for executing transfers and approvals, including transfer_helper, approve_helper, and spend_allowance. These include additional checks and emit the corresponding events.
- Specific features and checks
   - Ownership and Authorization: Certain operations like mint and burn are restricted to the contract's owner, verified through checks against the caller's address.
   - Allowance Handling: Implements a unique mechanism for handling allowances, including a method to optionally treat a special "max" value as an unlimited allowance, saving on gas costs by avoiding the need to update allowances for each transaction.
   - Zero Address Checks: Enforces checks against transferring to or approving zero addresses, similar to safety checks found in Solidity contracts.
   - Event Emission: Emits events for Transfer and Approval actions, facilitating external tracking of these operations.

Next, we need to update **Scarb.toml** to inform the compiler we are dealing with a smart contract. Copy the code below and paste it under dependencies.

```other
[package]
name = "stackie_erc"
version = "0.1.0"
edition = "2023_11"

# See more keys and their definitions at https://docs.swmansion.com/scarb/docs/reference/manifest.html

[dependencies]
starknet = "2.3.1"

[[target.starknet-contract]]
sierra = true
```

Next, we will build our contract by entering the following command in our terminal.

```other
scarb build
```

A new folder called **target** will be generated automatically. Expand the target folder and you will notice there is another folder called **dev**. Proceed to expand the dev folder which contains two json files, one is the **contract class** and the other is the artifacts. Take note of the contract class file as we will need it later.

To create our smart wallet, we need an encrypted keystore to hold our account credentials safely. The encrypted values in the keystore are what we need to create the signer. We need a private key for this which can be obtained from **Argent X**.

Open your Argent X wallet browser extension. Click on the settings icon at the top right hand corner. Click on the account banner and then click on **export private key**. Unlock and copy the private key.

Next, in your terminal, run the following command. ⚠️ If you already have a stackie_keystore.json in your folder, do delete it before running the command.

```other
starkli signer keystore from-key stackie_keystore.json
```

You will be prompted to paste your private key. Do note that the private key you paste in your terminal will not be visible. ⚠️This means that it is important that you only paste your key once, and then hit the enter key. You will be required to enter a password of your choosing. Similarly, it will not be visible so hit enter when you are done.

You will get an output like the one below:

```other
Created new encrypted keystore file: 
/home/stackie/stackie_starknet_erc/stackie_keystore.json
Public key: 0x06385f55ee31418f7cb52a656c57953cea9a7bb3bab6403258e7be091c25fc1b
```

In the stackie_erc folder, create a new file called stackie_account.json. Then, add the following code.

```other
{
    "version": 1,
    "variant": {
      "type": "argent",
      "version": 1,
      "owner": "<OWNER>",
      "guardian": "0x0"
    },
    "deployment": {
      "status": "deployed",
      "class_hash": "<CLASSHASH>",
      "address": "<ADDRESS>"
    }
  }
```

There are three items you will need to replace. For **** (line 6), paste the value of the public key that was created with stackie_keystore.json.

Open your Argent X wallet. Click on the settings icon on the top right hand corner. Then, click on your account banner. Click on '**View on Starkscan**". A contract page on Starkscan will be opened on your browser.

For CLASSHASH (line 11), copy the value of Class Hash from the page.

For ADDRESS (line 12), copy the value of Contract Address from the page.

We can proceed with the rest of the contract deployment. First we need to declare the contract so that we can obtain the class hash of our contract. Run the command below in your terminal to declare the contract.

```other
starkli declare target/dev/stackie_erc_BWCERC20Token.contract_class.json --rpc https://starknet-testnet.public.blastapi.io/rpc/v0_6  --account stackie_account.json --keystore stackie_keystore.json
```

Note: **target/dev/stackie_erc_BWCERC20Token.contract_class.json** is the relative path of the contract class json file you were told to take note of when we ran `scarb build` earlier.

You will be requested to enter the password that you set when creating your keystore file earlier. You will get an output like the one below.

```other
Not declaring class as it's already declared. Class hash:
0x02d132d59e419063e9180b27a4827c327bee16f5c845ca7410b066795a5086be
```

Do take note of the **class hash declared** as we will need it.

Next, let's proceed to deploy the smart contract by running the command below. Be sure to replace **** with the class hash declared value from earlier. Also, replace **** with the wallet address obtained from Argent X.

```other
starkli deploy <CLASS HASH DECLARED> --rpc https://starknet-testnet.public.blastapi.io/rpc/v0_6 --account stackie_account.json --keystore stackie_keystore.json <YOUR WALLET ADDRESS> 35731471428046310164841589102 1398033219 8 100000 0 <YOUR WALLET ADDRESS>
```

Your command would look similar to this.

![q2s9_2.jpg](https://res.craft.do/user/full/55fbf0f6-b802-cfae-f9ae-874a9e8e032d/doc/7d274b90-7216-40bf-a4e6-87ccd0f194ee/9540c3a2-0b48-4803-b183-654869ad6409)

**Note**: we added the constructor argument (arguments can be converted to the right data types using [https://www.stark-utils.xyz/converter](https://www.stark-utils.xyz/converter)) to the deploy command above.

_owner =>

- _name => 35731471428046310164841589102
- _symbol => 1398033219
- _decimal => 8
- _initial_supply => 100000 0

Recipient =>

You should then receive an output like this.

![q2s9_3.jpg](https://res.craft.do/user/full/55fbf0f6-b802-cfae-f9ae-874a9e8e032d/doc/7d274b90-7216-40bf-a4e6-87ccd0f194ee/167e7ed6-361c-40e0-9d58-ac7d6eb58eb4)

In your terminal, copy the value of **contract deployment transaction**. Then, head to [https://testnet.starkscan.co/](https://testnet.starkscan.co/) and search for the transaction.

When you scroll down to the input data, you should see that have deployed an ERC-20 token called stackie coin, with the symbol STKC.

![q2s9_4.jpg](https://res.craft.do/user/full/55fbf0f6-b802-cfae-f9ae-874a9e8e032d/doc/7d274b90-7216-40bf-a4e6-87ccd0f194ee/ca5187f8-1c98-498c-a092-66fc7f5a265a)

