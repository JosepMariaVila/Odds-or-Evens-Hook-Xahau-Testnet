![oddsevenspetit](https://github.com/user-attachments/assets/d4f5bd2f-d8dd-48d6-9bef-947e8be05a5f)


# Odds or Evens, a game Hook 

## Introduction

Odds or Evens is a hook programmed in C for the Xahau blockchain. A hook is a series of rules that enable smart logic in Xahau, considered the smart contracts of Xahau. 

Odds or Evens is a Hook that converts a Xahau account in an engine for a simple game. One player will hava an Odd or and Even number based on the sequence ledger of the payload he sent. Next player will have an Odd or an Even number based on the sequence ledger of the payload he sent. 

The hooks in which this hook is based is the Highest Number Hook by @ekiserrepe. That hook had an undesirable thing, the range between numbers where in some cases predictable, for example, if first player had a 0 or 1, then there were high chances the second player was able to win because, 2, 3, 4, 5, 6, 7, 8 and 9 would win him. So I thought this should be reduced to a range of 2 options, this way the result would be less predictable. A way to do so is reduce de ledger sequence number to an odd or even number or to its corresponding remainders when they are dividid by 2. The result of dividing the ledger sequence by 2 is 0 for even numbers, on 1 for odd numbers.

But who wins?

If the first player has a remainder of 0 and the second player a remainder of 0, the first player wins. 

If the first player has a remainder of 0 and the second player a remainder of 1, the second player wins. 

If the first player has a remainder of 1 and the second player a remainder of 1, the first player wins. 

If the first player has a remainder of 1 and the second player a remainder of 0, the second player wins. 

These rules can be sumarized like this:
Compare the remainders of both players regarding being odd or even, if they are equal, first player wins, if they are different, second player wins.

## Explanation

**Attention:** Only use if you are sure of what you are doing on Testnet/Mainnet. You could put your funds at risk. It is recommended to install on new accounts.

The hook when installed allows you to play the game of the odds or evens numbers. The hook will accept two players sending 1 XAH. With each payment the hook will check the sequence of the current ledger. The hook will check if it's an odd or even number and store the remainder of the division in the namespace next to the address of the first player referring to the first payment he receives. When a second player sends a payment of 1 XAH to the hook address, the hook will check the sequence and compare the remainder with that of the first player. This can lead to 2 possible outcomes. Both have the same remainder or a different one. If both have the same remainder, Player 1 wins and receives 2 XAH. If both players have a different remainder, Player 2 wins and receives 2 XAH. 

The hook blocks any payment other than 1 XAH. So a third case could occur, that the hook account runs out of funds and the game cannot be managed. If there are insufficient funds it might not be possible to send the “prize” to the winners. Therefore, it has been enabled to manage an account known as funding “FUND” that the hook allows to operate payments in both directions to be able to take out or put in XAH and avoid the mentioned problem. To assign a “FUND” account it is necessary to create an Invoke transaction from the Hook account with the parameter “FUND” and the account that we want to assign as a “FUND” account in the parameter value. The process is explained below.

## Installation & Usage

Once the hook is installed, the following triggers are expected for the hook.

- An account will send a payment of 1 XAH to the hook account. The payment account will be known as Player 1. The hook will register in the namespace the address of Player one with the key P1AD and store the odd or even number of the ledger sequence in the namespace under the key P1LG.

- A second account (different from the first one) will send a payment of 1 XAH to the hook account. The payment account will be known as Player 2. The hook will compare the oddeness or evenss of the ledger sequence number with that of Player 1 and decide the final result of the game.

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
