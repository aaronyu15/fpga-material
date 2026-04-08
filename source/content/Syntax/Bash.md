A Guide to Bash Scripting

Variables store data. In Bash, there are no data types in the traditional sense; all variables are treated as strings.

Defining Variables
Always wrap values in quotes.
```bash
# No spaces around the equals sign
variable_name="some value"
```


Accessing Variables
Use the dollar sign `$` to access the value of a variable.
It's recommended to put quotes around accessed variables.
```bash
# Worst way
echo $variable_name
# Preferred
echo ${variable_name}
# Best, safest in terms of word splitting and pathname expansion
echo "${variable_name}"
```

The non quotes method `${variable_name}` can also be used when word splitting is desired, such as in a for loop or as flags to a command.

Special Variables
Bash has several special, pre-defined variables:
```bash
$0
The name of the script.

$1 - $9
The first nine command-line arguments.

$#
The number of command-line arguments.

$@
All command-line arguments as separate strings.

$*
All command-line arguments as a single string.

$?
The exit status of the last command (0 for success, non-zero for failure).

$$
The process ID (PID) of the current shell.
```

The `shift` keyword can be used to parse through the `$@` command-line arguments. It discards the current `$1`, decrements `$#`, and moves the rest of the commands down so that `$1` then points to the next command. For example:
```bash
while [[ "$#" -gt 0 ]]; do
    case $1 in
        --fpga) FPGA=1 ;;
        --cpu) CPU=1 ;;
        --gpu) GPU=1 ;;
        *) echo "Unknown parameter passed: $1"; exit 1 ;;
    esac
    shift
done
```

3. Command Substitution
Command substitution allows you to use the output of a command as part of another command, typically for assigning to a variable.

```bash
# Using backticks (legacy)
current_date=`date`

# Using $(...) (recommended)
current_directory=$(pwd)
```

4. Arithmetic Operations
Use the `$((...))` syntax for arithmetic operations.

```bash
x=10
y=5
sum=$(($x + $y))
echo "Sum: $sum" # Output: Sum: 15

# Other operations
product=$(($x * $y))
difference=$(($x - $y))
quotient=$(($x / $y))
```

5. Conditional Statements
Conditional statements allow your script to make decisions.

```bash
count=10
if [ $count -eq 10 ]; then
  echo "Count is 10."
elif [ $count -gt 10 ]; then
  echo "Count is greater than 10."
else
  echo "Count is less than 10."
fi
```
The `[[ ... ]]` Construct
While the single-bracket `[ ... ]` (which is an alias for the test command) is POSIX-compliant and works in many shells, Bash offers a more powerful and intuitive alternative: the double-bracket `[[ ... ]]`.
- No Word Splitting or Globbing: You do not need to quote variables to prevent word splitting or filename expansion. This is a major source of bugs when using single brackets.
- Pattern Matching: You can use standard globbing patterns (like `*` and `?`) directly for string comparisons.
- Regular Expression Matching: It supports regular expression matching with the `=~` operator.
- Logical Operators: You can use `&&` and `||` for logical AND and OR directly inside the brackets, instead of `-a` and `-o`.

It's still recommended to quote variables.

Examples:
```bash
# No need to quote variables
filename="My File.txt"
if [[ -f $filename ]]; then # Or [[ -f "$filename" ]]
  echo "File exists (no quotes needed)."
fi

# Pattern Matching
if [[ $filename == M* ]]; then # Or [[ "$filename" == M* ]]
  echo "Filename starts with 'M'."
fi

# Regular Expression Matching
email="user@example.com"
if [[ $email =~ ^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,4}$ ]]; then
  echo "Valid email address."
fi

# Logical AND
number=15
if [[ $number -gt 10 && $number -lt 20 ]]; then # or [[  "$number" -gt 10 && "$number" -lt 20  ]]
    echo "The number is between 10 and 20."
fi
```

Test Operators
File Tests:
```bash
-f
True if the file is a regular file.

-d
True if the file is a directory.

-e
True if the file exists.

-r
True if the file is readable.

-w
True if the file is writable.

-x
True if the file is executable.

-s
True if the file has a size greater than zero.

-n
True if the given string is not empty.

-a
Logical AND. Using && is preferred.
```

