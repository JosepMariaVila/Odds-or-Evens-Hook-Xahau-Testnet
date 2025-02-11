![highestnumber](https://github.com/user-attachments/assets/6128313d-cced-4431-a290-b18ee25eb920)

# Highest Number A Minigame Hook 

## Introduction

Highest Number is a hook programmed in C for the Xahau blockchain. A hook is a series of rules that enable smart logic in Xahau, considered the smart contracts of Xahau.

Highest Number is a Hook that converts a Xahau account in an engine for a simple game. This a simple idea for others to use to create more sophisticated and customizable hooks.

## Explanation


**Attention:** Only use if you are sure of what you are doing on Testnet/Mainnet. You could put your funds at risk. It is recommended to install on new accounts.

The hook when installed allows you to play the game of the largest number. The hook will accept two players sending 1 XAH. With each payment the hook will check the sequence of the current ledger. The hook will store the rightmost number of the ledger sequence and store it in the namespace next to the address of the first player referring to the first payment he receives. When a second player sends a payment of 1 XAH to the hook address, the hook will check the sequence and compare the rightmost number with that of the first player. This can lead to 3 possible outcomes. Player 1 has the largest number and receives the 2 XAH. Player 2 has the largest number and receives the 2 XAH. There is a tie and each player receives 1 XAH. 

The hook blocks any payment other than 1 XAH. So a fourth case could occur, that the hook account runs out of funds and the game cannot be managed. If there are insufficient funds it might not be possible to send the “prize” to the winners. Therefore, it has been enabled to manage an account known as funding “FUND” that the hook allows to operate payments in both directions to be able to take out or put in XAH and avoid the mentioned problem. To assign a “FUND” account it is necessary to create an Invoke transaction from the Hook account with the parameter “FUND” and the account that we want to assign as a “FUND” account in the parameter value. The process is explained below.

## Installation & Usage

The hook is installed in the account that we will call Hook Account. Hook Account will be the one affected by the Stop Me Hook logic. To make it work we first need to decide the address (From now, Backup Account) that will be from now on responsible for the Hook Account to be able to send payments or not.

In order to assign an Backup Account we will use an Invoke transaction from the hook account. We will add the BACK parameter with the address of the backup account. We must translate it to HEX, for this we go to https://transia-rnd.github.io/xrpl-hex-visualizer/ , add the account and click “From Hex” and copy the value xrpAddress in our transaction.

If everything went well from now on, the new transactions we will create to manage the hook, will be sent from the Backup Account. If we give it orders from the Hook Account, it would not make sense to install a secuirity hook if we are going to be able to activate and deactivate it at will.

To activate the hook account payment sending block we must activate the ACTI flag. To do this we will create an Invoke transaction with origin the backup account and destination the hook account. We will add the parameter ACTI and the value 01. If everything went well, from now on if we try to make payments from our payment account it will not be possible until we deactivate the ACTI flag.

To deactivate the ACTI flag and allow outgoing payments again, we create an Invoke transaction with source the Backup Account and destination the hook Account. As hook parameter we will add ACTI and the value 00. If everything went well, from now on, the account will be able to operate normally again.

If we want to change the Backup Account for another account, we create an Invoke transaction with source the Backup Account and destination the Hook Account. We will use the Hook BACK parameter and the address of the new backup address in the same way we defined it last time.

If we want to completely eliminate the administration of the Backup Account and that the hook account no longer has a manager, we create an Invoke operation with origin the backup account and destination the hook account. As Hook parameter we will add DELE and as value the account translated to HEX where the hook is installed.


## How to install the Stop Me Hook on Testnet?

HookHash: E31E3F125C377E89ACC76610DD017FBEE434FB5BDAE86DEF3C345B994916C64F

1. You can do it by (XRPLWin Hook Install Tool)[https://xahau-testnet.xrplwin.com/tools/hook/from-hash]

2. Or you can do it sending the transaction below:

HookOn is activated to trigger for Invoke and Payment transactions. You can verify it copying the HookOn value (FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF7FFFFFFFFFFFFFFFFFFBFFFFE) in this website: https://richardah.github.io/xrpl-hookon-calculator/

    const prepared = {
      "TransactionType": "SetHook",
      "Account": your_account_address,
      "Flags": 0,
      "Hooks": [
        {
          "Hook": {
            "HookHash": "1A9D1EEA98A9BE3C45A35872E51E36B6E73CBB7033A96CE0D98DB484215E0494",
            "HookNamespace": "0000000000000000000000000000000000000000000000000000000000000000",
            "HookOn": "FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF7FFFFFFFFFFFFFFFFFFBFFFFE",
          }
        }
      ],
      ...networkInfo.txValues,
    };

## How to uninstall the Stop Me Hook on Mainnet/Testnet?

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

## Adding the backup account for first time

When there is no previous backup account in our namespace, we create a Invoke transaction with our Hook Account as "Account" and NO Destination Account. 

Hook Parameters and values will be:
- BACK
- backup_address

In this example we are using 1E2D42546C8A5270D4E182FAE3D12186F2A32A7E that is the translated version of the address rskZVQvBEXAwsBTFsgEZMBfwnhC7oydSnp. You can visit https://transia-rnd.github.io/xrpl-hex-visualizer/ , Insert the account and click on From Hex, you will see the xrpAddress will be the same as we added. BACK string is HEX translated: 4241434B. (For https://builder.xahau.network/ IDE you don't need to translate Parameter Name)

    const prepared = {
      TransactionType: "Invoke",
      Account: your_account_address,
      Flags: 0,
      HookParameters: [
        {
          HookParameter: {
            HookParameterName: "4241434B",
            HookParameterValue: "1E2D42546C8A5270D4E182FAE3D12186F2A32A7E",
          },
        },
        ],
      ...networkInfo.txValues,
    };

## Adding the backup account AFTER the first time

When there is a previous backup account in our namespace and we want to change it, we create a Invoke transaction with the backup account as "Account" and hook account as "Destination".

Hook Parameters and values will be:
- BACK
- new_backup_address

In this example we are using 09BA2DF121D730675E3CA15408B6AC24A481ACF9 that is the translation version of the address rtSf6j9hGRPHLRWiRUYjmoQBD1N2bT8Z1. You can visit https://transia-rnd.github.io/xrpl-hex-visualizer/ , insert the account and click on From Hex, you will see the xrpAddress will be the same as we added. BACK string is HEX translated: 4241434B. (For https://builder.xahau.network/ IDE you don't need to translate Parameter Name)

    const prepared = {
      TransactionType: "Invoke",
      Account: backup_account_address,
      Flags: 0,
      Destination: hook_account,
      HookParameters: [
        {
          HookParameter: {
            HookParameterName: "4241434B",
            HookParameterValue: "09BA2DF121D730675E3CA15408B6AC24A481ACF9",
          },
        },
        ],
      ...networkInfo.txValues,
    };

## Activating Flag (Stop allowing outgoing payments from the hook account)

When we want to activate the ACTI flag = 1 to stop allowing outgoing paymets from the hook account we generate the next transaction. A Invoke transaction with the backup account as "Account" and hook account as "Destination".

Hook Parameters and values will be:
- ACTI
- 01


    const prepared = {
      TransactionType: "Invoke",
      Account: backup_account_address,
      Flags: 0,
      Destination: hook_account,
      HookParameters: [
        {
          HookParameter: {
            HookParameterName: "41435449",
            HookParameterValue: "01",
          },
        },
        ],
      ...networkInfo.txValues,
    };

## Disabling Flag (Allowing outgoing payments from the hook account)

When we want to disable the ACTI flag and allowing outgoing paymets from the hook account we generate the next transaction. A Invoke transaction with the backup account as "Account" and hook account as "Destination".

Hook Parameters and values will be:
- ACTI
- 00


    const prepared = {
      TransactionType: "Invoke",
      Account: backup_account_address,
      Flags: 0,
      Destination: hook_account,
      HookParameters: [
        {
          HookParameter: {
            HookParameterName: "41435449",
            HookParameterValue: "00",
          },
        },
        ],
      ...networkInfo.txValues,
    };

## Deleting Backup Account and Flag

When we want to delete the backup account and delete the ACTI flag, we create an Invoke transaction with the backup account as "Account" and hook account as "Destination".

Hook Parameters and values will be:
- DELE
- hook_account Visit https://transia-rnd.github.io/xrpl-hex-visualizer/ to know the HEX translation (xrplAddress).


    const prepared = {
      TransactionType: "Invoke",
      Account: backup_account_address,
      Flags: 0,
      Destination: hook_account,
      HookParameters: [
        {
          HookParameter: {
            HookParameterName: "44454C45",
            HookParameterValue: "hook_account_in_hex",
          },
        },
        ],
      ...networkInfo.txValues,
    };


## How to install the Stop Me Hook on Mainnet?

Same as Testnet but changing the hookhash. The Hookhash is B5822426DFE897EF446BBCDEC3F83D803EC80188390DDA74112F6AAFE202A9E8.

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
            "HookHash": "B5822426DFE897EF446BBCDEC3F83D803EC80188390DDA74112F6AAFE202A9E8",
            "HookNamespace": "0000000000000000000000000000000000000000000000000000000000000000",
            "HookOn": "FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF7FFFFFFFFFFFFFFFFFFBFFFFE",
          }
        }
      ],
      ...networkInfo.txValues,
    };
```

## Ideas for your hooks

This is just a small example so that others can build similar ideas or extend the current hook. Some ideas I propose:

1. Use timestamps and ledger sequences to make the denial of payments happen for a given time and not for a flag.
2. Let several accounts be administrators of the management of the hook account instead of having only one backup account.
3. That the blocking of payments is from a certain amount.
4. That in case of detecting a strange situation, all the money is sent to the backup account or to another account for security.
5. Stop more types of transactions, you can check the codes here: (https://docs.xahau.network/technical/hooks-c-functions/originating-transaction/otxn_type)

## Special thanks

Thanks to [@denis_angell](https://x.com/angell_denis) and [@Satish_nl](https://x.com/angell_denis) for being there when i get stuck.
