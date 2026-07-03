# LTL2BA-and-LTL2dstar-for-windows
The toolbox for convert LTL to the corresponding automata

For LTL2BA, the original source code is intended to be compiled on Linux. To build a Windows `.exe` file, 
replace


```c
#include <sys/resource.h>
```
with

```c
#ifndef _WIN32
#include <sys/resource.h>
#else
#include <sys/time.h>

struct rusage {
    struct timeval ru_utime;
    struct timeval ru_stime;
};

#define RUSAGE_SELF 0

static int getrusage(int who, struct rusage *usage)
{
    (void) who;
    usage->ru_utime.tv_sec = 0;
    usage->ru_utime.tv_usec = 0;
    usage->ru_stime.tv_sec = 0;
    usage->ru_stime.tv_usec = 0;
    return 0;
}
#endif
```

in line 37 of the file "\ltl2ba-1.3\ltl2ba.h".

To get rabin automata, create ltl file 

```bash
cmd /c "echo G F a>formula.ltl"
```

```bash
cmd /c "echo ^& ^& G F gather G F recharge G F upload>formula.ltl"
```

and run 


```bash
.\ltl2dstar.exe --ltl2nba=spin:.\ltl2ba.exe --automata=rabin --output-format=hoa formula.ltl rabin.hoa
```

To get graph:

```bash
 .\ltl2dstar.exe --ltl2nba=spin:.\ltl2ba.exe --automata=rabin --output-format=dot formula.ltl rabin.dot
```
```bash
& "C:\Program Files\Graphviz\bin\dot.exe" -Tpdf rabin.dot -o rabin.pdf
```
