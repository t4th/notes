# rurst

## install
```
$ curl --proto '=https' --tlsv1.3 https://sh.rustup.rs -sSf | sh
```

## cargo
cargo is default rust packet manager
it list dependancies, update them and allow to lock the versions

1. new project
cargo new project_name // create new project with *.toml configuration and git repo (if not already a repo)

2. build (call from within project_name directory)
cargo build             // build debug
cargo build --release   // build release
cargo run               // build and run
cargo clean             // clean targets
cargo check             // check for errors without building exec

3. doc
cargo doc --open 	// build doc
