Google Sign-In for Rust
=======================
*The crates.io version is old, and I'm not sure why it hasn't been updated.*

Rust API bindings for Google Sign-in.  
See [authenticating with a backend server](https://developers.google.com/identity/sign-in/web/backend-auth).

## Usage
Put this in your `Cargo.toml`:

```toml
[dependencies]
google-signin = { git = "https://github.com/abhizer/google-signin-rs" }
```

And then you can verify a google JSON web token

```rust
use google_signin;
let mut client = google_signin::Client::new();
let mut cached_certs = google_signin::CachedCerts::new(); 
client.audiences.push(YOUR_CLIENT_ID); // required
client.hosted_domains.push(YOUR_HOSTED_DOMAIN); // optional

let token = TOKEN_TO_VERIFY; 

// Refresh the cached certs if required
match cached_certs.refresh_if_needed().await {
    Err(e) => eprintln!("{e:?}"), 
    Ok(x) => println!("Were cached certs refreshed? {x:?}"); 
};

let x = client.verify(token, &cached_certs).await.unwrap(); 

eprintln!("{x:?}"); 
```

Certs are only fetched if they've expired and the token is verified locally. 

*This wasn't working for me with HTTP2 so I stopped using it and got it working for me.*
