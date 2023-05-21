# Detect legit-looking contracts which are actually malicious
## 👎 Genuine looking malicious contracts through external helpers

In the crypto world, you will often hear about how contracts which looked legitimate were the reason behind a big scam. How are hackers able to execute malicious code from a legitimate looking contract?

We will learn one method today 👀

## 👀 What will happen?

There will be three contracts - Malicious.sol, Helper.sol and Good.sol. User will be able to enter an eligibility list using Good.sol which will further call Helper.sol to keep track of all the users which are eligible.

Malicious.sol will be designed in such a way that eligibility list can be manipulated, lets see how 👀

```shell
gitpod /workspace/LW3-malicious-contracts (main) $ npx hardhat test
Compiled 4 Solidity files successfully


  Lock
    Deployment
      ✔ Should set the right unlockTime (986ms)
      ✔ Should set the right owner
      ✔ Should receive and store the funds to lock
      ✔ Should fail if the unlockTime is not in the future
    Withdrawals
      Validations
        ✔ Should revert with the right error if called too soon
        ✔ Should revert with the right error if called from another account
        ✔ Shouldn't fail if the unlockTime has arrived and the owner calls it
      Events
        ✔ Should emit an event on withdrawals
      Transfers
        ✔ Should transfer the funds to the owner

  Malicious External Contract
Malicious Contract's Address 0x9fE46736679d2D9a65F0992F2272dE9f3c7fa6e0
Good Contract's Address: 0xCf7Ed3AccA5a467e9e704C703E8D87F634fB0Fc9
    ✔ Should change the owner of the Good contract (82ms)


  10 passing (1s)
```
# 👮 Prevention

Make the address of the external contract public and also get your external contract verified so that all users can view the code

Create a new contract, instead of typecasting an address into a contract inside the constructor. So instead of doing Helper(_helper) where you are typecasting _helper address into a contract which may or may not be the Helper contract, create an explicit new helper contract instance using new Helper().

Example
```shell
contract Good {
    Helper public helper;
    constructor() {
        helper = new Helper();
}
```