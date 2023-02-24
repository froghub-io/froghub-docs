---
title: "WFIL"
date: 2018-12-29T11:02:05+06:00
lastmod: 2023-02-15T10:42:26+06:00
weight: 2
draft: false
# search related keywords
keywords: ["wfil"]
---


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
