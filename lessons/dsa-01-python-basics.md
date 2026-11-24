# DSA Lesson 1 (Python basics 1): running Python, variables, types, input/output, operators, if/else

**Why Python:** it is easy to read, it is used for DevOps scripting and automation, and it is accepted in nearly every campus coding test. You already have Python 3 inside your Ubuntu.

## 1. What Python is

Python is a **high-level, interpreted** language. *High-level* means it looks close to English. *Interpreted* means a program called the interpreter reads and runs your code line by line, with no separate compile step.

Check it: `python3 --version`.

## 2. Two ways to run Python

**A. Interactive mode (REPL)**: type `python3`, then type code and see results at once. Leave with `exit()` or Ctrl+D. Great for trying small things.

**B. Script file**: save code in a `.py` file and run it.

```
cd ~/devops-lab
mkdir python
cd python
nano hello.py
```
Type `print("Hello, Anita")` inside nano. Then **Ctrl+O** (save), **Enter** (confirm name), **Ctrl+X** (exit). Run it:
```
python3 hello.py
```

## 3. print()

```python
print("Hello", "World", sep="-")     # Hello-World
print("a", end=" "); print("b")      # a b
name = "Anita"; age = 20
print(f"{name} is {age}")            # Anita is 20
```
- `sep` sets what goes **between** items (default is a space).
- `end` sets what goes **after** the output (default is a new line).
- **f-string:** put `f` before the quotes and variables inside `{ }`.
- `#` starts a comment.

## 4. Variables

A variable is a **name that points to a value**. You do not declare a type. Python decides (**dynamic typing**).

```python
count = 5
price = 99.5
name = "Anita"
```
**Naming rules:** letters, digits and underscore; cannot start with a digit; case-sensitive (`Age` and `age` differ); cannot be a keyword (`if`, `for`, `class`...). **Style:** `snake_case` (`total_marks`).

**Multiple assignment and swap:**
```python
a, b = 3, 7
a, b = b, a        # swaps; now a is 7 and b is 3
```

## 5. Data types

| Type | Meaning | Example |
|---|---|---|
| `int` | Whole numbers (any size) | `7` |
| `float` | Decimal numbers | `7.5` |
| `str` | Text | `"hello"` |
| `bool` | `True` or `False` | `True` |
| `NoneType` | "Nothing" | `None` |

`type(x)` tells you the type: `type(7.0)` -> `<class 'float'>`.

**Conversion (casting):** `int("7") + 1` is `8`. `float("2.5") * 2` is `5.0`. `str(7) * 2` is `"77"`. `int(6.8)` is `6`: it **cuts off** the decimal part (it truncates toward zero, it does not round).

**Strings (first look):**
```python
s = "Anita"
s[0]       # 'A'  (first character; index starts at 0)
s[-1]      # 'a'  (last character)
s[1:4]     # 'nit' (from index 1 up to, but not including, 4)
len(s)     # 5
s.upper()  # 'ANITA'
s.lower()  # 'anita'
s.replace("a", "A")   # 'AnitA'
"ab" + "cd"  # 'abcd'  (joins)
"ab" * 2     # 'abab'  (repeats)
```

**Truthiness:** these are **False**: `0`, `0.0`, `""` (empty string), `None`, empty list. Everything else is **True**. `bool(0)` is `False`; `bool("hi")` is `True`. Also, `True` behaves like `1` and `False` like `0`.

## 6. input() always gives a string

```python
a = int(input("Enter first number: "))
b = int(input("Enter second number: "))
print("Sum =", a + b)
```
Typing `12` and `30` prints `Sum = 42`. **If you forget `int()`**, `"12" + "30"` becomes `"1230"`. This is the most common beginner bug.

## 7. Operators

| Operator | Meaning | Example |
|---|---|---|
| `+ - *` | add, subtract, multiply | `7 * 2` is `14` |
| `/` | true division, **always a float** | `7 / 2` is `3.5` |
| `//` | floor division (rounds **down**) | `7 // 2` is `3`; `-9 // 4` is `-3` |
| `%` | remainder (modulus) | `17 % 5` is `2`; `-9 % 4` is `3` |
| `**` | power | `2 ** 10` is `1024` |
| `== != < > <= >=` | comparisons, give `True`/`False` | `4 == 4.0` is `True` |
| `and or not` | logic | `5 > 3 and not 2 > 4` is `True` |
| `+= -= *= //= **= %=` | shortcut assignment | `x += 3` means `x = x + 3` |

