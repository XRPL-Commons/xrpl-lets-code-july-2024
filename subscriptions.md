In this session we'll demonstrate how to coordinate via the XRPL. 

The XRPL is a public ledger, meaning that when you write on chain anyone can read. In this session we will demonstrate how to subscribe to events as they happen on chain. 

First off we will create a pair of accounts:

```
import xrpl from 'xrpl';

const serverURL = 'wss://s.altnet.rippletest.net:51233'; // For testnet
const walletAddress = 'rfwGxAj4B1NkBihbuL83CiGjvsp9BVzrKX'

const main = async () => {
  const client = new xrpl.Client(serverURL)
  await client.connect()

  // do something useful
    const subscriptionRequest = {
      command: 'subscribe',
      accounts: [walletAddress]
    };

    await client.request(subscriptionRequest)
    console.log(`Subscribed to transactions for account: ${walletAddress}`)

    // Event listener for incoming transactions
    client.on('transaction', (transaction) => {
      console.log('Transaction:', transaction);
    })

    // Event listener for errors
    client.on('error', (error) => {
      console.error('Error:', error);
    })

  // end
  // keep open
  console.log('all done')
}

main()
setInterval(() => {
  console.log('One second has passed!');
}, 1000);
```


Then we will generate some transactions on the one hand.

```
import xrpl from "xrpl";

const serverURL = "wss://s.altnet.rippletest.net:51233"; // For testnet

const wallet1 = xrpl.Wallet.fromSeed("sEdVYquTyQbwn7URKzzdbJ6L4tx1n5u");
const wallet2 = xrpl.Wallet.fromSeed("sEdTxX3LJChazh9FDU51nVDd11n5nDQ");

// Call the function to read transactions

const main = async () => {
  const client = new xrpl.Client(serverURL);
  await client.connect();

  // do something useful
  console.log("funding wallet");
  console.log(
    wallet1.classicAddress,
    await client.getBalances(wallet1.classicAddress),
  );
  console.log(
    wallet2.classicAddress,
    await client.getBalances(wallet2.classicAddress),
  );

  const tx = {
    TransactionType: "Payment",
      Account: wallet1.classicAddress,
      Destination: wallet2.classicAddress,
      Amount: "1234",
  }

  const result = await client.submitAndWait(tx, {
    autofill: true,
    wallet: wallet1,
  });
  console.log(result)

  // console.log(wallet1)

  // const request = {
  //   command: 'account_tx',
  //   account: wallet1.classicAddress,
  //   ledger_index_min: -1, // To get transactions from all ledgers
  //   ledger_index_max: -1, // To get transactions up to the most recent ledger
  //   limit: 10, // Limit the number of transactions (optional)
  // }
  // const response = await client.request(request)
  // console.log('Account Transactions:', response.result.transactions)

  // end
  client.disconnect()
};

main()

```

And finally we will demonstrate how to listen for transactions affecting a given account. 

```
import xrpl from 'xrpl';

const serverURL = 'wss://s.altnet.rippletest.net:51233'; // For testnet
const walletAddress = 'rfwGxAj4B1NkBihbuL83CiGjvsp9BVzrKX'

const main = async () => {
  const client = new xrpl.Client(serverURL)
  await client.connect()

  // do something useful
    const subscriptionRequest = {
      command: 'subscribe',
      accounts: [walletAddress]
    };

    await client.request(subscriptionRequest)
    console.log(`Subscribed to transactions for account: ${walletAddress}`)

    // Event listener for incoming transactions
    client.on('transaction', (transaction) => {
      console.log('Transaction:', transaction);
    })

    // Event listener for errors
    client.on('error', (error) => {
      console.error('Error:', error);
    })

  // end
  // keep open
  console.log('all done')
}

main()
setInterval(() => {
  console.log('One second has passed!');
}, 1000)
```

