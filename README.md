# Reminders & concepts

## Quick access

Fast API setup : https://github.com/blueswen/fastapi-observability

## UNIX commands

### Files search

```bash
# Search for files
find . -name "*.py"                # Find all .py files
find / -type f -name "myfile.txt" # Search system-wide

# Search within files
grep "pattern" file.txt           # Basic search
grep -r "TODO" ./src              # Recursive
grep -rnw '.' -e "pattern"        # Exact match + line number

# Count lines, words, chars
wc -l file.txt      # Line count
wc -w file.txt      # Word count
```

### Sytem info & processes

```bash
# Current processes
ps aux | grep python        # All processes with "python"
top                         # Live system monitor
htop                        # (better if installed)

# Kill process
kill PID                   # Graceful
kill -9 PID                # Force kill
pkill -f "name"            # Kill by name

# Disk usage
df -h                      # Disk free space
du -sh *                   # Size of current dir contents
```

### Networking

```bash

# IP info
ip a                       # Show IP addresses
hostname -I                # Quick IP

# Open ports
ss -tuln                   # Show listening ports

# SSH
ssh user@host              # Connect
scp file user@host:/path   # Copy file via SSH
```

### Others

```bash
# Environment variables
export VAR=value           # Set
echo $VAR                  # Get

# History & Re-run
history                    # Show command history
!!                         # Run last command
!123                       # Run specific command by number
```

Notes:

- systemd
- selinux
- bash

### Bash 

```bash
#!/bin/bash

echo "===== 🧠 Variable Basics ====="
name="Alice"
age=30
echo "Name: $name, Age: $age"
echo

echo "===== ✅ If / Else / Elif ====="
if [ "$age" -lt 18 ]; then
  echo "Underage"
elif [ "$age" -ge 18 ] && [ "$age" -lt 65 ]; then
  echo "Adult"
else
  echo "Senior"
fi
echo

echo "===== 📂 File & Directory Checks ====="
file="sample.txt"
dir="testdir"

touch $file
mkdir -p $dir

if [ -f "$file" ]; then
  echo "$file is a file"
fi

if [ -d "$dir" ]; then
  echo "$dir is a directory"
fi

rm -f $file
rm -rf $dir
echo

echo "===== ❓ Variable Check ====="
unset MY_VAR

if [ -z "$MY_VAR" ]; then
  echo "MY_VAR is unset or empty"
fi

MY_VAR="Hello"
if [ -n "$MY_VAR" ]; then
  echo "MY_VAR is set: $MY_VAR"
fi
echo

echo "===== 🔁 Loops ====="

echo "--- For Loop ---"
for i in 1 2 3; do
  echo "Looping: $i"
done

echo "--- While Loop ---"
count=0
while [ $count -lt 3 ]; do
  echo "Count: $count"
  ((count++))
done
echo

echo "===== 🧪 Functions ====="

say_hello() {
  echo "Hello, $1!"
}

say_hello "Bash User"
echo

echo "===== 🔄 Case Statement ====="

fruit="banana"
case "$fruit" in
  apple) echo "It's an apple" ;;
  banana) echo "Bananas are yellow" ;;
  *) echo "Unknown fruit" ;;
esac
echo

echo "===== 💥 Error Handling ====="

command_not_found 2>/dev/null
if [ $? -ne 0 ]; then
  echo "Previous command failed"
fi
echo

echo "===== 💡 Arrays ====="
arr=("apple" "banana" "cherry")
echo "First element: ${arr[0]}"
for item in "${arr[@]}"; do
  echo "Fruit: $item"
done
echo

echo "===== 📝 Read User Input ====="
# Uncomment to test interaction
# read -p "Enter your name: " user_input
# echo "Hello, $user_input"
echo "Skipped for script demo"
echo

echo "===== 📦 Command Line Args ====="
echo "Script name: $0"
echo "First arg: $1"
echo "Total args: $#"
echo "All args: $@"
echo

echo "===== ✅ Done! Quick Ref of Bash Basics. ====="
```

## Python

### Files

```python
with open("example.txt", "w") as f:
    f.write("Hello\nWorld")

with open("example.txt", "r") as f:
    lines = f.readlines()
    print("Read lines:", lines)
```

### Functools

