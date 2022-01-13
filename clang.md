# C

## Pointers

```c
#include <stdio.h>

int
main(void)
{
	int i[2];
	int *j;

	/* Pointers j and i have the same address */
	j = i;
	/* Initialise destination of pointer */
	*j = 0;
	printf("Address: j = %p\n", (void *)j); /* n */
	printf("Value: *j = %d\n", *j);		/* 0 */

	/* Pointer moves right */
	j++;
	/* Initialise destination of pointer */
	*j = 0;
	/* Increments destination of pointer */
	(*j)++;

	printf("Address: j = %p\n", (void *)j); /* n+1 */
	printf("Value: *j = %d\n", *j);		/* 1 */

	return (0);
}
```

## Rules for safety critical code

1. Do not use goto statements, setjmp or longjmp constructs, and do not use
direct or indirect recursion.

2. Give all loops a fixed upper-bound (asserts).

3. Allow for memory allocation calls in only function calls with names ending
in the suffix "\_init". Typically, there is only one such function per process,
and it is executed only once after a system reset or reboot.

4. Limit functions to no more than N lines of text. N can be set to any
reasonable value in the range 50..100 lines.

5. Use minimally N assertions for every function of more than M lines. For
safety critical code, typical values for N and M are N=2 and M=20.

```c
if (!c_assert(p >= 0)) {
		return ERROR;
	}
```

with the assertion defined as follows:

```c
#define c_assert(e)	((e) ? (true) : \
	(tst_debugging("%s,%d: assertion '%s' failed\n", \
	__FILE__, __LINE__, #e), false))
```

6. Declare data objects at the smallest possible level of scope.

7. Check the return value of all non-void functions, and check the validity of
all function parameters.

8. Check the return value of all non-void functions, and check the validity of
all function parameters.

9. Limit the use of the preprocessor to file inclusion and simple macros.

10. The limit of two derefence operations per expression means that there should
be no more than two star operators to the left or to the right of an assignment
operator, or in any single conditional expression.

## When to use `malloc()`

In general, use `malloc()` when:

-   the array is too large to be placed on the stack
-   the lifetime of the array must outlive the scope where it is created

Otherwise, use a stack allocated array.

## Size of an array

```c
char str[] = "abc"; // str has type char[4] and holds 'a', 'b', 'c', '\0'

char str[3] = "abc"; // str has type char[3] and holds 'a', 'b', 'c'

char *str = malloc(4 * sizeof(char))
```

## `malloc` and strings

```c
#include <stdio.h>
#include <stdlib.h>

int
main(void)
{
	char *word = (char *)malloc(4 * sizeof(char));

	if (!word) {
		return (1);
	}

	word[0] = 'a';
	word[1] = 'b';
	word[2] = 'c';
	word[3] = '\0';

	int i;
	for (i = 0; i < 4; i++) {
		printf("\'%c\' ", word[i]);

		if (word[i] == '\0') {
			printf("(null character)\n");
		} else {
			printf("(NON-null character)\n");
		}
	}
	// 'a' (NON-null character)
	// 'b' (NON-null character)
	// 'c' (NON-null character)
	// '' (null character)

	free(word);

	return (0);
}
```

## Bools in C99
```c
_Bool flag;
```

or 

```c
#include <stdbool.h>

bool flag;
flag = true;
```