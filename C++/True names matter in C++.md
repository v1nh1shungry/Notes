## ADL

```cpp
namespace N1 {
    struct A {};
    int f(A) { return 1; }
}

namespae N2 {
    using B = N1::A;
    int f(B) { return 2; }
}

int main() {
    N2::B b;
    return f(b);
}
```

**返回 1**，因为 `b` 的实际类型是 `N1::A`，根据 ADL，unqualified 调用会在参数类型的命名空间里查找，也就是 `N1`。

### 坑点

如果将

```cpp
namespace my {
    class String {
        // ...
    };
}
```

替换成

```cpp
namespace my {
    using String = std::string;
}
```

将影响 `my::String` 的 ADL 查询的命名空间。

## References

* [True names matter in C++](https://quuxplusone.github.io/blog/2025/08/01/true-names/)
