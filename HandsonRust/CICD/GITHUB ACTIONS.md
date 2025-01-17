# audit.yml
```yml
name: Security audit
on:
  schedule:
    - cron: '0 0 * * *'
  push:
    paths:
      - '**/Cargo.toml'
      - '**/Cargo.lock'
jobs:
  security_audit:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: taiki-e/install-action@cargo-deny
      - name: Scan for vulnerabilities
        run: cargo deny check advisories
```

# general.yml
```yml
name: Rust

on: [push, pull_request]

env:
  CARGO_TERM_COLOR: always

jobs:
  test:
    name: Test
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: dtolnay/rust-toolchain@stable
      - uses: Swatinem/rust-cache@v2
      - name: Run tests
        run: cargo test

  fmt:
    name: Rustfmt
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: dtolnay/rust-toolchain@stable
        with:
          components: rustfmt
      - name: Enforce formatting
        run: cargo fmt --check

  clippy:
    name: Clippy
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: dtolnay/rust-toolchain@stable
        with:
          components: clippy
      - uses: Swatinem/rust-cache@v2
      - name: Linting
        run: cargo clippy -- -D warnings

  coverage:
    name: Code coverage
    runs-on: ubuntu-latest
    container:
      image: xd009642/tarpaulin
      options: --security-opt seccomp=unconfined
    steps:
      - name: Checkout repository
        uses: actions/checkout@v3
      - name: Generate code coverage
        run: |
          cargo tarpaulin --verbose --workspace
```