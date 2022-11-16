# 第7节：使用prover

首先安装：

```sh
./scripts/dev_setup.sh -yp
Y
cargo build
source ~/.profile
```

[Move prover](https://github.com/move-language/move/blob/main/language/move-prover/doc/user/prover-guide.md)

## Code

在上一节的BasicCoin中增加如下代码：

```rust
    spec balance_of {
        pragma aborts_if_is_strict;
    }
```

执行：

```sh
move prove
```

报错：

![image-20221116184224594](https://duke-typora.s3.ap-southeast-1.amazonaws.com/uPic/image-20221116184224594.png)

修改代码：

```rust
    spec balance_of {
        pragma aborts_if_is_strict;
      	
      	// 增加这行代码
        aborts_if !exists<Balance<CoinType>>(owner);
    }
```

重新运行：

```sh
move prove
```

结果如下：

![image-20221116183826667](https://duke-typora.s3.ap-southeast-1.amazonaws.com/uPic/image-20221116183826667.png)



## Note

