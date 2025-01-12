# C++ Interview Questions

Consider the following code:

```cpp
#include <iostream>
#include <string>

void foo(std::string&& s)
{
    /* some code here */
    std::string s1 = s;
}

int main()
{
    std::string s1{100'000, 'K'};
    foo(std::move(s1));
    /* some code here */
    return 0;
}

```
#### • What does the call to foo(std::move(s1)) do in terms of ownership of the string data?
#### • Does s1's resource get "moved" or "stolen" in this case?

<details> <summary><b>Answer</b></summary>

### The Role of `std::move`:

The `std::move` function **does not perform any moving operation**. It simply casts its argument into an R-value reference, which allows the compiler to treat the argument as a temporary value. In this case, `std::move(s1)` makes `s1` an R-value, but it **does not transfer ownership** of the resources in `s1`.

---

### Does `s1`'s Resource Get "Moved" or "Stolen"?

In this case, **no resources are stolen or moved** because inside the function `func`, the string `s` is copied into the new local variable `s1`. The **copy constructor** of `std::string` is invoked because `s` is an r-value reference but is used as an l-value in the assignment (`std::string s1 = s;`), which triggers a **copy**, not a **move**.

---

### Why Is the Copy Constructor Called?

The reason the copy constructor is called is that `std::string s1 = s;` is an assignment where `s` (even though it's passed as an r-value reference) is copied into the local variable `s1`. The `std::move` only casts `s1` into an r-value to allow the possibility of moving, but the actual code in `func` is performing a **copy** because it uses the `std::string`'s **copy constructor** when creating `s1`.

</details>
