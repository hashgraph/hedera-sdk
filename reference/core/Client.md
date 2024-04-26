> class `Client`

Managed client for use on the Hedera Hashgraph network.

<!-- tabs:start -->

### ** Java **

```java
// create from..
var network = new HashMap<String, AccountId>();
network.put("0.testnet.hedera.com:50211", new AccountId(3));

var client = Client.forMainnet(); // pre-defined
var client = Client.forNetwork(network); // custom network
var client = Client.fromConfigFile("./client-config.json"); // configuration file
var client = Client.forName("testnet"); // named

// set operator (to use to pay transaction fees)
client.setOperator(operatorAccountId, operatorPrivateKey);
```

### ** JavaScript **

```javascript
// create from..
const network = {
    "0.testnet.hedera.com:50211": "0.0.3"
};

const client = Client.forMainnet(); // pre-defined
const client = Client.forNetwork(network); // custom network
const client = Client.fromConfigFile("./client-config.json"); // configuration file
const client = Client.forName("testnet"); // named

// set operator (to use to pay transaction fees)
client.setOperator(operatorAccountId, operatorPrivateKey);
```

### ** Go **

```go
// create from..
network := make(map[string]AccountID)
network["0.testnet.hedera.com:50211"] = AccountID{Account: 3}

client := ClientForMainnet(); // pre-defined
client := ClientForNetwork(network); // custom network
client, err := ClientFromConfigFile("./client-config.json"); // configuration file
client, err := ClientForName("testnet"); // named

// set operator (to use to pay transaction fees)
client.setOperator(operatorAccountId, operatorPrivateKey);
```

<!-- tabs:end -->

### Static Methods

##### `forCustomNetwork` ( `network`: `Map` < `String` , [`AccountId`](../cryptocurrency/AccountId.md) >, `ledgerId`: [`LedgerId`](../LedgerId.md) ): `Client`

Construct a client for a specific network.

It is the responsibility of the caller to ensure that all nodes
in the map are part of the same Hedera network. Failure to do
so will result in undefined behavior.

The client will load balance all requests to Hedera using
a simple round-robin scheme to chose nodes to send transactions
to. For one transaction, at most 1/3 of the nodes will be tried.

Network constants are made available to use.

---

##### `forMainnet` (): `Client`

Construct a Hedera client pre-configured for Mainnet access.

Will initially be filled with a hard-coded address book for the consensus node network, but in the background the client will immediately attempt to update its consensus node network using an [`AddressBookQuery`](../network/AddressBookQuery.md) against the mirror network.  If the query fails, the consensus node network will remain unchanged and the query failure will be logged.

---

##### `forTestnet` (): `Client`

Construct a Hedera client pre-configured for Testnet access.

Will initially be filled with a hard-coded address book for the consensus node network, but in the background the client will immediately attempt to update its consensus node network using an [`AddressBookQuery`](../network/AddressBookQuery.md) against the mirror network.  If the query fails, the consensus node network will remain unchanged and the query failure will be logged.

---

##### `forPreviewnet` (): `Client`

Construct a Hedera client pre-configured for Previewnet access.

Will initially be filled with a hard-coded address book for the consensus node network, but in the background the client will immediately attempt to update its consensus node network using an [`AddressBookQuery`](../network/AddressBookQuery.md) against the mirror network.  If the query fails, the consensus node network will remain unchanged and the query failure will be logged.

---

##### `forName` ( `name`: `String` ): `Client`

Construct a Hedera client for a given name.

Valid names are "previewnet", "testnet", "mainnet"

For a valid name, the behavior is identical to `for[Mainnet|Testnet|Previewnet]()`

---

##### `fromConfig` ( `data`: `String` ): `Client`

Configure a client from the JSON configuration string.

<details>
<summary><b>File Specification</b></summary>

`network` can be `mainnet`, `testnet`, `previewnet`, or a dictionary of Account
ID to IP:PORT

```json
{
  "network": "mainnet"
}
```

or

```json
{
  "network": { "0.0.1": "0.testnet.hedera.com:50211" }
}
```

`operator` is an _optional_ object

```json
{
  "operator": {
    "accountId": "0.0.21",
    "privateKey": "302...."
  }
}
```

