---
title: "Contracts"
date: 2018-12-29T11:02:05+06:00
lastmod: 2020-01-05T10:42:26+06:00
weight: 3
draft: false
# search related keywords
keywords: ["wfil"]
---


The WFIL contract is upgraded with minor changes referring to WETH10. The wrapped FIL will be implemented in the Filecoin network and deployed on the Filecoin main network and other test networks to provide vitality for the Filecoin DiFe ecology.

## WFIL Contracts

|Chain（ChainId）|Address|
|:----|:----|
|Mainnet（314）|Coming Soon...|
|Hyperspace（3141）|0x123abc123abc123abc123abc123abc|

## Features Specifications

### Contract Design

The WFIL contract are built with can‘t be upgraded, can‘t be terminated, and there is no service fee. Once deployed, it will no longer require maintenance.

Future new functions will release a new version of the contract to carry it, and guide users to switch in an orderly manner instead of forcing users to upgrade.

### WFIL

Wrapped Filecoin (WFIL) is an Filecoin (FIL) ERC-20 wrapper. You can `deposit` FIL and obtain a WFIL balance which can then be operated as an ERC-20 token. You can `withdraw` FIL from WFIL, which will then burn WFIL token in your wallet. The amount of WFIL token in any wallet is always identical to the balance of FIL deposited minus the FIL withdrawn with that specific wallet.

#### flashMinted()

Returns current amount of flash-minted WFIL token.

```plain
function flashMinted() external view returns(uint256);
```

#### deposit()

`msg.value` of FIL sent to this contract grants caller account a matching increase in WFIL token balance.

Emits {Transfer} event to reflect WFIL token mint of `msg.value` from `address(0)` to caller account.

```plain
function deposit() external payable;
```

#### depositTo(address to)

`msg.value` of FIL sent to this contract grants `to` account a matching increase in WFIL token balance.

Emits {Transfer} event to reflect WFIL token mint of `msg.value` from `address(0)` to `to` account.

```plain
    function depositTo(address to) external payable;
```

#### withdraw(uint256 value)

Burn `value` WFIL token from caller account and withdraw matching FIL to the same.

Emits {Transfer} event to reflect WFIL token burn of `value` to `address(0)` from caller account.

Requirements:

- caller account must have at least `value` balance of WFIL token.

```plain
function withdraw(uint256 value) external;
```

#### withdrawTo(address payable to, uint256 value)

Burn `value` WFIL token from caller account and withdraw matching FIL to account (`to`).

Emits {Transfer} event to reflect WFIL token burn of `value` to `address(0)` from caller account.

Requirements:

- caller account must have at least `value` balance of WFIL token.

```plain
function withdrawTo(address payable to, uint256 value) external;
```

#### withdrawFrom(address from, address payable to, uint256 value)

Burn `value` WFIL token from account (`from`) and withdraw matching FIL to account (`to`).

Emits {Approval} event to reflect reduced allowance `value` for caller account to spend from account (`from`),unless allowance is set to `type(uint256).max`

Emits {Transfer} event to reflect WFIL token burn of `value` to `address(0)` from account (`from`).

Requirements:

- `from` account must have at least `value` balance of WFIL token.

- `from` account must have approved caller to spend at least `value` of WFIL token, unless `from` and caller are the same account.

```plain
function withdrawFrom(address from, address payable to, uint256 value) external;
```

#### depositToAndCall(address to, bytes calldata data)

`msg.value` of FIL sent to this contract grants `to` account a matching increase in WFIL token balance,after which a call is executed to an ERC677-compliant contract with the `data` parameter.

Emits {Transfer} event.

Returns boolean value indicating whether operation succeeded.

