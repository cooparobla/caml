# CAML (`caml`)

`caml` is a high-performance C++ library and command-line utility for encoding `.yaml` files into compressed, encrypted `.caml` binary files (and decoding them back).

## Features
- **Compressed & Encrypted Storage**: Uses **Zstandard (Zstd)** for compression and **AES-256-GCM (AEAD)** for secure, authenticated encryption.
- **Interoperable**: 100% binary format compatibility with `pycaml` (Python).
- **YAML Map Wrapper**: Provides `caml::CAMLMap` wrapping `fkYAML` for C++ data parsing and direct `.caml` file persistence.

## System Dependencies
- C++20 compatible compiler (GCC / Clang)
- CMake >= 3.16
- `libzstd-dev`
- `libssl-dev` (OpenSSL)

Install dependencies on Ubuntu/Debian:
```bash
sudo apt-get update
sudo apt-get install -y build-essential cmake libzstd-dev libssl-dev
```

## Build & Install
```bash
mkdir build && cd build
cmake ..
make -j$(nproc)
```

## Command Line Interface (CLI)

### Encode YAML to CAML
```bash
./caml encode config.yaml config.caml --passphrase "mysecretkey"
```

### Decode CAML to YAML
```bash
./caml decode config.caml output.yaml --passphrase "mysecretkey"
```

## C++ API Usage Example

```cpp
#include <caml/caml.h>
#include <iostream>

int main() {
    // Load existing YAML or create CAMLMap
    caml::CAMLMap config = caml::CAMLMap::load_yaml("config.yaml");

    // Save as encrypted & compressed .caml
    config.save_caml("config.caml", "my_passphrase");

    // Read back from encrypted .caml
    caml::CAMLMap loaded = caml::CAMLMap::load_caml("config.caml", "my_passphrase");
    std::cout << loaded.to_yaml_string() << std::endl;

    return 0;
}
```