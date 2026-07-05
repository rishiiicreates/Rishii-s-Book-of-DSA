# Input and Output (I/O) in C++

## What is I/O in C++?
- input and Output (I/O) operations allow a program to interact with the user or an external file. In C++, the standard I/O library `<iostream>` provides the objects `cin` (for standard input) and `cout` (for standard output).

- for Competitive Programming and Data Structures & Algorithms, I/O performance is critical. Reading $10^6$ integers natively can cause a Time Limit Exceeded (TLE) error if not optimized properly.


## Fast I/O Optimization

- by default, C++ streams (`cin`/`cout`) are synchronized with C streams (`scanf`/`printf`). This ensures that if you mix them, the output appears in the correct order. However, this synchronization adds massive overhead.

- to write the fastest possible I/O in C++, we always start our `main` function with these two lines:

```cpp
#include <iostream>
using namespace std;

int main() {
    // 1. Disable synchronization between C and C++ standard streams
    ios_base::sync_with_stdio(false);
    
    // 2. Untie cin from cout
    cin.tie(NULL);
    
    // Fast I/O logic goes here
    return 0;
}
```

### Why does this work?
- **`sync_with_stdio(false)`**: Tells the C++ compiler not to sync its streams with C's standard I/O. **Warning:** After doing this, you must *never* mix `cin/cout` with `scanf/printf`.
- **`cin.tie(NULL)`**: By default, `cin` is tied to `cout`. This means that before `cin` reads anything, it automatically flushes the `cout` buffer (so prompts like `"Enter value: "` appear immediately). By untying them, we avoid wasting time flushing the buffer automatically on every read.

> [!important]
> **Never use `endl`** in performance-critical code. `endl` not only inserts a newline but also forces a buffer flush. Flushing the buffer is an expensive OS-level operation. Instead, use `\n` to insert a newline. The buffer will flush itself automatically when it's full or when the program terminates.


## Common I/O Patterns

### Reading until End-Of-File (EOF)
- sometimes the number of inputs isn't given. You must read until the file ends.
```cpp
int x;
while (cin >> x) {
    // Process x
}
```
- *`cin >> x` evaluates to `true` as long as it successfully extracts an integer.*

### Reading a Full Line with Spaces
- if you use `cin >> s` for a string, it stops at the first space. To read an entire line including spaces, use `getline()`.
```cpp
#include <string>

string sentence;
getline(cin, sentence); // Reads the whole line until \n
```

### Handling the "Newline Buffer" Bug
- if you read an integer and then immediately use `getline()`, the `getline()` will instantly read the leftover newline character `\n` from the integer input and return an empty string.
```cpp
int n;
cin >> n;
// Fix: Ignore the leftover newline character in the buffer
cin.ignore(numeric_limits<streamsize>::max(), '\n'); 

string s;
getline(cin, s); 
```


## Complexity
- **time Complexity**: $O(1)$ per I/O operation. With fast I/O enabled, the speed is virtually identical to pure C's `scanf`/`printf`.
- **space Complexity**: $O(1)$ overhead to hold the parsed variable.

NEXT: [[Index]]
