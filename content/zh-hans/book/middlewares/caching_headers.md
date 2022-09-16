---
title: "Caching Headers"
weight: 8011
menu:
  book:
    parent: "middlewares"
---


```rust
use salvo::prelude::*;

#[handler]
async fn hello_world() -> &'static str {
    "Hello World"
}

#[tokio::main]
async fn main() {
    tracing_subscriber::fmt().init();

    tracing::info!("Listening on http://127.0.0.1:7878");
    // Compression must be before CachingHeader.
    let router = Router::with_hoop(CachingHeaders::new())
        .hoop(Compression::new().with_min_length(0))
        .get(hello_world);
    Server::new(TcpListener::bind("127.0.0.1:7878")).serve(router).await;
}
```