**Important details**
- `//` rounds **toward minus infinity** (floor), so `-9 // 4` is `-3`, not `-2`.
- `%` takes the sign of the divisor: `-9 % 4` is `3` because `-9 = 4 x (-3) + 3`.
- `**` is **right-to-left**: `3 ** 2 ** 2` means `3 ** (2 ** 2)` = `3 ** 4` = `81`.
- Comparisons can be **chained**: `10 > 5 > 2` is `True`.
- Precedence, high to low: `**`, then `* / // %`, then `+ -`, then comparisons, then `not`, then `and`, then `or`. So `True or False and False` is `True`.
- **Floating-point surprise:** `0.1 + 0.7` gives `0.7999999999999999`. Computers store decimals in binary, so some fractions are slightly off. Round for display: `round(2.567, 2)` is `2.57`.
- **Python's rounding:** `round(0.5)` is `0` and `round(1.5)` is `2`. It rounds exact halves to the **nearest even** number ("banker's rounding").
- Useful built-ins: `abs(-4)` is `4`, `max(3, 9, 2)` is `9`, `min(3, 9, 2)` is `2`.

**Chained assignment check:** starting with `x = 10`: `x -= 3` (7), `x *= 2` (14), `x //= 4` (3), `x **= 2` (9), `x %= 5` gives **4**.

## 8. Decisions: if / elif / else

```python
marks = int(input("Marks: "))
if marks >= 90:
    grade = "A"
elif marks >= 75:
    grade = "B"
elif marks >= 50:
    grade = "C"
else:
    grade = "F"
print("Grade:", grade)
```
Entering `80` prints `Grade: B`.

**Indentation is part of the syntax.** Use 4 spaces. The colon `:` after the condition is required. Conditions are checked from the top; the **first** true branch runs and the rest are skipped.

**Largest of three numbers:**
```python
a = int(input("a: "))
b = int(input("b: "))
c = int(input("c: "))
if a >= b and a >= c:
    largest = a
elif b >= c:
    largest = b
else:
    largest = c
print("Largest =", largest)
```
Input `4`, `9`, `7` prints `Largest = 9`.

**Leap year** (divisible by 4, but century years must be divisible by 400):
```python
year = int(input("Year: "))
if (year % 4 == 0 and year % 100 != 0) or year % 400 == 0:
    print("Leap year")
else:
    print("Not a leap year")
```
Checked: `2024` -> Leap year; `2100` -> Not a leap year; `2000` -> Leap year; `2023` -> Not a leap year.

## 9. Common errors (and what they mean)

| Error | Cause | Example |
|---|---|---|
| `TypeError` | Mixing incompatible types | `"5" + 5` -> *can only concatenate str (not "int") to str* |
| `ValueError` | Right type, impossible value | `int("abc")` -> *invalid literal for int()* |
| `NameError` | Using a name that does not exist | `print(y)` -> *name 'y' is not defined* |
| `ZeroDivisionError` | Dividing by zero | `10 / 0` -> *division by zero* |
| `IndentationError` | Wrong or missing indentation | code after `if ...:` not indented |
| `SyntaxError` | Broken grammar (missing colon, quote, bracket) | `if x == 1` without `:` |

**How to read an error:** look at the **last line**. It names the error type and tells you why.

## 10. Question types that repeat in campus tests

> Honest note: companies change papers every year, and I can't see their current question banks. These are the patterns that appear again and again in the pseudocode and programming sections of TCS NQT, Infosys, Wipro, Accenture and Cognizant style tests.

1. **Output prediction (MCQ):** operator precedence, `/` vs `//`, string operations, type conversion, truthiness, floating-point. This is the **most repeated** type.
2. **Basic logic programs:** swap, even/odd, largest of three, leap year, simple interest, grade calculator.
3. **Next lessons (loops):** factorial, prime, palindrome, Fibonacci, sum of digits, patterns.

## 11. Expected practice

### A. Predict the output (paper first, then check in Python)

1. `print(10/3)`
2. `print(-7//2)`
3. `print(-7 % 3)`
4. `print(2**3**2)`
5. `print("5"+"5")` and `print("5"*3)`
6. `print(0.1+0.2)`
7. `print(round(2.5), round(3.5))`
8. `print(True + True)`
9. `print(3 == 3.0)`
10. `print(1 < 2 and 3 > 4 or 5 > 1)`
11. `print(int(-3.9))`
12. `print("Python"[1:4])`

### B. Write these programs (run them with `python3`)

1. Swap two numbers: first with a third variable, then the Python way.
2. Read a number and print whether it is even or odd.
3. Read three numbers and print the largest *(you have a model above; write it from memory)*.
4. Read a number and print whether it is positive, negative or zero.
5. Simple interest: read principal P, rate R, time T, print `P * R * T / 100`.
6. Convert Celsius to Fahrenheit: `F = C * 9 / 5 + 32`.
7. Read marks and print the grade (write the version with `>= 90`, `>= 75`, `>= 50`, from memory).
8. Read a letter and print whether it is a vowel or a consonant.
9. A calculator: read two numbers and an operator (`+ - * /`), print the result, and handle division by zero.
10. Read marks of 5 subjects, print the total and the percentage.

I'll check your answers, explain mistakes, and add the solutions here.
