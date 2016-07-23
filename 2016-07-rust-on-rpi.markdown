Based on http://blog.thiago.me/raspberry-pi-bare-metal-programming-with-rust/

## Install rust nightly to get `lang_items` feature ##

    curl -sSf https://static.rust-lang.org/rustup.sh | sh -s -- --channel=nightly
    rustc --version
    rustc 1.12.0-nightly (936bfea94 2016-07-20)
    cargo --version
    cargo 0.13.0-nightly (664125b 2016-07-19)

## Build libcore for RPi architecture ##

    git clone https://github.com/rust-lang/rust.git
    cd rust
    git checkout 936bfea94
    sudo mkdir -p /usr/local/lib/rustlib/arm-unknown-linux-gnueabihf/lib
    sudo rustc --target arm-unknown-linux-gnueabihf -O -Z no-landing-pads src/libcore/lib.rs --out-dir /usr/local/lib/rustlib/arm-unknown-linux-gnueabihf/lib

## Create new rust project ##

    cargo new hello_world --bin

## src/main.rs for new project ##

    #![feature(lang_items)]
    #![crate_type = "staticlib"]
    #![no_std]
    
    pub extern fn main() {
        //println!("Ain't nobody got time for that!");
        loop {}
    }
    
    #[lang = "eh_personality"] extern fn eh_personality() {}
    #[lang = "panic_fmt"] extern fn panic_fmt() {}

## Build ##

    rustc --target arm-unknown-linux-gnueabihf -O --emit=obj src/main.rs
    file main.o
    main.o: ELF 32-bit LSB relocatable, ARM, version 1 (SYSV), not stripped
