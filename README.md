# Actix Web Flash
Actix web flash is an unofficial crate to provide flash messages in servers using Actix web.

Flash messages are typically used to display errors on websites that are rendered server side.

* [Docs](https://docs.rs/actix-web-flash/0.1.0/actix-web-flash/)
* [Examples](examples/)

```rust
use actix_web::{http, server, App, HttpRequest, HttpResponse, Responder};
use actix_web_flash::{FlashMessage, FlashResponse, FlashMiddleware};

fn show_flash(flash: FlashMessage<String>) -> impl Responder {
    flash.into_inner()
}

fn set_flash(_req: &HttpRequest) -> FlashResponse<HttpResponse, String> {
    FlashResponse::new(
        Some("This is the message".to_owned()),
        HttpResponse::SeeOther()
            .header(http::header::LOCATION, "/show_flash")
            .finish(),
    )
}

fn main() {
    server::new(|| {
        App::new()
            .middleware(FlashMiddleware::default())
            .route("/show_flash", http::Method::GET, show_flash)
            .resource("/set_flash", |r| r.f(set_flash))
    }).bind("127.0.0.1:8080")
        .unwrap()
        .run();
}
```

## License

MIT/Apache-2.0
