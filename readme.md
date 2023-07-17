# 使用说明

## 概述

这个 DLL 提供了名为 `SCCMain` 的函数，它可以执行一些字符串加密和解密操作。该函数接受以下参数：

- `chioce`：选择执行的操作模式，可取值为 1 或 2。
- `userInput`：用户输入的字符串。
- `passWordsub`：密码字符串。
- `outputBuffer`：用于接收函数执行结果的缓冲区。
- `bufferSize`：输出缓冲区的大小。

## 操作模式

这个 DLL 支持两种操作模式：

1. 模式一：加密字符串
   - 设置 `chioce` 参数为 1。
   - 将要加密的字符串作为 `userInput` 参数传递。
   - 设置加密所需的密码字符串为 `passWordsub` 参数传递。
   - 提供足够大的 `outputBuffer` 缓冲区来接收加密后的结果。

2. 模式二：解密字符串
   - 设置 `chioce` 参数为 2。
   - 将要解密的字符串作为 `userInput` 参数传递。
   - 设置解密所需的密码字符串为 `passWordsub` 参数传递。
   - 提供足够大的 `outputBuffer` 缓冲区来接收解密后的结果。

## 使用示例

以下是一个使用该 DLL 的示例代码，你可以参考它来集成 DLL 到你的项目中：

```cpp
#include <iostream>
#include <Windows.h>

// 声明要调用的函数类型
typedef void (*CallSCCMainFunc)(int, const wchar_t*, const wchar_t*, wchar_t*, int);

int main() {
    // 加载 DLL
    HMODULE dllHandle = LoadLibrary(L"your_dll_name.dll");
    if (dllHandle == nullptr) {
        std::cerr << "Failed to load DLL." << std::endl;
        return 1;
    }

    // 获取要调用的函数地址
    CallSCCMainFunc callSCCMain = reinterpret_cast<CallSCCMainFunc>(GetProcAddress(dllHandle, "CallSCCMain"));
    if (callSCCMain == nullptr) {
        std::cerr << "Failed to get function address." << std::endl;
        FreeLibrary(dllHandle);  // 卸载 DLL
        return 1;
    }

    // 准备输入参数和输出缓冲区
    int choice = 1;
    const wchar_t* userInput = L"Hello, world!";
    const wchar_t* password = L"MyPassword";
    wchar_t outputBuffer[256] = { 0 };  // 假设输出缓冲区大小为 256

    // 调用 DLL 中的函数
    callSCCMain(choice, userInput, password, outputBuffer, sizeof(outputBuffer) / sizeof(wchar_t));

    // 输出结果
    std::wcout << "Output: " << outputBuffer << std::endl;

    // 卸载 DLL
    FreeLibrary(dllHandle);

    return 0;
}
```
请将示例代码中的 your_dll_name.dll 替换为你的 DLL 文件名，并根据你的实际情况修改输入参数和输出缓冲区的大小。确保输入参数和输出缓冲区的类型与你的 DLL 函数声明一致。

## 注意事项

- 在使用 DLL 之前，请确保 DLL 文件与你的应用程序位于同一个目录下，或者将 DLL 文件放置在系统搜索路径中。
- 请根据你的实际需求提供足够大的输出缓冲区，以确保能够容纳函数执行结果。
- 如果遇到任何问题或错误，请参考错误消息并确保 DLL 文件正确加载。

