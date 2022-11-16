# 第5节：增加单元测试

上节我们已经编写了合约代码，本节我们将编写单元测试。

## Code

```rust
/// This module defines a minimal Coin and Balance.
module NamedAddr::BasicCoin {
    use std::signer;

    // 省略上一节的业务代码 ... 

    #[test(account = @0x1)] // Creates a signer for the `account` argument with address `@0x1`
    #[expected_failure] // This test should abort
    fun mint_non_owner(account: signer) acquires Balance {
        // Make sure the address we've chosen doesn't match the module
        // owner address
        publish_balance(&account);
        assert!(signer::address_of(&account) != MODULE_OWNER, 0);
        mint(&account, @0x1, 10);
    }

    #[test(account = @NamedAddr)] // Creates a signer for the `account` argument with the value of the named address `NamedAddr`
    fun mint_check_balance(account: signer) acquires Balance {
        let addr = signer::address_of(&account);
        publish_balance(&account);
        mint(&account, @NamedAddr, 42);
        assert!(balance_of(addr) == 42, 0);
    }

    #[test(account = @0x1)]
    fun publish_balance_has_zero(account: signer) acquires Balance {
        let addr = signer::address_of(&account);
        publish_balance(&account);
        assert!(balance_of(addr) == 0, 0);
    }

    #[test(account = @0x1)]
    #[expected_failure(abort_code = 2)] // Can specify an abort code
    fun publish_balance_already_exists(account: signer) {
        publish_balance(&account);
        publish_balance(&account);
    }

    #[test]
    #[expected_failure]
    fun withdraw_dne() acquires Balance {
        // Need to unpack the coin since `Coin` is a resource
        Coin { value: _ } = withdraw(@0x1, 0);
    }

    #[test(account = @0x1)]
    #[expected_failure] // This test should fail
    fun withdraw_too_much(account: signer) acquires Balance {
        let addr = signer::address_of(&account);
        publish_balance(&account);
        Coin { value: _ } = withdraw(addr, 1);
    }

    #[test(account = @NamedAddr)]
    fun can_withdraw_amount(account: signer) acquires Balance {
        publish_balance(&account);
        let amount = 1000;
        let addr = signer::address_of(&account);
        mint(&account, addr, amount);
        let Coin { value } = withdraw(addr, amount);
        assert!(value == amount, 0); }
  
  
      // EXERCISE: Write `balance_of_dne` test here!
      // # [test(account = @0x1)]
      # [test]
      # [expected_failure]
      fun balance_of_dne() acquires Balance {
          // balance_of(&account);
          balance_of(@0x1);
      }
}
```

运行：

```sh
move test
```

![image-20221115185532608](https://duke-typora.s3.ap-southeast-1.amazonaws.com/uPic/image-20221115185532608.png)

## Note

- 语法很怪，需要系统过一下才行

