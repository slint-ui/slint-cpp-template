# SixtyFPS C++ Template

A template for a C++ application that's using SixtyFPS for the user interface and CMake for the build system.

## About

This template helps you get started developing a C++ application with SixtyFPS as toolkit
for the user interface. It demonstrates the integration between the `.60` UI markup and
C++ code, how to trigger react to callbacks, get and set properties and use basic widgets.

## Usage

1. Clone or download this repository
    ```
    git clone https://github.com/sixtyfpsui/sixtyfps-cpp-template my-project
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

We recommend using an IDE for development, along with our [LSP-based IDE integration for `.60` files](https://github.com/sixtyfpsui/sixtyfps/blob/master/tools/lsp/README.md). You can also load this project directly in [Visual Studio Code](https://code.visualstudio.com) and install our [SixtyFPS extension](https://marketplace.visualstudio.com/items?itemName=SixtyFPS.sixtyfps-vscode).

## Next Steps

We hope that this template helps you get started and you enjoy exploring making user interfaces with SixtyFPS. To learn more
about the SixtyFPS APIs and the `.60` markup language check out our [online documentation](https://sixtyfps.io/docs/cpp/).