# trackalloc

Trackalloc is a dead simple and willingly low overhead allocator for rust 
which allows you to track (and consult) the amount of memory that is being
allocated to your process as well as the *maximum* amount of memory that has
been allocated to your process over the course of its life.

It is a fork of [peak_alloc](https://crates.io/crates/peak_alloc) by Xavier Gillard that just updates it to Rust 2024 and adds a function that spawns a thread that will auto record the current memory usage every N nanoseconds. It also lets you choose your flavour of allocator instead of being tied to System. They did the hard work not me!

## Usage
In your `Cargo.toml`, you should add the following line to your dependencies
section.

```toml
[dependencies]
trackalloc = "0.4.0"
```

Then in your main code, you will simply use it as shown below:

```rust
use track_alloc::PeakAlloc;
use std::alloc::System;

#[global_allocator]
static PEAK_ALLOC: PeakAlloc<System> = PeakAlloc::system();

fn main() {
	// Do your funky stuff...

	let current_mem = PEAK_ALLOC.current_usage_as_mb();
	println!("This program currently uses {} MB of RAM.", current_mem);
	let peak_mem = PEAK_ALLOC.peak_usage_as_gb();
	println!("The max amount that was used {}", peak_mem);
}
```
