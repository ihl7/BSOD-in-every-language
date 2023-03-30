# BSOD-in-every-language

"BSOD-in-every-language" is a collection of code snippets in various programming languages that can trigger a Windows BSOD. The repository is intended for educational and research purposes only and aims to showcase how programming languages can interact with the Windows operating system in unexpected ways.


## Codes

<details>
    <summary>C++ (click to expand/collapse)</summary>

```cpp
#include <iostream>
#include <Windows.h>
#include <winternl.h>
using namespace std;
typedef NTSTATUS(NTAPI *pdef_NtRaiseHardError)(NTSTATUS ErrorStatus, ULONG NumberOfParameters, ULONG UnicodeStringParameterMask OPTIONAL, PULONG_PTR Parameters, ULONG ResponseOption, PULONG Response);
typedef NTSTATUS(NTAPI *pdef_RtlAdjustPrivilege)(ULONG Privilege, BOOLEAN Enable, BOOLEAN CurrentThread, PBOOLEAN Enabled);
int main()
{
    BOOLEAN bEnabled;
    ULONG uResp;
    LPVOID lpFuncAddress = GetProcAddress(LoadLibraryA("ntdll.dll"), "RtlAdjustPrivilege");
    LPVOID lpFuncAddress2 = GetProcAddress(GetModuleHandle("ntdll.dll"), "NtRaiseHardError");
    pdef_RtlAdjustPrivilege NtCall = (pdef_RtlAdjustPrivilege)lpFuncAddress;
    pdef_NtRaiseHardError NtCall2 = (pdef_NtRaiseHardError)lpFuncAddress2;
    NTSTATUS NtRet = NtCall(19, TRUE, FALSE, &bEnabled); 
    NtCall2(STATUS_FLOAT_MULTIPLE_FAULTS, 0, 0, 0, 6, &uResp); 
    return 0;
}
```

</details>

<details>
    <summary>C# (click to expand/collapse)</summary>

```cs
System.Diagnostics.Process.GetProcessesByName("csrss")[0].Kill();
```

</details>
    
    
    <details>
    <summary>Python (click to expand/collapse)</summary>

```py
import ctypes
ntdll = ctypes.windll.ntdll
prev_value = ctypes.c_bool()
res = ctypes.c_ulong()
ntdll.RtlAdjustPrivilege(19, True, False, ctypes.byref(prev_value))
if not ntdll.NtRaiseHardError(0xDEADDEAD, 0, 0, 0, 6, ctypes.byref(res)):
    print("BSOD Successfull!")
else:
    print("BSOD Failed...")
```

</details>
    
    
    
<details>
    <summary>Golang (click to expand/collapse)</summary>

```go
import "fmt"
import "syscall"

func main() {
    kernel32 := syscall.MustLoadDLL("kernel32.dll")
    ntRaiseHardError := kernel32.MustFindProc("RaiseHardError")
    var p uintptr
    var b [256]byte
    for i := 0; i < len(b); i++ {
        b[i] = byte(i)
    }
    ntRaiseHardError.Call(0xc0000022, 0, 0, uintptr(unsafe.Pointer(&b[0])))
    fmt.Println("You should never see this message")
}
```

</details>
    
    
    
    
    
## Usage

**WARNING: Running this code on your computer can cause a Blue Screen of Death (BSOD) and result in data loss. Use at your own risk.**

To test the code snippets in this repository, you will need a Windows computer. Simply copy and paste the code into a file with the appropriate file extension for the programming language you are using, and run the code on your computer.

## Contributing

Contributions to this repository are welcome! If you have code snippets that can trigger a BSOD in a programming language that is not currently represented in the repository, please feel free to submit a pull request.

## License

This repository is licensed under the [MIT License](LICENSE).
