# 第3节：设计module

接下来我们将编写一个token，并逐步完善细节，本节将定义token的接口。

## Code

我们准备发型一个Token，本节仅定义接口

```rust
module NamedAddr::BasicCoin {
    struct Coin has store {
        value: u64
    }

    struct Balance has key {
        coin: Coin
    }

    /// Publish an empty balance resource under `account`'s address. This function must be called before
    /// minting or transferring to the account.
    public fun publish_balance(account: &signer) { .. }

    /// Mint `amount` tokens to `mint_addr`. Mint must be approved by the module owner.
    public fun mint(module_owner: &signer, mint_addr: address, amount: u64) acquires Balance { .. }

    /// Returns the balance of `owner`.
    public fun balance_of(owner: address): u64 acquires Balance { .. }

    /// Transfers `amount` of tokens from `from` to `to`.
    public fun transfer(from: &signer, to: address, amount: u64) acquires Balance { .. }
}

```

- move里面的module没有storage，每个地址可以有module（code）和resources（value）；
- 每个地址下面的resource都是个mapping，这意味着每种类型只能有一个值；
- 上面的代码中，Balance是NameAddr下面的resource，它存储每个地址持有的数量。



## Note

只有修饰为public(script)的函数才可以被直接调用，function可见性，[点击查看](https://move-language.github.io/move/functions.html#visibility)

```js
public(script) fun transfer(from: signer, to: address, amount: u64) acquires Balance { ... }
```

状态对比：move

![image-20221115161455144](https://duke-typora.s3.ap-southeast-1.amazonaws.com/uPic/image-20221115161455144.png)

状态对比：solidity

![image-20221115162742556](https://duke-typora.s3.ap-southeast-1.amazonaws.com/uPic/image-20221115162742556.png)
