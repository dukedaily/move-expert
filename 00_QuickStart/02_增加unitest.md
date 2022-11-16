# 第2节：单元测试

本节我们将运行代码，并执行单元测试。

## Code

修改为如下内容：

```rust
module 0xCAFE::BasicCoin {
		//1. 仅在单元测试中编译，导入标准库
    #[test_only]
    use std::signer;

    struct Coin has key {
        value: u64,
    }

    public fun mint(account: signer, value: u64) {
        move_to(&account, Coin { value })
    }

		//2. 创建一个testcase，为account赋值
    #[test(account = @0xC0FFEE)]
    fun test_mint_10(account: signer) acquires Coin {
        let addr = signer::address_of(&account);
        mint(account, 10);
				
     		// 获取addr的resources和value数量，进行比较
        assert!(borrow_global<Coin>(addr).value == 10, 0);
    }
}

```

修改配置文件：

```toml
[package]
name = "BasicCoin"
version = "0.0.0"

[dependencies]
# 这是一个标准库，在工程本地
MoveStdlib = { local = "../../../../move-stdlib/", addr_subst = { "std" = "0x1" } }
```

运行：

```sh
move test
```

![image-20221115151022550](https://duke-typora.s3.ap-southeast-1.amazonaws.com/uPic/image-20221115151022550.png)

更多操作：

```sh
# Exercise 1
move test -g

# Exercise 2
move test --coverage

Followed by:

move coverage summary
move coverage summary --summarize-functions
move coverage source --module BasicCoin
```

接下来，请将10改成11，再次运行`move test`，会报错，与预期相符合！

![image-20221115152402333](https://duke-typora.s3.ap-southeast-1.amazonaws.com/uPic/image-20221115152402333.png)



## Note

### 单测细节

- [点击查看](https://github.com/move-language/move/blob/main/language/changes/4-unit-testing.md#testing-annotations-their-meaning-and-usage)

### 依赖库

- 每次运行前都需要检查，[点击查看细节](https://move-language.github.io/move/packages.html#movetoml)，依赖可以从github上获取，不一定要存储在本地（本案例中）