String Tests:
```bash
=
True if the strings are equal.

!=
True if the strings are not equal.

-z
True if the string is empty.

-n
True if the string is not empty.
```

Integer Tests:
```bash
-eq
Equal to

-ne
Not equal to

-gt
Greater than

-ge
Greater than or equal to

-lt
Less than

-le
Less than or equal to
```

case Statements
A case statement is a simpler way to write multiple if conditions.
```bash
read -p "Enter a fruit: " fruit

case $fruit in
  "apple")
    echo "This is a red fruit."
    ;;
  "banana")
    echo "This is a yellow fruit."
    ;;
  *)
    echo "Unknown fruit."
    ;;
esac
```


6. Loops
Loops allow you to execute a block of code multiple times.

```bash
for Loop
# Looping through a list of items
for i in 1 2 3 4 5; do
  echo "Number: $i"
done

# Looping through a list of items
for i in {1..10}; do
  echo "Number: $i"
done

# C-style for loop
for (( i=0; i<5; i++ )); do
  echo "Iteration: $i"
done
```

while Loop
Executes as long as a condition is true.

```bash
counter=0
while [ $counter -lt 5 ]; do
  echo "Counter is $counter"
  let counter=counter+1
done
```

until Loop
Executes until a condition becomes true.

```bash
counter=0
until [ $counter -ge 5 ]; do
  echo "Counter is $counter"
  let counter++
done
```

7. Functions
Functions help organize your code into reusable blocks.

```bash
Defining Functions
# Method 1
function my_function() {
  echo "Hello from my_function!"
}

# Method 2 (more common)
my_other_function() {
  echo "Positional argument 1: $1"
  echo "Positional argument 2: $2"
  return 0 # Optional return status
}

Calling Functions
my_function
my_other_function "hello" "world"
```


8. File I/O and Redirection
You can redirect the input and output of commands.
```bash
>
Redirect standard output (stdout), overwriting the file.

>>
Redirect stdout, appending to the file.

<
Redirect standard input (stdin).

2>
Redirect standard error (stderr).

&>
Redirect both stdout and stderr.

Examples:
# Write "Hello" to a file
echo "Hello" > output.txt

# Append "World" to the same file
echo "World" >> output.txt

# Read from a file line by line
while IFS= read -r line; do
  echo "Read line: $line"
done < input.txt

# Redirect errors to a log file
ls /nonexistent_directory 2> error.log
```


9. Arrays
Bash supports one-dimensional indexed arrays.
```bash
Defining Arrays
# Indexed array
my_array=("apple" "banana" "cherry")

Accessing Array Elements
# Access a single element
echo ${my_array[0]} # Output: apple

# Access all elements
echo ${my_array[@]} # Output: apple banana cherry

Array Operations
# Get the number of elements
echo ${#my_array[@]} # Output: 3

# Add an element
my_array+=("orange")

# Loop through an array
for fruit in "${my_array[@]}"; do
  echo "Fruit: $fruit"
done
```

10. Regular Expressions
```bash
string="My phone number is 123-456-7890"
if [[ $string =~ ([0-9]{3})-([0-9]{3})-([0-9]{4}) ]]; then
  echo "Phone number found: ${BASH_REMATCH[0]}"
  echo "Area code: ${BASH_REMATCH[1]}"
fi
```

11. Useful Commands for Scripting
```bash
grep: Search for patterns in text.

sed: Stream editor for filtering and transforming text.

awk: A powerful pattern-scanning and processing language.

find: Search for files in a directory hierarchy.

cut: Remove sections from each line of files.

sort: Sort lines of text files.

uniq: Report or omit repeated lines.

tr: Translate or delete characters.
```

# Common Patterns

1. The Standard Script Header
Every script should start with a shebang to specify the interpreter and comments to describe its purpose.
```bash
#!/bin/bash
#
# Script Name: your_script_name.sh
#
# Author: Your Name
# Date: YYYY-MM-DD
#
# Description: A brief summary of what the script does.
#
# Usage: ./your_script_name.sh [arguments]
#
```

