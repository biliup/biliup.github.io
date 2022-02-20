# API调用
本项目使用 Rust , 可以作为 lib 被调用
### Rust
```rust
Studio::builder()
    .desc("desc")
    .dtime("dtime")
    .copyright("copyright")
    .cover("cover")
    .dynamic("dynamic")
    .source("source")
    .tag("tag")
    .tid("tid")
    .title("title")
    .videos("videos")
    .build()
    .submit(&Client::new().login_by_cookies("file").await?);
```
### Python
可以通过 [PyO3](https://github.com/PyO3/pyo3) 
导出接口给 Python 调用。
### Node.js
可以通过 [napi-rs](https://github.com/napi-rs/napi-rs)
导出接口给 Node.js 调用。

___
如果你有非Rust语言调用的需求，可以提一个issue