```python
# 🛠️ FUNCTOOLS: Powerful utilities
from functools import lru_cache, partial, reduce

# Cache function calls
@lru_cache(maxsize=128)
def fib(n):
    if n < 2:
        return n
    return fib(n-1) + fib(n-2)

print("Fibonacci(10):", fib(10))

# Partial functions
def power(base, exp):
    return base ** exp

square = partial(power, exp=2)
print("3 squared:", square(3))

# Reduce
nums = [1, 2, 3, 4]
total = reduce(lambda x, y: x + y, nums)
print("Sum using reduce:", total)
```

### Lambdas

```python
nums = [1, 2, 3]
squares = list(map(lambda x: x**2, nums))
print(squares)  # → [1, 4, 9]

nums = [1, 2, 3]
squares = list(map(lambda x: x**2, nums))
print(squares)  # → [1, 4, 9]
```

### Itertools

```python
from itertools import groupby, islice

# Grouping similar elements
data = ['a', 'a', 'b', 'b', 'c']
for key, group in groupby(data):
    print(key, list(group))

# Take first 3 elements of a generator
from itertools import count
for i in islice(count(100), 3):
    print(i)
```

### Logger

```python

import logging

logging.basicConfig(
    level=logging.DEBUG,  # Use INFO or WARNING in prod
    format="%(asctime)s [%(levelname)s] %(message)s"
)

logging.debug("This is a debug message")    # Dev only
logging.info("This is an info message")     # Always useful
logging.warning("This is a warning")        # Show in prod too

```

Use logger = ```python logging.getLogger(__name__)``` in modules and configure logging once in your main app.

### Typing

```python
from typing import List, Dict, Optional

def greet(name: str, age: Optional[int] = None) -> str:
    if age:
        return f"Hello {name}, age {age}"
    return f"Hello {name}"

def sum_list(nums: List[int]) -> int:
    return sum(nums)
```

### Measuring time

```python
import time

start = time.time()
result = sum([x for x in range(1000000)])

end = time.time()
print(f"Execution time: {end - start:.4f} seconds")
```


### Multithreading

```python
from concurrent.futures import as_completed

with ThreadPoolExecutor(max_workers=3) as executor:
    futures = [executor.submit(task, i) for i in range(5)]
    for future in as_completed(futures):
        print(future.result())
```

### Multiprocessing

```python
from multiprocessing import Process, Queue, cpu_count

def worker(q):
    q.put("Done")

if __name__ == "__main__":
    q = Queue()
    p = Process(target=worker, args=(q,))
    p.start()
    p.join()
    print(q.get())
    print("CPUs:", cpu_count())
```

### Unit testing

```python
import unittest
from my_module import add, divide

class TestMyModule(unittest.TestCase):

    def setUp(self):
        print("\n[Setup] Preparing test environment...")
        self.a = 10
        self.b = 5

    def tearDown(self):
        print("[Teardown] Cleaning up after test...")

    def test_add(self):
        result = add(self.a, self.b)
        self.assertEqual(result, 15)

    def test_divide(self):
        result = divide(self.a, self.b)
        self.assertEqual(result, 2)

    def test_divide_by_zero(self):
        with self.assertRaises(ValueError):
            divide(self.a, 0)

if __name__ == "__main__":
    unittest.main()
```

```python
import unittest
from unittest.mock import patch
from my_module import process_data

class TestMyModule(unittest.TestCase):

    @patch('my_module.get_data')
    def test_process_data(self, mock_get):
        # Setup mock return value
        mock_get.return_value = "mocked response"

        result = process_data()
        self.assertEqual(result, "MOCKED RESPONSE")
        mock_get.assert_called_once()

if __name__ == "__main__":
    unittest.main()
```

## TDD rules

** Red: Write a failing test **
Write a test case for a new feature or behavior that does not yet exist.

Run tests and watch it fail. This confirms your test is valid and the feature is missing.

Example: Test a function that doesn’t exist or a new behavior.

** Green: Make the test pass **
Write the simplest possible code to make the failing test pass.

Don’t worry about code quality yet — focus on correctness.

Run tests and confirm the test suite is green (all passing).

** Refactor: Improve the code **
Now, clean up your code:

* Remove duplication
* Improve naming
* Simplify logic

Run tests often to ensure no regressions.

Your codebase gets better without breaking functionality.

## Openshift