2. Best Practices for Variables
Always quote your variables and use curly braces. This prevents issues with spaces and special characters.
```bash
# --- Define a variable (no spaces around '=')
config_file="/etc/my_app/config.conf"

# --- Access a variable (best practice: quoted and braced)
echo "Reading configuration from: \"${config_file}\""

# --- Define a constant (convention: uppercase and readonly)
readonly LOG_DIR="/var/log/my_app"
```

3. Handling Command-Line Arguments
This template shows how to check for the correct number of arguments and assign them to clearly named variables.
```bash
# --- Check for the correct number of arguments
if [ "$#" -ne 2 ]; then
    echo "Usage: $0 <source_directory> <destination_file>"
    exit 1
fi

# --- Assign arguments to descriptive variables
readonly source_dir="$1"
readonly destination_file="$2"

echo "Source directory is: \"${source_dir}\""
echo "Destination file is: \"${destination_file}\""

# --- Check if source directory exists
if [ ! -d "${source_dir}" ]; then
    echo "Error: Source directory '${source_dir}' not found."
    exit 1
fi
```

4. Looping Through a List (for loop)
This is the standard way to iterate over a known set of items.
```bash
servers=("server1.example.com" "server2.example.com" "server3.example.com")

echo "Pinging servers..."
for server in "${servers[@]}"; do
    echo "--- Pinging ${server} ---"
    ping -c 1 "${server}"
    echo ""
done

echo "Finished."
```

5. Reading a File Line by Line (while loop)
This is the most robust and memory-efficient way to process a file line by line. It avoids common pitfalls with spaces or special characters in the lines.
```bash
input_file="domains.txt"

if [ ! -f "${input_file}" ]; then
    echo "Error: Input file '${input_file}' not found."
    exit 1
fi

echo "Reading domains from ${input_file}:"
while IFS= read -r line; do
    # Skip empty lines or lines that start with # (comments)
    if [ -z "${line}" ] || [[ "${line}" == \#* ]]; then
        continue
    fi
    
    echo "Processing domain: ${line}"
    # Do something with the line, e.g., host "${line}"
done < "${input_file}"
```

6. Searching for Content in Files
This recipe combines find and grep to locate files containing specific text.
```bash
search_term="ERROR"
search_dir="/var/log"

echo "Searching for term \"${search_term}\" in directory \"${search_dir}\"..."

# -type f: only search in files
# -name "*.log": only search in files ending with .log
# -print0 and -0: Safely handle filenames with spaces or special characters
find "${search_dir}" -type f -name "*.log" -print0 | while IFS= read -r -d '' file; do
    if grep -q "${search_term}" "${file}"; then
        echo "Found in: ${file}"
    fi
done

echo "Search complete."
```

7. Defining and Using Functions
Functions help organize your code into reusable and readable blocks.
```bash
# --- Function to print a log message with a timestamp
log() {
    local message="$1"
    echo "$(date '+%Y-%m-%d %H:%M:%S') - ${message}"
}

# --- Function to check if a command exists
command_exists() {
    command -v "$1" >/dev/null 2>&1
}

# --- Main script logic ---
log "Script started."

if command_exists "git"; then
    log "Git is installed."
else
    log "Error: Git is not installed. Please install it."
    exit 1
fi

log "Script finished."
```

8. Handling Script Exit and Cleanup (trap)
The trap command allows you to execute a command when the script receives a signal, such as EXIT (any exit), INT (Ctrl+C), or TERM. This is crucial for cleaning up temporary files.
```bash
# --- Create a temporary file
temp_file=$(mktemp)

# --- Define a cleanup function
cleanup() {
    echo "Cleaning up temporary file: ${temp_file}"
    rm -f "${temp_file}"
}

# --- Set the trap to call the cleanup function on script exit
trap cleanup EXIT

# --- Main script logic ---
echo "Hello, World!" > "${temp_file}"
echo "Temporary data written to ${temp_file}"
sleep 5
echo "Script is finishing."
```