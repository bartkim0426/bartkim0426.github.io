---
layout: post 
title:  "Rust: rust guess_number 예제"
categories: rust
---


```rust
extern crate rand;

use std::io;
use std::cmp::Ordering;
use rand::Rng;

fn main() {
    println!("Guess number game start");

    let secret_number = rand::thread_rng().gen_range(1, 101);

    loop {
        println!("Guess your number: ");

        let mut guess = String::new();

        io::stdin().read_line(&mut guess)
            .expect("Failed to read line");

        let guess: u32 = match guess.trim().parse() {
            Ok(num) => num,
            Err(_) => {
                println!("Please write number only");
                continue;
            }
        };

        match guess.cmp(&secret_number) {
            Ordering::Less => println!("Too small"),
            Ordering::Greater => println!("Too big"),
            Ordering::Equal => {
                println!("You WIN!");
                break;
            }
        };

    }
}
```


- `let guess: u32 = ...`와 같이 `u32`, `u64` 등의 타입으로 변환
- `read_line()`, `trim().parse()`와 같이 exception handling이 필요한 경우 `expect`를 필수로 써주어야 컴파일 에러가 나지 않음
- `match`를 사용해서 `Ok`, `Err` 결과를 반환 (`Result` 타입)하는 방식으로 사용 가능
- `cargo doc --open` 명령어로 사용된 library의 문서를 확인 가능
