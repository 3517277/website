---
title: "Proxy"
weight: 3050
menu:
  book:
    parent: "middlewares"
---


```rust
use salvo::prelude::*;
use salvo_extra::proxy::ProxyHandler;

#[tokio::main]
async fn main() {
    tracing_subscriber::fmt().init();
    
    let router = Router::new()
        .push(
            Router::new()
                .path("google/<**rest>")
                .handle(ProxyHandler::new(vec!["https://www.google.com".into()])),
        )
        .push(
            Router::new()
                .path("baidu/<**rest>")
                .handle(ProxyHandler::new(vec!["https://www.baidu.com".into()])),
        );
    Server::new(TcpListener::bind("127.0.0.1:7878")).serve(router).await
}
```