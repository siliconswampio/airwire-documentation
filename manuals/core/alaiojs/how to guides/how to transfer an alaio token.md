To transfer an alaio token, [submit a transaction](01_how-to-submit-a-transaction.md) to the [`transfer`](https://github.com/ALADINIO/alaio.contracts/blob/52fbd4ac7e6c38c558302c48d00469a4bed35f7c/contracts/alaio.token/include/alaio.token/alaio.token.hpp#L83) action of the account storing the token you wish to transfer.

In the example shown below `useraaaaaaaa` transfers **1.0000 ALA** token stored in the `alaio.token` account from `useraaaaaaaa` to `userbbbbbbbb`.
```javascript
(async () => {
  await api.transact({
    actions: [{
      account: 'alaio.token',
      name: 'transfer',
      authorization: [{
        actor: 'useraaaaaaaa',
        permission: 'active',
      }],
      data: {
        from: 'useraaaaaaaa',
        to: 'userbbbbbbbb',
        quantity: '1.0000 ALA',
        memo: 'some memo'
      }
    }]
  }, {
    blocksBehind: 3,
    expireSeconds: 30,
  });
})();
```