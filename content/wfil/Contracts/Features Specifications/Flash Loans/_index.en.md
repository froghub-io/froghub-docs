---
title: "Flash Loans"
date: 2018-12-29T11:02:05+06:00
lastmod: 2023-02-15T10:42:26+06:00
weight: 4
draft: false
# search related keywords
keywords: ["wfil"]
---


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
