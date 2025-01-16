# C Interview Questions

### • What is the output of the following program?

```c
#include <stdio.h>
#include <stdint.h>

int main()
{
    int arr[5] = {0};

    memset(arr, 1, sizeof(arr));

    for (int i = 0; i < sizeof(arr)/sizeof(arr[0]); i++)
    {
        printf("%d ", arr[i]);
    }

    return 0;
}
```

<details> <summary><b>Answer</b></summary>

The output of the program is `16843009 16843009 16843009 16843009 16843009`.

**Explanation:** The `memset` function sets each byte of the array `arr` to the value `1`. Since `int32_t` is a 4-byte type, each element of the array will have all its bytes set to `1`:

```c
0x01010101 in hexadecimal (4 bytes)
```

When this value is interpreted as an integer, it is equal to `16843009` in decimal.

</details>

---

### • Can you define a function inside another function in C?

```c
#include <stdio.h>

void outer_function()
{
    void inner_function()
    {
        printf("Inner function called\n");
    }

    inner_function(); /* Function call */
}
```

<details> <summary><b>Answer</b></summary>

- **Standard C** does not allow nested function definitions.

- However, you can declare a function inside a function, but this is not the same as defining it. A declaration simply tells the compiler about the function's existence and signature, while the actual definition must occur at file scope.

```c
#include <stdio.h>

void outer_function()
{
    void inner_function(); /* Function declaration */

    inner_function(); /* Function call */
}

void inner_function()
{
    printf("Inner function called\n");
}

int main()
{
    outer_function();
    return 0;
}
```

**- GCC Language Extension:** GCC provides a language extension that supports nested functions. These functions are nonstandard, meaning they are not portable and are entirely compiler-dependent.

```c
#include <stdio.h>

void outer_function()
{
    void inner_function()
    {
        printf("Inner function called\n");
    }

    inner_function(); /* Function call */
}

int main()
{
    outer_function();
    return 0;
}
```

</details>

---

### • What is the output of the follow program?

```c
#include <stdio.h>

int main()
{
    int i = 0;
    int j = i++ + ++i;
    printf("%d\n", j);
    return 0;
}
```

<details> <summary><b>Answer</b></summary>

The output of the program is undefined behavior.

1. **Order of evaluation:**
    - In the statement `int j = i++ + ++i;`, there is no sequence point between `i++` and `++i`.
    - The order in which `i++` (post-increment) and `++i` (pre-increment) are evaluated is unspecified.
    - This leads to a conflict because `i` is being modified more than once without an intervening sequence point.
    - See: [C Operator Precedence](https://en.cppreference.com/w/c/language/operator_precedence)
    - See: [Order of evaluation](https://en.cppreference.com/w/c/language/eval_order)
2. **Undefined behavior:**
    - Modifying the same variable (`i`) multiple times without a sequence point (or a clear order of evaluation) causes undefined behavior in C.
    - The compiler may generate different results depending on how it evaluates the expression.
    - See: [Undefined Behavior in C](https://en.cppreference.com/w/c/language/behavior)

</details>

### • What is the output of the follow program?

```c

#include <stdio.h>

int main()
{
    int x = 5;
    size_t sz = sizeof(x++);
    int y = ++x;

    printf("x = %d\n", x);
    printf("y = %d\n", y);
}

```

<details> <summary><b>Answer</b></summary>

The program behaves as follows:

1. `int x = 5;` initializes `x` to 5.
2. `size_t sz = sizeof(x++);` evaluates the size of `x` without incrementing it because `sizeof` does not evaluate its operand. `x` remains 5, and `sz` holds the size of `x` (typically 4 bytes).
3. `int y = ++x;` increments `x` to 6 before assigning it to `y`. So, both `x` and `y` become 6.
4. The program prints: x = 6 y = 6

### Key Points:
- The sizeof operator yields the size (in bytes) of its operand, which may be an expression or the parenthesized name of a type. The size is determined from the type of the operand. The result is an integer. If the type of the operand is a variable length array type, the operand is evaluated; otherwise, the operand is not evaluated and the result is an integer constant, so  `sizeof(x++)` does not increment `x`.

</details>

### • What is the output of the follow program?

```c
#include <stdio.h>

int main() {
    int x = 10;
    int y = sizeof(int [x++]);
    printf("%d %d ", x, y);
    return 0;
}
```
<details> <summary><b>Answer</b></summary>
    
The program behaves as follows:

1. `int x = 10;` initializes `x` to10.
2. `int y = sizeof(int [x++]);` The memory occupied by an int[10] array is the product of the number of elements and the value of sizeof(int). (Note: The value of sizeof(int) may vary depending on the platform. However, in common systems, it is considered to be 4 bytes.)
3.  `int y = sizeof(int [x++]);` Normally, the operand of the sizeof operator is not evaluated (it does not have any side effects), but note that when dealing with VLA types, the expression determining the array's size (in this case, `x++`) is evaluated to compute the array's size.
4. The program prints: x = 11 y = 40


</details>
