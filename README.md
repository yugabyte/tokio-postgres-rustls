# yb-postgres-rustls

TLS support for the [YugabyteDB `yb-tokio-postgres` smart driver][yb-driver] via the
[rustls TLS stack](https://github.com/rustls/rustls).

This is a fork of [`tokio-postgres-rustls`](https://github.com/jbg/tokio-postgres-rustls)
by Jasper Hugo, adapted to depend on `yb-tokio-postgres` instead of upstream
`tokio-postgres`. It provides a pure-Rust alternative to `yb-postgres-openssl`.

[![Crate](https://img.shields.io/crates/v/yb-postgres-rustls.svg)](https://crates.io/crates/yb-postgres-rustls)

[yb-driver]: https://docs.yugabyte.com/stable/drivers-orms/rust/yb-rust-postgres

# Features

This crate has no default features. Enable the rustls crypto provider that your
application uses, either `ring` or `aws-lc-rs` — otherwise `rustls::ClientConfig::builder()`
will panic at runtime because no process-level `CryptoProvider` is installed. The
optional `webpki-roots` and `native-certs` features add convenience constructors
(`MakeRustlsConnect::with_webpki_roots()` / `with_native_roots()`) for common root stores.

Requires Rust 1.85 or newer (edition 2024).

# Example

```no_run
# async fn run() -> Result<(), Box<dyn std::error::Error>> {
let config = rustls::ClientConfig::builder()
    .with_root_certificates(rustls::RootCertStore::empty())
    .with_no_client_auth();
let tls = yb_postgres_rustls::MakeRustlsConnect::new(config);
let connect_fut = yb_tokio_postgres::connect("sslmode=require host=localhost user=postgres", tls);
// ...
# Ok(())
# }
```

# License

`yb-postgres-rustls` is distributed under the MIT license, the same as its
upstream `tokio-postgres-rustls`.
