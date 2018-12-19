---
layout: post 
title:  "rust: Mac OS X에서 rust 설치하기"
categories: rust
---

다음 명령어로 `rustup`, `cargo`, `rustc`를 간단하게 설치 가능하다.

```
curl https://sh.rustup.rs -sSf | sh
```


이후 `rustc -v`로 설치가 되어있는것을 확인할 수 있다.


```
$rustc -v

rustup 1.15.0 (f0a3d9a84 2018-11-27)
The Rust toolchain installer

USAGE:
    rustup [FLAGS] <SUBCOMMAND>

FLAGS:
    -v, --verbose    Enable verbose output
    -h, --help       Prints help information
    -V, --version    Prints version information

SUBCOMMANDS:
    show           Show the active and installed toolchains
    update         Update Rust toolchains and rustup
    default        Set the default toolchain
    toolchain      Modify or query the installed toolchains
    target         Modify a toolchain's supported targets
    component      Modify a toolchain's installed components
    override       Modify directory toolchain overrides
    run            Run a command with an environment configured for a given toolchain
    which          Display which binary will be run for a given command
    doc            Open the documentation for the current toolchain
    man            View the man page for a given command
    self           Modify the rustup installation
    set            Alter rustup settings
    completions    Generate completion scripts for your shell
    help           Prints this message or the help of the given subcommand(s)

DISCUSSION:
    rustup installs The Rust Programming Language from the official
    release channels, enabling you to easily switch between stable,
    beta, and nightly compilers and keep them updated. It makes
    cross-compiling simpler with binary builds of the standard library
    for common platforms.

    If you are new to Rust consider running `rustup doc --book` to
    learn Rust.
```

rust는 `~/.cargo/bin`에 설치되는데, 만약 환경변수가 제대로 지정이 되어있지 않았다면 

```
source ~/.cargo/env
```

를 해주면 된다.


Rust로 개인 프로젝트를 하나 할 생각이다.


자세한 설명은 [Rust 문서](https://doc.rust-lang.org/book/2018-edition/ch01-01-installation.html)에서도 확인 가능하다.

> `rustup doc` 명령어로 local 파일의 docs를 이용할 수 있다.
