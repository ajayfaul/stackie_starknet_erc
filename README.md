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