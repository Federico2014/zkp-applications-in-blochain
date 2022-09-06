# Tutorial

For this tutorial, we are going to fuzz the URL parser [rust-url][]. Our goal here is to find some input generated by the fuzzer such that, when passed to [`Url::parse`], it causes some sort of panic or crash to happen.

## Create a fuzz target

The first thing we’ll do is create a *fuzz target* in the form of a Rust binary crate. AFL will call the resulting binary, supplying generated bytes to standard input that we’ll pass to `Url::parse`.

```sh
cargo new --bin url-fuzz-target
cd url-fuzz-target
```

We’ll need two dependencies in this crate:

* `url`: the crate we’re fuzzing
* `afl`: not required, but includes a couple utility functions to assist in creating fuzz targets

So add these to the `Cargo.toml` file:

```toml
[dependencies]
afl = "*"
url = "*"
```

Now we’ll need to write the source for the fuzz target in `src/main.rs`:

```rust,ignore
#[macro_use]
extern crate afl;
extern crate url;

fn main() {
    fuzz!(|data: &[u8]| {
        if let Ok(s) = std::str::from_utf8(data) {
            let _ = url::Url::parse(&s);
        }
    });
}
```

[`fuzz!`] is a utility macro provided by the `afl` crate that reads bytes from standard input and
passes the bytes to the provided closure.

In the body of the closure, we call `Url::parse` with the bytes that AFL generated. If all goes well, `url::Url::parse` will return an `Ok` containing a valid `Url`, or an `Err` indicating a `Url` could not be constructed from the `String`. If `Url::parse` panics while parsing the `String`, AFL will treat it as a crash and the AFL UI will indicate as such.

One important detail about the [`fuzz!`] macro: if a panic occurs within the body of the closure, the panic will be [caught][`panic::catch_unwind`] and [`process::abort`] will be subsequently called. Without the call to [`process::abort`], AFL would not consider the unwinding panic to be a crash.

[`fuzz!`]: https://docs.rs/afl/*/afl/macro.fuzz.html
[`process::abort`]: https://doc.rust-lang.org/std/process/fn.abort.html
[`panic::catch_unwind`]: https://doc.rust-lang.org/std/panic/fn.catch_unwind.html

## Build the fuzz target

Normally, one uses `cargo build` to compile a Cargo-based Rust project. To get AFL to work with Rust, a few extra compiler flags need to be passed to rustc during the build process. To make this easier, there is an AFL cargo subcommand (provided by the `afl` crate) that automatically passes these rustc flags for us. To use it, you’ll do something like:

```sh
cargo afl <cargo command>
```

Since we want to build this crate, we’ll run:

```sh
cargo afl build
```

## Provide starting inputs

AFL doesn't strictly require starting inputs, but providing some can make AFL’s job easier since it won’t need to ‘learn’ what a valid URL looks like. To do this, we'll create a directory called `in` with a few files (filenames don’t matter) containing valid URLs:

```sh
mkdir in
echo "tcp://example.com/" > in/url
echo "ssh://192.168.1.1" > in/url2
echo "http://www.example.com:80/foo?hi=bar" > in/url3
```

## Start fuzzing

To begin fuzzing, we’ll run:

```sh
cargo afl fuzz -i in -o out target/debug/url-fuzz-target
```

The `fuzz` subcommand of `cargo-afl` is the primary interface for fuzzing Rust code with AFL. For those already familiar with AFL, the `fuzz` subcommand of `cargo-afl` is identical to running `afl-fuzz`.

The `-i` flag specifies a directory full of input files AFL will use as seeds.

The `-o` flag specifies a directory AFL will write all its state and results to.

The last argument `target/debug/url-fuzz-target` specifies the fuzz target binary AFL will call, supplying random bytes to standard input.

As soon as you run this command, you should see AFL’s interface start up:

![](https://raw.githubusercontent.com/rust-fuzz/afl.rs/master/etc/screencap.gif)

For more information about this UI and what each of the sections mean, see [this resource hosted on the AFL website](http://lcamtuf.coredump.cx/afl/status_screen.txt).

AFL will run indefinitely, so if you want to quit, press `CTRL-C`.

[`Url::parse`]: https://docs.rs/url/*/url/struct.Url.html#method.parse
[rust-url]: https://github.com/servo/rust-url