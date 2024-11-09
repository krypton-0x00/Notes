how long it takes cargo to rebuild our binary after having made a small change to the source code

- A sizable chunk of time is spent in the linking phase - assembling the actual binary given the outputs of the earlier compilation stages. 
- The default linker does a good job, but there are faster alternatives depending on the operating system you are using:
- `lld` on Windows and Linux, a linker developed by the LLVM project; 
- `zld` on MacOS.
# .cargo/config.toml 
# On Windows # ``` 
#cargo install -f cargo-binutils 
#rustup component add llvm-tools-preview 
###### ```[target.x86_64-pc-windows-msvc]
rustflags = ["-C", "link-arg=-fuse-ld=lld"] [target.x86_64-pc-windows-gnu] 
rustflags = ["-C", "link-arg=-fuse-ld=lld"] 
# On Linux: 
 sudo pacman -S lld clang`
[target.x86_64-unknown-linux-gnu] rustflags = ["-C", "linker=clang", "-C", "link-arg=-fuse-ld=lld"]
# On MacOS, 
`brew install michaeleisel/zld/zld` 
[target.x86_64-apple-darwin] 
rustflags = ["-C", "link-arg=-fuse-ld=/usr/local/bin/zld"] [target.aarch64-apple-darwin] 
rustflags = ["-C", "link-arg=-fuse-ld=/usr/local/bin/zld"]