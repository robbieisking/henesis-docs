---
description: Henesis makes it easy to track the status changes of transactions.
---

# Introduction

## What is a Transaction?

The "transaction" is the signed data set to change the state recorded in the blockchain.

To change the state on the blockchain, a transaction must be created and propagated to the blockchain network. However, dealing with transactions in the blockchain is difficult. Why?

* Transactions are not mined in blocks in real time. Therefore, in order to give the user accurate information, we need to know exactly the current status of the transaction.
* Mining transactions could be delayed or canceled, depending on several situations.

Then, what does it mean "Handling transactions properly"?

* It constantly checks whether a transaction was finally mined and notifies the result to users
* If the transaction has not been mined within expected time, investigate the cause and take appropriate actions.

In short, you need to monitor the status of the transaction in real time, and take an appropriate action regarding to the status of transactions. What kinds of status a transaction can have?

## Transaction Status

In Henesis, there are **three transaction status.**

<table>
  <thead>
    <tr>
      <th style="text-align:left">Status</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><b>Pending</b>
      </td>
      <td style="text-align:left">
        <p></p>
        <p>The transaction is created, but not yet mined. This can be caused largely
          by two cases.</p>
        <ol>
          <li>In case the transaction is not propagated correctly in the blockchain
            network, the transaction is not contained in the miner&apos;s tx pool.</li>
          <li>In case the gas price is low, miners don&apos;t put transactions into
            blocks</li>
        </ol>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Receipt</b>
      </td>
      <td style="text-align:left">
        <p>The transaction has been mined.</p>
        <p></p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Confirmation</b>
      </td>
      <td style="text-align:left">After transactions are mined, additional blocks are created, making chain
        re-organization difficult.</td>
    </tr>
  </tbody>
</table>## Difficulties in tracking transaction's status 

Now, we know there are **three transaction status**. However, tracking the status of all transactions generated in your DApps is more difficult than you might expect.

* The more transactions you track, the more system resources are consumed.
* To check the `pending` status, you need to query the transaction pool of blockchain nodes. However, most node providers don't support RPC calls for querying the transaction pool. Therefore, you must operate stable nodes by yourself.
* You also need to build an infrastructure that keeps your service informed of updates in real time whenever transaction status is changed.

## Henesis solves those problems

* Henesis provides the managed transaction tracker to help customers handle many transactions without scaling system resources
* Henesis uses own blockchain nodes that supports querying the transaction pool for tracking transactions. So you don't have to run a blockchain node.
* Henesis informs the status of transactions via WebSocket in real-time. So there is no need to build a separate infrastructure for message delivery.





