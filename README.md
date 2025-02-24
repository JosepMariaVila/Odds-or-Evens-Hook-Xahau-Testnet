<p align="center">
  
</p>

# Odds or Evens, a game Hook 

## Introduction

Odds or Evens is a hook programmed in C for the Xahau blockchain. A hook is a series of rules that enable smart logic in Xahau, considered the smart contracts of Xahau.

Odds or Evens is a Hook that converts a Xahau account in an engine for a simple game. One player will hava an Odd or and Even number based on the sequence ledger of the payload he sent. Next player will receive an Odd or an Even number based on the sequence ledger of the payload he sent. 

If the first player has an Odd number and the second player an Odd number, the first player wins. 

If the first player has an Odd number and the second player an Even number, the second player wins. 

If the first player has an Even number and the second player an Even number, the first player wins. 

If the first player has an Even number and the second player an Odd number, the second player wins. 

These rules can be sumarized like this:
Compare the numbers of both players regarding being odd or even, if they are equal, first player wins, if they are different, second player wins.

## Explanation

**Attention:** Only use if you are sure of what you are doing on Testnet/Mainnet. You could put your funds at risk. It is recommended to install on new accounts.

The hook when installed allows you to play the game of the largest number. The hook will accept two players sending 1 XAH. With each payment the hook will check the sequence of the current ledger. The hook will store the rightmost number of the ledger sequence and store it in the namespace next to the address of the first player referring to the first payment he receives. When a second player sends a payment of 1 XAH to the hook address, the hook will check the sequence and compare the rightmost number with that of the first player. This can lead to 3 possible outcomes. Player 1 has the largest number and receives the 2 XAH. Player 2 has the largest number and receives the 2 XAH. There is a tie and each player receives 1 XAH. 

The hook blocks any payment other than 1 XAH. So a fourth case could occur, that the hook account runs out of funds and the game cannot be managed. If there are insufficient funds it might not be possible to send the “prize” to the winners. Therefore, it has been enabled to manage an account known as funding “FUND” that the hook allows to operate payments in both directions to be able to take out or put in XAH and avoid the mentioned problem. To assign a “FUND” account it is necessary to create an Invoke transaction from the Hook account with the parameter “FUND” and the account that we want to assign as a “FUND” account in the parameter value. The process is explained below.

## Installation & Usage

Once the hook is installed, the following triggers are expected for the hook.

- An account will send a payment of 1 XAH to the hook account. The payment account will be known as Player 1. The hook will register in the namespace the address of Player one with the key P1AD and store the rightmost number of the ledger sequence in the namespace under the key P1LG.

- A second account (different from the first one) will send a payment of 1 XAH to the hook account. The payment account will be known as Player 2. The hook will compare the rightmost number of the ledger sequence of the ledger sequence with that of Player 1 and decide the final result of the game.

- An Invoke transaction from the hook account with the hook parameter “FUND” and the desired address as value in HEX format. This will store in the namespace the information with the key “FUND” and with value the address in HEX.


## How to install the Highest Number Hook on Testnet?

HookHash: E0C65E4A905F6A0835C3753467176F2BE031C239A7E0F82B44A99C8A4F119028

1. You can do it by [XRPLWin Hook Install Tool](https://xahau-testnet.xrplwin.com/tools/hook/from-hash)

2. Or you can do it sending the transaction below:

HookOn is activated to trigger for Invoke and Payment transactions. You can verify it copying the HookOn value (FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF7FFFFFFFFFFFFFFFFFFBFFFFE) in this website: https://richardah.github.io/xrpl-hookon-calculator/

    const prepared = {
      "TransactionType": "SetHook",
      "Account": your_account_address,
      "Flags": 0,
      "Hooks": [
        {
          "Hook": {
            "HookHash": "E0C65E4A905F6A0835C3753467176F2BE031C239A7E0F82B44A99C8A4F119028",
            "HookNamespace": "0000000000000000000000000000000000000000000000000000000000000000",
            "HookOn": "FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF7FFFFFFFFFFFFFFFFFFBFFFFE",
          }
        }
      ],
      ...networkInfo.txValues,
    };

## How to uninstall the Highest Number Hook on Mainnet/Testnet?

    const prepared = {
      "TransactionType": "SetHook",
      "Account": your_account_address,
      "Flags": 0,
      Hooks:
    [        
        {                        
            Hook: {
                CreateCode: "",
                Flags: 1,
            }
        }
    ],
      ...networkInfo.txValues,
    };

## Transaction Examples for Hook Parameters

## Adding the FUND account

We create a Invoke transaction with our Hook Account as "Account" and NO Destination Account. 

Hook Parameters and values will be:
- FUND
- funding_address

In this example we are using 1E2D42546C8A5270D4E182FAE3D12186F2A32A7E that is the translated version of the address rskZVQvBEXAwsBTFsgEZMBfwnhC7oydSnp. You can visit https://transia-rnd.github.io/xrpl-hex-visualizer/ , Insert the account and click on From Hex, you will see the xrpAddress will be the same as we added. FUND string is HEX translated: 46554E44. (For https://builder.xahau.network/ IDE you don't need to translate Parameter Name)

    const prepared = {
      TransactionType: "Invoke",
      Account: your_account_address,
      Flags: 0,
      HookParameters: [
        {
          HookParameter: {
            HookParameterName: "46554E44",
            HookParameterValue: "1E2D42546C8A5270D4E182FAE3D12186F2A32A7E",
          },
        },
        ],
      ...networkInfo.txValues,
    };



## How to install the Highest Number Hook on Mainnet?

Same as Testnet but changing the hookhash. The Hookhash is 0FFB97070F3B6D4DFC5C088935A64DCEA4C20EC1EB40BABB26A5716A2A420350.

1. You can do it by [XRPLWin Hook Install Tool](https://xahau.xrplwin.com/tools/hook/from-hash)

2. Or you can do it sending the transaction below:

```
    const prepared = {
      "TransactionType": "SetHook",
      "Account": your_account_address,
      "Flags": 0,
      "Hooks": [
        {
          "Hook": {
            "HookHash": "0FFB97070F3B6D4DFC5C088935A64DCEA4C20EC1EB40BABB26A5716A2A420350",
            "HookNamespace": "0000000000000000000000000000000000000000000000000000000000000000",
            "HookOn": "FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF7FFFFFFFFFFFFFFFFFFBFFFFE",
          }
        }
      ],
      ...networkInfo.txValues,
    };
```

## I want to try them without installing anything

You can try this hook just sending 1 XAH to one of the next networks (I am not responsible for loss of funds if the hook has stopped working.):

- Testnet: rK9rfPgLXExBY24X6aAfKNePMyqzhTDtCe
- Mainnet: rBne1jL7bPPkZpf1iESrgPS48cPt86K9RH


## Credits

This hook was originally created by @ekiserrepe. You can find more of his projects on ekiserrepe.com
