____What does the function do____:
------------
### Global Explanation :
The function, tries to recalculate the balance of an account by it's `iban` then update the last balance and return the new information : the account and the updated database information

### Detailed Explanation :

![1](https://user-images.githubusercontent.com/52347993/200164726-903fa10e-f830-4348-b9b3-ff029ef480a7.png)

Here, we check if the value of `iban` is not an existing iban. If it's not, the function returns an error.

</br>
</br>

![2](https://user-images.githubusercontent.com/52347993/200164879-6849ca34-2aba-4c10-8b6a-a8df38a57365.png)

Then the function tries to disable timeouts, 
with `tx.Exec(...)` that executes transactions in the database. 
By setting into the DB `lock_timeout` and `statement_timeout` to 0. if it fails, it returns an error.

    `lock_timeout = 0` :
        it returns a message as soon as a lock is encountered.
    `statement_timeout = 0` :
        it's the maximum allowed duration of any statement, when it's at 0, meaning no timeout.

</br>
</br>

![3](https://user-images.githubusercontent.com/52347993/200165057-79b89f98-530b-458b-a546-fb894d2661f4.png)

* Here the function tries to recalculate the account balance, with the `iban` and the `startDate`. returning an error if it fails.

* Then, it tries to get from the database, the account that iban was set in parameter.
      the function tries to `select` from the database all the accounts that the `iban` is the one set in parameter.
      
      `FOR UPDATE` command, allows to lock the rows until all the transaction has been committed.

</br>
</br>

![5](https://user-images.githubusercontent.com/52347993/200165239-3425ce23-6397-41c0-8576-04806722c55b.png)
* After that, it gets the last account balance, in order to update it

</br>
</br>

![31](https://user-images.githubusercontent.com/52347993/200165454-dd65ea50-ea2c-4800-a8c0-ad14ceb47345.png)
![32](https://user-images.githubusercontent.com/52347993/200165455-4d6c6e1e-ca7b-4d65-926a-56ac8b121664.png)


* Then we call `dbutils.IsRecordNotFoundError(err) {...}` function, where we try to get the last accounted balance, and returns a pointer to the account and the updated transactions.

![7](https://user-images.githubusercontent.com/52347993/200165506-3858c5b9-e363-4bb3-9890-bc29e3ffdf58.png)

* After getting the last balance, the function makes a transaction to update it.
* Finally, the function returns the account, and the updated database.