For more information on {transferAndCall} format, see [https://github.com/ethereum/EIPs/issues/677.](https://github.com/ethereum/EIPs/issues/677.)

```plain
function depositToAndCall(address to, bytes calldata data) external payable returns (bool);
```

#### approveAndCall(address spender, uint256 value, bytes calldata data)

Sets `value` as allowance of `spender` account over caller account's WFIL token, after which a call is executed to an ERC677-compliant contract with the `data` parameter.

Emits {Approval} event.

Returns boolean value indicating whether operation succeeded.

For more information on {approveAndCall} format, see [https://github.com/ethereum/EIPs/issues/677.](https://github.com/ethereum/EIPs/issues/677.)

```plain
function approveAndCall(address spender, uint256 value, bytes calldata data) external returns (bool);
```

#### transferAndCall(address to, uint value, bytes calldata data)

Moves `value` WFIL token from caller's account to account (`to`), after which a call is executed to an ERC677-compliant contract with the `data` parameter.A transfer to `address(0)` triggers an FIL withdraw matching the sent WFIL token in favor of caller.

Emits {Transfer} event.

Returns boolean value indicating whether operation succeeded.

Requirements:

- caller account must have at least `value` WFIL token.

For more information on {transferAndCall} format, see [https://github.com/ethereum/EIPs/issues/677.](https://github.com/ethereum/EIPs/issues/677.)

```plain
function transferAndCall(address to, uint value, bytes calldata data) external returns (bool);
```
### Approvals

This contract implements EIP2612 (an ERC20 Permit extension).Its approval can be signed offline, and the signature information can be submitted to the chain when the transfer transaction is received, and the authorization and transfer are completed in one transaction. At the same time, the transfer transaction can also be submitted by the recipient (or a third party), avoiding the need for users to rely on FIL.

Compliant contracts must implement 3 new functions in addition to EIP-20:

#### permit(address owner, address spender, uint256 value, uint256 deadline, uint8 v, bytes32 r, bytes32 s)

Sets `value` as the allowance of `spender` over `owner`'s tokens, given `owner`'s signed approval.

IMPORTANT: The same issues {IERC20-approve} has related to transaction ordering also apply here.

Emits an {Approval} event.

Requirements:

- `owner` cannot be `address(0)`.

- `spender` cannot be `address(0)`.

- `deadline` must be a timestamp in the future.

- `v`, `r` and `s` must be a valid `secp256k1` signature from `owner` over the EIP712-formatted function arguments.

- the signature must use `owner`'s current nonce (see {nonces}).

For more information on the signature format, see the [https://eips.ethereum.org/EIPS/eip-2612](https://eips.ethereum.org/EIPS/eip-2612)

```plain
function permit(address owner, address spender, uint256 value, uint256 deadline, uint8 v, bytes32 r, bytes32 s) external;
```

#### nonces(address owner)

Returns the current [ERC2612](https://eips.ethereum.org/EIPS/eip-2612) nonce for `owner`. This value must be ncluded whenever a signature is generated for {permit}.

Every successful call to {permit} increases `owner`'s nonce by one. This prevents a signature from being used multiple times.

```plain
function nonces(address owner) external view returns (uint256);
```
  
#### DOMAIN_SEPARATOR()

Returns the domain separator used in the encoding of the signature for {permit}, as defined by EIP712.

```plain
function DOMAIN_SEPARATOR() external view returns (bytes32);
```

### Flash Loans

This contract implements [EIP3156](https://eips.ethereum.org/EIPS/eip-3156) that allows to flashLoan an arbitrary amount of Wrapped FIL, unbacked by real FIL, with the condition that it is burned before the end of the transaction. No fees are charged.

This function will call onFlashLoan on the calling address, receiving and passing along a bytes parameter which can be used by the calling contract to process the callback.

This feature requires users to have the ability to write and publish smart contracts by themselves. By implementing the contract of IERC3156FlashBorrower to realize the callback of the method, you can refer to the following example:

```
// SPDX-License-Identifier: GPL-3.0-or-later
pragma solidity 0.7.6;

import "../interfaces/IWFIL.sol";
import "../interfaces/IERC3156FlashBorrower.sol";

contract TestFlashLender is IERC3156FlashBorrower {
    enum Action {NORMAL, STEAL, WITHDRAW, REENTER}

    uint256 public flashBalance;
    address public flashToken;
    uint256 public flashValue;
    address public flashSender;

    receive() external payable {}

    function onFlashLoan(address sender, address token, uint256 value, uint256, bytes calldata data) external override returns(bytes32) {
        address lender = msg.sender;
        (Action action) = abi.decode(data, (Action)); // Use this to unpack arbitrary data
        flashSender = sender;
        flashToken = token;
        flashValue = value;
        if (action == Action.NORMAL) {
            flashBalance = IWFIL(lender).balanceOf(address(this));
        } else if (action == Action.WITHDRAW) {
            IWFIL(lender).withdraw(value);
            flashBalance = address(this).balance;
            IWFIL(lender).deposit{ value: value }();
        } else if (action == Action.STEAL) {
            // Do nothing
        } else if (action == Action.REENTER) {
            bytes memory newData = abi.encode(Action.NORMAL);
            IWFIL(lender).approve(lender, IWFIL(lender).allowance(address(this), lender) + value * 2);
            IWFIL(lender).flashLoan(this, address(lender), value * 2, newData);
        }
        return keccak256("ERC3156FlashBorrower.onFlashLoan");
    }

    function flashLoan(address lender, uint256 value) public {
        // Use this to pack arbitrary data to `onFlashLoan`
        bytes memory data = abi.encode(Action.NORMAL);
        IWFIL(lender).approve(lender, value);
        IWFIL(lender).flashLoan(this, address(lender), value, data);
    }

    function flashLoanAndWithdraw(address lender, uint256 value) public {
        // Use this to pack arbitrary data to `onFlashLoan`
        bytes memory data = abi.encode(Action.WITHDRAW);
        IWFIL(lender).approve(lender, value);
        IWFIL(lender).flashLoan(this, address(lender), value, data);
    }

    function flashLoanAndSteal(address lender, uint256 value) public {
        // Use this to pack arbitrary data to `onFlashLoan`
        bytes memory data = abi.encode(Action.STEAL);
        IWFIL(lender).flashLoan(this, address(lender), value, data);
    }

    function flashLoanAndReenter(address lender, uint256 value) public {
        // Use this to pack arbitrary data to `onFlashLoan`
        bytes memory data = abi.encode(Action.REENTER);
        IWFIL(lender).approve(lender, value);
        IWFIL(lender).flashLoan(this, address(lender), value, data);
    }
}
```
