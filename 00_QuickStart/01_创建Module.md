# 第1节：创建module

本节将介绍move工程，并编写&编译第一个move合约。

## Code

创建文件夹BasicCoin，并在里面创建文件FirstModule.move

```rust
// 1. 定义一个module，只能发布在地址0xCAFE地址之下
module 0xCAFE::BasicCoin {
  	// 2. 定义一个Coin
    struct Coin has key {
        value: u64,
    }

  	// 3.定义mint函数，给account发型value数量多coin
    public fun mint(account: signer, value: u64) {
        move_to(&account, Coin { value })
    }
}
```

创建Move.toml，填写如下内容：

```toml
[package]
name = "BasicCoin"
version = "0.0.0"
```

运行：

```sh
move build
```

此时会创建一个build文件夹，相关信息在里面，我们在下节运行！

![image-20221115153809129](https://duke-typora.s3.ap-southeast-1.amazonaws.com/uPic/image-20221115153809129.png)



## Note

### Struct

结构体Struct有四种特性：

- `copy`: Allows values of types with this ability to be copied.
- `drop`: Allows values of types with this ability to be popped/dropped.
- `store`: Allows values of types with this ability to exist inside a struct in global storage.
- **key**: Allows the type to serve as a key for global storage operations.

### Storage operator

- **move_to**
- move_from
- borrow_global_mut
- borrow_global
- **exist**

### move new

- move new <module_name> 可以创建空module

### functions

- 默认是private的，可以声明为public