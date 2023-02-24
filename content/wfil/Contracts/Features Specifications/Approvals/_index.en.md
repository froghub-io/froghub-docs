---
title: "Approvals"
date: 2018-12-29T11:02:05+06:00
lastmod: 2023-02-15T10:42:26+06:00
weight: 3
draft: false
# search related keywords
keywords: ["wfil"]
---


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
