# Slint C++ Template

A template for a C++ application that's using [Slint](https://slint.dev) for the user interface and CMake for the build system.

## About

This template helps you get started developing a C++ application with Slint as toolkit
for the user interface. It demonstrates the integration between the `.slint` UI markup and
C++ code, how to trigger react to callbacks, get and set properties and use basic widgets.

## Prerequisites

In order to use this template and build a C++ application, you need to install a few tools:

  * **[cmake](https://cmake.org/download/)** (3.21 or newer)
  * A C++ compiler that supports C++ 20 

If your target environment is Linux or Windows on an x86-64 architecture, then you may also opt into downloading one of our binary Slint packages. These are pre-compiled and require no further tools. You can find setup instructions and download links at

<https://slint.dev/docs/cpp/cmake.html#install-binary-packages>

Alternatively, this template will automatically download the Slint sources and compile them. This option requires you to install Rust by following the [Rust Getting Started Guide](https://www.rust-lang.org/learn/get-started). Once this is done, you should have the ```rustc``` compiler and the ```cargo``` build system installed in your path.

## Usage

1. Clone or download this repository
    ```
    git clone https://github.com/slint-ui/slint-cpp-template my-project
    cd my-project
    ```
2. Configure with CMake
   ```
   mkdir build
   cmake -B build
   ```
3. Build with CMake
   ```
   cmake --build build
   ```
4. Run the application binary
    * Linux/macOS:
        ```
        ./build/my_application
        ```
    * Windows:
        ```
        build\my_application.exe
        ```

We recommend using an IDE for development, along with our [LSP-based IDE integration for `.slint` files](https://github.com/slint-ui/slint/blob/master/tools/lsp/README.md). You can also load this project directly in [Visual Studio Code](https://code.visualstudio.com) and install our [Slint extension](https://marketplace.visualstudio.com/items?itemName=Slint.slint).

## Next Steps

We hope that this template helps you get started and you enjoy exploring making user interfaces with Slint. To learn more
about the Slint APIs and the `.slint` markup language check out our [online documentation](https://slint.dev/docs/cpp/).

Don't forget to edit this README to replace it by yours