`mirrorNetwork` can be a network name (mainnet, previewnet, etc.) or a list
of addresses. `mirrorNetwork` defaults to the name of `network` _if_ that is
a network name.

```json
{
  "mirrorNetwork": "mainnet"
}
```

or

```json
{
  "mirrorNetwork": [ "kabuto.sh:50211", "hedera.com:50211" ]
}
```

</details>

---

##### `fromConfigFile` ( `filename`: `String` ): `Client`

Configure a client from the JSON configuration file. The file must have the same structure as the JSON in [`fromConfig`](#fromconfig--data--string---client)

---

### Methods

##### `close` ()

Close all open connections with the Hedera network; and, release all
associated resources.

---

##### `ping` ( `nodeAccountId`: [`AccountId`](../cryptocurrency/AccountId.md))

Ping a single node

**NOTE**: This method will **not** throw if the node doesn't respond with a successful status code,
instead the network will de-prioritize it.

---

##### `pingAll` ()

Ping all nodes in the current network.

**NOTE**: This method will **not** throw if a node doesn't respond with a successful status code,
instead the network will de-prioritize it.

---

##### `setNetworkFromAddressBook` ( `addressBook`: `AddressBook`)

Replaces the current network with one which is given in an `AddressBook` (which presumably was acquired via an [`AddressBookQuery`](../network/AddressBookQuery.md))

---

##### `setOperator` ( `accountId`: [`AccountId`](../cryptocurrency/AccountId.md), `privateKey`: [`PrivateKey`](../cryptography/PrivateKey.md) ): `Client`

Sets the account that will, by default, pay for transactions and queries built
with this client.

---

##### `setOperatorWith` ( `accountId`: [`AccountId`](../cryptocurrency/AccountId.md), `publicKey`: [`PublicKey`](../cryptography/PublicKey.md), `transactionSigner`: `(bytes) => bytes` ): `Client`

Sets the account that will, by default, pay for transactions and queries built
with this client.

It is expected that the signing method utilize the private key associated
with the given public key.

This form is made available for integrating the SDK to sign
from an external source such as the Ledger Hardware Wallet.

---

### Properties

##### `autoValidateChecksums` : `bool`

Is automatic entity ID checksum validation enabled.

---

##### `closeTimeout` : `Duration`

Maximum amount of time closing a network can take.

---

##### `defaultMaxQueryPayment` : [`Hbar`](../Hbar.md)

The maximum query payment.

**NOTE**: Defaults to 1 Hbar.

---

##### `defaultMaxTransactionFee` : [`Hbar`](../Hbar.md)

The default maximum fee used for transactions.

---

##### `defaultRegenerateTransactionId` : `bool`

Declares if we should generate new transaction IDs when a transaction fails with `TRANSACTION_EXPIRED`.

**NOTE**: Defaults to `true`

---

##### `ledgerId` () : [`LedgerId`](../LedgerId.md)

Current LedgerId of the network; corresponds to ledger ID in entity ID checksum calculations.

---

##### `maxAttempts` () : `Uint32`

Max number of attempts a request executed with this client will do.

---

##### `maxBackoff` () : `Duration`

The maximum amount of time to wait between retries

---

##### `maxNodeAttempts` () : `Uint32`

Max number of times any node in the network can receive a bad gRPC status before being removed from the network.

---

##### `minBackoff` () : `Duration`

The minimum amount of time to wait between retries

---

##### `mirrorNetwork` : `List` < `String` >

The mirror network node list

---

##### `network` : `Map` < `String` , [`AccountId`](../cryptocurrency/AccountId.md) >

The list of network records

---

##### `networkUpdatePeriod` : `Duration`

If present, the client will periodically attempt to update its consensus node network in the background using
an [`AddressBookQuery`](../network/AddressBookQuery.md) against its current mirror network.
If the query fails, the consensus node network will remain unchanged and the query failure will be logged.

---

##### `operatorAccountId`: [`AccountId?`](../cryptocurrency/AccountId.md)

The ID of the operator

---

##### `operatorPublicKey`: [`PublicKey?`](../cryptography/PublicKey.md)

The key of the operator

---

##### `requestTimeout`: `Duration`

Maximum amount of time a request can run

---

##### `verifyCertificates` () : `bool`

Is certificate verification enabled

Only available in Java and Go SDKs
