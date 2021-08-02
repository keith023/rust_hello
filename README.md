# Rust Embedded Helloworld Program
This is a base hello world project using the Rust Programming Language to build a demo design to run on the QEMU.  This project was based on the Rust Embedded documentation and book listed online at www.rust-lang.org

## Tools
Tools Needed:
* ARM Embedded Toolchain (10-2020-Q4 Major) - https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-toolchain/gnu-rm
* Microsoft Build Tools (2019 Build Tools) - https://visualstudio.microsoft.com/thank-you-downloading-visual-studio/?sku=BuildTools&rel=16
* Rust (v1.54.0)- https://www.rust-lang.org/tools/install
* QEMU (v6.0.90) - https://www.qemu.org/download/#windows


## Tool Configuration
Prior to using the build tools, make sure to download the proper Rust Crates and ARM Targets shown below.

Adding the ARM target for this repo can be done using the following command for the ARM M3 Target:

	rustup target add thumbv7m-none-eabi
	
Rust will need the bin-utils crate and llvm tools preview component.  This can be done by running these commands:
	
	cargo install cargo-binutils
	rustup component add llvm-tools-preview

After installing QEMU above, you may need to add the QEMU install directory to your path.  Make sure you can run the application in the terminal:

	qemu-system-arm --version

## Building the application
To build the application, the cargo tool can be used with the toml configuration files to create the binary.  Just run the following command:
	
	cargo build

This will build the helloworld.rs application and generate the binary in the target/thumbv7m-none-eabi/debug/ folder.  You can verify the binary is correct by using the cargo obj-read:
	
	cargo readobj --bin helloworldapp -- -file-headers
	
Which should report the binary is an ELF32 file for the ARM machine.

## Running the application
To test the application, we can use QEMU to emulate the hardware enviroment and run the example design:
	
	qemu-system-arm -cpu cortex-m3 -machine lm3s6965evb -nographic -semihosting-config enable=on,target=native -kernel target/thumbv7m-none-eabi/debug/helloworldapp

OR use the cargo run command:
	
	cargo run
	
which uses the config.toml setup to run the same QEMU command.
	
This will run the helloworldapp built by the cargo build for the thumbv7m-non-eabi target on an emulated cortex m3 machine .  The following should be printed to verify it worked:

`"Hello D5 World!"`


	



