> abstract class `Transaction`

<details>
<summary><b>Table of Contents</b></summary>

| Item | Java | JavaScript | Go
| - | - | - | - |
| [`fromBytes`](#frombytes-data-bytes-transaction) | ✅ | ✅ | ✅
| [`nodeAccountIds`](#nodeaccountids-accountid) | ✅ | ✅ | ✅
| [`transactionValidDuration`](#transactionvalidduration-duration) | ✅ | ✅ | ✅
| [`transactionMemo`](#transactionmemo-string) | ✅ | ✅ | ✅
| [`transactionId`](#transactionid-transactionid) | ✅ | ✅ | ✅
| [`maxTransactionFee`](#maxtransactionfee-hbar) | ✅ | ✅ | ✅
| [`maxRetry`](#maxretry-uint64) | ✅ | ✅ | ✅
| [`getTransactionHash`](#gettransactionhash-bytes) | ✅ | ✅ | ✅
| [`getTransactionHashPerNode`](#gettransactionhashpernode-map-lt-accoundid-byte-gt) | ✅ | ✅ | ✅
| [`toBytes`](#tobytes-bytes) | ✅ | ✅ | ✅
| [`sign`](#sign-key-privatekey-this) | ✅ | ✅ | ✅
| [`signWith`](#signwith-publickey-publickey-transactionsigner-bytes-gt-bytes-this) | ✅ | ✅ | ✅
| [`signWithOperator`](#signwithoperator-client-client-this) | ✅ | ✅ | ✅
| [`getSignatures`](#getsignatures-map-lt-accoundid-map-lt-publickey-bytes-gt) | ✅ | ✅ | ✅
| [`addSignature`](#addsignaturekey-publickey-signature-byte-this) | ✅ | ✅ | ✅
| [`freeze`](#freeze-this) | ✅ | ✅ | ✅
| [`freezeWith`](#freezewith-client-client-this) | ✅ | ✅ | ✅
| [`execute`](#execute-client-client-transactionresponse) | ✅ | ✅ | ✅

</details>

Base class for all transactions that may be submitted to Hedera.

### Static Methods

##### `fromBytes` ( `data` : `bytes` ): [`Transaction`](#transaction)

---

### Methods

##### `execute` ( `client`: [`Client`](reference/core/Client.md) ): [`TransactionResponse`](reference/core/TransactionResponse.md)

Execute this transaction on the Hedera network, immediately returning
metadata about the transaction that was executed.
The [`TransactionResponse`](reference/core/TransactionResponse.md) may be used to fetch
the [`TransactionReceipt`](reference/core/TransactionReceipt.md) or
[`TransactionRecord`](reference/core/TransactionRecord.md)
for more information.

---

##### `toBytes` (): `bytes`

Encodes this transaction to a byte array that can be later decoded into
the same [`Transaction`](#transaction).

---

##### `getTransactionHash` (): `bytes`

---

##### `getTransactionHashPerNode()`: `Map` < [`AccoundId`](reference/cryptocurrency/AccountId.md), `bytes` >

---

##### `sign` ( `key`: [`PrivateKey`](reference/cryptography/PrivateKey.md) ): `this`

---

##### `signWith` ( `publicKey`: [`PublicKey`](reference/cryptography/PublicKey.md), `transactionSigner`: `(bytes) => bytes` ): `this`

---

##### `signWithOperator` ( `client`: [`Client`](reference/core/Client.md) ): `this`

---

##### `freeze` (): `this`

---

##### `freezeWith` ( `client`: [`Client`](reference/core/Client.md) ): `this`

---

##### `getSignatures()`: `Map` < [`AccoundId`](reference/cryptocurrency/AccountId.md), `Map` < [`PublicKey`](reference/cryptography/PublicKey.md), `bytes` >

---

##### `addSignature`(`key`: [`PublicKey`](reference/cryptography/PublicKey.md), `signature`: `byte[]`): `this`

---

### Properties

##### `nodeAccountIds`: [`AccountId`](reference/AccountId.md)\[\]

The list of node account IDs that this transaction will be submitted to.

Providing an explicit set of node account IDs interferes with client-side load
balancing of the network. By default, the SDK will pre-generate a transaction
for 1/3 of the nodes on the network. If a node is down, busy, or otherwise
reports a fatal error, the SDK will try again with a different node.

---

##### `transactionValidDuration`: `Duration`

Duration from the valid start (within the transaction ID) that this
transaction is valid for.

Defaults to 120 seconds.

---

##### `maxTransactionFee`: `Hbar`

The maximum transaction fee the client is willing to pay.

Defaults to `maxTransactionFee` from the `client`.

---

##### `transactionId`: [`TransactionId`](reference/core/TransactionId.md)

The ID for this transaction, which includes the payer's
account (the account paying the transaction fee).

If two transactions have the same transactionID, they won't both have an effect

---

##### `transactionMemo`: `String`

Any notes or descriptions that should be put into the record (max length 100).

---

##### `maxRetry`: `Uint64`

The number of times to return this transaction. Transactions are retried when
Hedera Hashgraph returns a status code that implies the transaction should be
resubmitted, or when a node doesn't responde.

---
