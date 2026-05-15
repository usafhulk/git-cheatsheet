# 🔧 Comprehensive Bash Commands & Scripting Reference

> Every major Bash feature, built-in, and scripting construct with real examples.
> Bash version: 5.x | Tested on Linux and macOS.

---

## Table of Contents

1. [Running Bash Scripts](#1-running-bash-scripts)
2. [Variables](#2-variables)
3. [String Operations](#3-string-operations)
4. [Arrays](#4-arrays)
5. [Arithmetic](#5-arithmetic)
6. [Conditionals (if/elif/else)](#6-conditionals-ifelifelse)
7. [Test Operators](#7-test-operators)
8. [Loops](#8-loops)
9. [Functions](#9-functions)
10. [Input & Output](#10-input--output)
11. [Redirection & Pipes](#11-redirection--pipes)
12. [Process Substitution](#12-process-substitution)
13. [Here Documents & Here Strings](#13-here-documents--here-strings)
14. [Command Substitution](#14-command-substitution)
15. [Subshells & Command Grouping](#15-subshells--command-grouping)
16. [Pattern Matching & Globbing](#16-pattern-matching--globbing)
17. [Regular Expressions](#17-regular-expressions)
18. [Error Handling](#18-error-handling)
19. [Debugging](#19-debugging)
20. [Parameters & Arguments](#20-parameters--arguments)
21. [Built-in Commands](#21-built-in-commands)
22. [Shell Options](#22-shell-options)
23. [Job Control](#23-job-control)
24. [Signal Handling (trap)](#24-signal-handling-trap)
25. [Coprocesses](#25-coprocesses)
26. [Associative Arrays (Dictionaries)](#26-associative-arrays-dictionaries)
27. [File Operations in Scripts](#27-file-operations-in-scripts)
28. [Networking in Bash](#28-networking-in-bash)
29. [Date & Time in Scripts](#29-date--time-in-scripts)
30. [Common Script Patterns & Recipes](#30-common-script-patterns--recipes)

---

## 1. Running Bash Scripts

### Shebang Line
```bash
#!/usr/bin/env bash         # portable — uses PATH to find bash
#!/bin/bash                  # direct path (most common)
#!/bin/sh                    # POSIX sh (limited features)
```

### Making a Script Executable
```bash
chmod +x script.sh
./script.sh                  # run from current directory
bash script.sh               # explicit interpreter (no need for shebang or +x)
sh script.sh                 # run as POSIX sh
source script.sh             # run in current shell (shares environment)
. script.sh                  # same as source
```

### Script Header Best Practice
```bash
#!/usr/bin/env bash
set -euo pipefail            # strict mode
IFS=$'\n\t'                  # safe word splitting
```

- `set -e` — exit on any error
- `set -u` — treat unset variables as errors
- `set -o pipefail` — exit on pipe failure
- `IFS=$'\n\t'` — prevent word-splitting on spaces

---

## 2. Variables

### Assignment and Use
```bash
name="Alice"
age=30
greeting="Hello, $name!"

echo $name                   # Alice
echo ${name}                 # safer (explicit braces)
echo "${name} is ${age}"     # always quote variables
```

### Variable Types
```bash
readonly PI=3.14159          # constant (cannot be reassigned)
declare -i count=0           # integer variable
declare -r MAX=100           # readonly
declare -l lower="HELLO"     # auto-lowercase
declare -u upper="hello"     # auto-uppercase
declare -x EXPORTED="val"    # exported to subprocesses
```

### Default and Fallback Values
```bash
echo ${VAR:-default}         # use default if VAR is unset or empty
echo ${VAR:=default}         # assign default if unset or empty
echo ${VAR:+replacement}     # use replacement if VAR is set
echo ${VAR:?error message}   # print error and exit if unset or empty
```

### Variable Scope
```bash
global_var="I am global"

function demo() {
    local local_var="I am local"
    echo "$global_var"       # accessible
    echo "$local_var"
}

demo
# echo "$local_var"          # empty or error — out of scope
```

### Indirect Reference
```bash
varname="greeting"
greeting="Hello!"
echo "${!varname}"           # Hello! (indirect expansion)
```

### Special Variables
```bash
$0           # script name
$1 $2 $3     # positional parameters
$@           # all positional params (as separate words)
$*           # all positional params (as one word)
$#           # number of positional params
$?           # exit status of last command
$$           # PID of current shell
$!           # PID of last background command
$_           # last argument of previous command
$-           # current shell options
RANDOM       # random integer 0-32767
LINENO       # current line number
FUNCNAME     # current function name (array)
BASH_VERSION # Bash version string
BASH_SOURCE  # array of source file names
PIPESTATUS   # array of exit codes from a pipe
```

---

## 3. String Operations

### Length
```bash
str="Hello World"
echo ${#str}                 # 11
```

### Substring Extraction
```bash
str="Hello World"
echo ${str:6}                # World (from index 6)
echo ${str:0:5}              # Hello (5 chars from index 0)
echo ${str: -5}              # World (last 5 chars, note the space)
echo ${str:6:3}              # Wor (3 chars from index 6)
```

### Substring Removal
```bash
path="/home/alice/docs/file.txt"

echo ${path#*/}              # home/alice/docs/file.txt  (remove shortest prefix match)
echo ${path##*/}             # file.txt                  (remove longest prefix match)
echo ${path%/*}              # /home/alice/docs           (remove shortest suffix match)
echo ${path%%/*}             # (empty — removes longest suffix)

filename="archive.tar.gz"
echo ${filename%.gz}         # archive.tar  (strip extension)
echo ${filename%%.*}         # archive (strip all extensions)
```

### Substitution
```bash
str="Hello World World"
echo ${str/World/Bash}       # Hello Bash World     (first match)
echo ${str//World/Bash}      # Hello Bash Bash      (all matches)
echo ${str/#Hello/Hi}        # Hi World World       (prefix match)
echo ${str/%World/Earth}     # Hello World Earth    (suffix match)
```

### Case Conversion (Bash 4+)
```bash
str="Hello World"
echo ${str,,}                # hello world          (all lowercase)
echo ${str^^}                # HELLO WORLD          (all uppercase)
echo ${str,}                 # hELLO WORLD          (first char lowercase)
echo ${str^}                 # Hello World          (first char uppercase)
```

### String Testing
```bash
str="Hello"

[[ -z "$str" ]] && echo "empty" || echo "not empty"
[[ -n "$str" ]] && echo "has content"
[[ "$str" == "Hello" ]] && echo "exact match"
[[ "$str" == H* ]] && echo "starts with H"
[[ "$str" =~ ^[A-Z] ]] && echo "starts with uppercase"
```

### printf for Formatting
```bash
printf "%-15s %5d %8.2f\n" "Alice" 30 9999.99
# Alice            30  9999.99

printf "%05d\n" 42            # 00042 (zero-padded)
printf "%x\n" 255             # ff (hex)
printf "%o\n" 8               # 10 (octal)
printf "%b\n" "line1\nline2"  # interpret \n
```

---

## 4. Arrays

### Indexed Arrays
```bash
# Declaration
fruits=("apple" "banana" "cherry")
declare -a colors

# Assignment
colors[0]="red"
colors[1]="green"
colors[2]="blue"

# Access
echo ${fruits[0]}            # apple
echo ${fruits[-1]}           # cherry (last element, Bash 4.3+)
echo ${fruits[@]}            # all elements (as words)
echo ${fruits[*]}            # all elements (as one string)
echo ${#fruits[@]}           # 3 (number of elements)
echo ${!fruits[@]}           # 0 1 2 (indices)

# Modify
fruits+=(  "date" "elderberry")  # append
fruits[1]="blueberry"        # replace
unset fruits[2]              # remove element

# Slice
echo ${fruits[@]:1:2}        # elements at index 1 and 2
```

### Iterating Arrays
```bash
fruits=("apple" "banana" "cherry")

# By value
for fruit in "${fruits[@]}"; do
    echo "$fruit"
done

# By index
for i in "${!fruits[@]}"; do
    echo "$i: ${fruits[$i]}"
done

# C-style
for (( i=0; i<${#fruits[@]}; i++ )); do
    echo "${fruits[$i]}"
done
```

### Sorting
```bash
fruits=("banana" "apple" "cherry")

# Sort into new array
IFS=$'\n' sorted=($(sort <<<"${fruits[*]}")); unset IFS
echo "${sorted[@]}"          # apple banana cherry

# Sort in place (Bash 4.4+)
readarray -t sorted < <(printf '%s\n' "${fruits[@]}" | sort)
```

### Reading File Into Array
```bash
mapfile -t lines < file.txt          # read lines into array
readarray -t lines < file.txt        # same as mapfile
echo "${lines[0]}"                    # first line
echo "${#lines[@]}"                   # number of lines
```

---

## 5. Arithmetic

### Arithmetic Expansion
```bash
echo $((2 + 3))              # 5
echo $((10 / 3))             # 3 (integer division)
echo $((10 % 3))             # 1 (modulo)
echo $((2 ** 8))             # 256 (exponentiation)

x=5
(( x++ ))                    # increment
(( x-- ))                    # decrement
(( x += 10 ))                # add 10
(( x *= 2 ))                 # multiply

result=$(( (x + 5) * 2 ))    # complex expression
```

### `let` Built-in
```bash
let "a = 5 + 3"
let "b = a * 2"
echo $a $b                   # 8 16
let a++
```

### `declare -i` Integer
```bash
declare -i num=42
num+=8                       # 50 (integer arithmetic)
```

### Floating-Point with `bc`
```bash
echo "scale=2; 22/7" | bc          # 3.14
echo "scale=4; sqrt(2)" | bc -l    # 1.4142
result=$(echo "scale=2; $a / $b" | bc)
```

### `awk` for Floating-Point
```bash
awk "BEGIN {printf \"%.4f\n\", 22/7}"  # 3.1429
awk "BEGIN {print sqrt(2)}"
awk "BEGIN {print int(3.9)}"           # 3 (truncate)
```

---

## 6. Conditionals (if/elif/else)

### Basic `if`
```bash
if [[ $age -ge 18 ]]; then
    echo "Adult"
fi
```

### `if/elif/else`
```bash
score=75

if [[ $score -ge 90 ]]; then
    grade="A"
elif [[ $score -ge 80 ]]; then
    grade="B"
elif [[ $score -ge 70 ]]; then
    grade="C"
else
    grade="F"
fi

echo "Grade: $grade"
```

### One-Liner Conditionals
```bash
[[ -f file.txt ]] && echo "exists"          # AND shorthand
[[ -f file.txt ]] || echo "not found"       # OR shorthand
command && echo "success" || echo "failed"
```

### `case` Statement
```bash
fruit="apple"

case "$fruit" in
    apple)
        echo "It's an apple"
        ;;
    banana|plantain)
        echo "It's a banana or plantain"
        ;;
    [Cc]herry)
        echo "Cherry (any case)"
        ;;
    *)
        echo "Unknown fruit"
        ;;
esac
```

### Case with Regex-Like Patterns (Bash 4.0+)
```bash
case "$input" in
    [0-9]*)     echo "starts with digit" ;;
    [A-Za-z]*)  echo "starts with letter" ;;
    *)          echo "other" ;;
esac
```

### Ternary Operator
```bash
result=$([ $x -gt 10 ] && echo "big" || echo "small")
```

---

## 7. Test Operators

### File Tests
```bash
[[ -e file ]]    # exists (any type)
[[ -f file ]]    # regular file
[[ -d dir ]]     # directory
[[ -L link ]]    # symbolic link
[[ -r file ]]    # readable
[[ -w file ]]    # writable
[[ -x file ]]    # executable
[[ -s file ]]    # not empty (size > 0)
[[ -p pipe ]]    # named pipe (FIFO)
[[ -b file ]]    # block special file
[[ -c file ]]    # character special file
[[ file1 -nt file2 ]] # file1 newer than file2
[[ file1 -ot file2 ]] # file1 older than file2
[[ file1 -ef file2 ]] # same file (same inode)
```

### String Tests
```bash
[[ -z "$str" ]]              # empty string
[[ -n "$str" ]]              # non-empty string
[[ "$a" == "$b" ]]           # equal
[[ "$a" != "$b" ]]           # not equal
[[ "$a" < "$b" ]]            # lexicographically less
[[ "$a" > "$b" ]]            # lexicographically greater
[[ "$str" =~ regex ]]        # regex match (Bash [[]])
[[ "$str" == pattern* ]]     # glob match
```

### Integer Tests
```bash
[[ $a -eq $b ]]              # equal
[[ $a -ne $b ]]              # not equal
[[ $a -lt $b ]]              # less than
[[ $a -le $b ]]              # less than or equal
[[ $a -gt $b ]]              # greater than
[[ $a -ge $b ]]              # greater than or equal
```

### Compound Tests
```bash
[[ condition1 && condition2 ]]     # logical AND
[[ condition1 || condition2 ]]     # logical OR
[[ ! condition ]]                  # logical NOT

# Example:
[[ -f file.txt && -r file.txt ]] && echo "readable file"
[[ $x -gt 0 && $x -lt 100 ]]     # range check
```

---

## 8. Loops

### `for` Loop
```bash
# Range
for i in {1..5}; do
    echo "$i"
done

# Range with step
for i in {0..20..5}; do
    echo "$i"
done

# C-style
for (( i=0; i<10; i++ )); do
    echo "$i"
done

# Iterate list
for color in red green blue; do
    echo "$color"
done

# Iterate files
for f in *.txt; do
    echo "Processing $f"
done

# Iterate command output
for user in $(awk -F: '$3>=1000{print $1}' /etc/passwd); do
    echo "$user"
done
```

### `while` Loop
```bash
count=1
while [[ $count -le 5 ]]; do
    echo "Count: $count"
    (( count++ ))
done

# Read lines from file
while IFS= read -r line; do
    echo "$line"
done < file.txt

# Read from command output
while IFS= read -r line; do
    echo "Log: $line"
done < <(journalctl -n 100)

# Infinite loop
while true; do
    echo "Running..."
    sleep 5
done
```

### `until` Loop
```bash
count=1
until [[ $count -gt 5 ]]; do
    echo "$count"
    (( count++ ))
done
```

### Loop Control
```bash
for i in {1..10}; do
    [[ $i -eq 5 ]] && break          # exit loop
    [[ $i -eq 3 ]] && continue       # skip this iteration
    echo "$i"
done

# Break from nested loop
for i in {1..3}; do
    for j in {1..3}; do
        [[ $j -eq 2 ]] && break 2    # break outer loop
        echo "$i $j"
    done
done
```

### `select` — Menu Loop
```bash
PS3="Choose a fruit (enter number): "
select fruit in apple banana cherry quit; do
    case $fruit in
        quit) break ;;
        *)    echo "You chose: $fruit" ;;
    esac
done
```

---

## 9. Functions

### Definition and Call
```bash
function greet() {
    echo "Hello, $1!"
}

say_goodbye() {                 # alternate syntax (no 'function' keyword)
    echo "Goodbye, $1!"
}

greet "Alice"
say_goodbye "Bob"
```

### Return Values
```bash
# Exit status
function is_even() {
    (( $1 % 2 == 0 ))          # returns 0 (true) or 1 (false)
}

if is_even 4; then
    echo "4 is even"
fi

# Return value via echo (command substitution)
function add() {
    echo $(( $1 + $2 ))
}

result=$(add 3 5)
echo "Result: $result"          # Result: 8

# Return value via global variable
function divide() {
    RESULT=$(echo "scale=2; $1 / $2" | bc)
}

divide 22 7
echo "$RESULT"                  # 3.14
```

### Local Variables
```bash
function calculate() {
    local a=$1
    local b=$2
    local sum=$(( a + b ))
    echo "$sum"
}
```

### Function with Default Arguments
```bash
function connect() {
    local host=${1:-localhost}
    local port=${2:-8080}
    echo "Connecting to $host:$port"
}

connect                         # Connecting to localhost:8080
connect myserver 443            # Connecting to myserver:443
```

### Recursive Functions
```bash
function factorial() {
    local n=$1
    if [[ $n -le 1 ]]; then
        echo 1
    else
        local prev=$(factorial $(( n - 1 )))
        echo $(( n * prev ))
    fi
}

echo $(factorial 5)             # 120
```

### Passing Arrays to Functions
```bash
function print_array() {
    local -n arr=$1             # nameref (Bash 4.3+)
    for item in "${arr[@]}"; do
        echo "  - $item"
    done
}

fruits=("apple" "banana" "cherry")
print_array fruits
```

---

## 10. Input & Output

### `read` — Read User Input
```bash
read -p "Enter name: " name
echo "Hello, $name!"

read -sp "Enter password: " password   # silent (no echo)
echo ""                                # newline after password

read -t 10 -p "Answer in 10s: " answer  # timeout
[[ $? -eq 142 ]] && echo "Timed out"

read -n 1 -p "Press any key..."        # read single character

read -a words                          # read into array
echo "${words[0]}"                     # first word

read -d ':' value                      # read until delimiter
```

### `echo` vs `printf`
```bash
echo "Hello"                    # simple
echo -n "No newline"            # no trailing newline
echo -e "Tab:\there\nNewline"  # escape sequences

printf "Name: %-10s Age: %d\n" "Alice" 30
printf "%04d\n" 7               # 0007
printf "%.2f\n" 3.14159         # 3.14
printf "%s\n" "${array[@]}"     # print each array element
```

---

## 11. Redirection & Pipes

### Standard Streams
```bash
# stdin  = 0
# stdout = 1
# stderr = 2
```

### Output Redirection
```bash
ls > output.txt                 # stdout to file (overwrite)
ls >> output.txt                # stdout to file (append)
ls 2> errors.txt                # stderr to file
ls > all.txt 2>&1              # stdout and stderr to file
ls &> all.txt                   # shorthand for above (Bash)
ls 2>/dev/null                  # discard stderr
ls >/dev/null 2>&1             # discard all output
```

### Input Redirection
```bash
wc -l < file.txt                # stdin from file
```

### Named Pipes (FIFOs)
```bash
mkfifo mypipe
cat mypipe &                    # reader in background
echo "data" > mypipe            # writer
```

### Pipes
```bash
ls -la | grep ".txt"            # pipe stdout to stdin
ls | sort | uniq | head -10     # multi-stage pipeline
cat /etc/passwd | awk -F: '{print $1}' | sort
command1 | tee output.txt | command2   # tee: split pipe to file and stdout
```

### Pipeline Exit Code
```bash
ls nonexistent | sort
echo "Pipe exit: ${PIPESTATUS[@]}"   # [2, 0] (ls failed, sort succeeded)

set -o pipefail                      # entire pipe fails if any command fails
```

---

## 12. Process Substitution

```bash
# <(command) — treat command output as a file
diff <(sort file1.txt) <(sort file2.txt)
while IFS= read -r line; do echo "$line"; done < <(ls -la)

# >(command) — treat file as command stdin
tee >(gzip > output.gz) < input.txt

# Compare two commands output without temp files
comm <(sort list1.txt) <(sort list2.txt)

# Use command output as argument
vim <(curl -s https://example.com/config.json)
```

---

## 13. Here Documents & Here Strings

### Here Document
```bash
cat <<EOF
Line 1
Line 2
Variable: $HOME
EOF

# No expansion (single-quoted delimiter)
cat <<'EOF'
No $variable expansion here
EOF

# Indented (strip leading tabs with <<-)
cat <<-EOF
	indented content
	more content
	EOF

# Write to file
cat > config.txt <<EOF
server=localhost
port=8080
debug=true
EOF

# Feed to command
mysql -u root -p <<EOF
CREATE DATABASE mydb;
USE mydb;
CREATE TABLE users (id INT, name VARCHAR(100));
EOF
```

### Here String
```bash
grep "pattern" <<< "input string"
bc <<< "scale=2; 22/7"
read var <<< "single value"
```

---

## 14. Command Substitution

```bash
now=$(date)                             # capture command output
files=$(ls -la | wc -l)
current_dir=$(pwd)
user=$(whoami)

# Nested substitution
echo "Files modified today: $(find . -mtime -1 | wc -l)"

# Old-style backtick (avoid — not nestable)
old_style=`date`

# Multi-line output
lines=$(cat file.txt)
echo "$lines"                           # quotes preserve newlines

# Exit code
output=$(command 2>&1); status=$?
```

---

## 15. Subshells & Command Grouping

### Subshell `()`
```bash
(cd /tmp && ls)                 # change dir only in subshell
echo "Still in: $PWD"          # original dir unchanged

# Capture subshell output
result=$(
    cd /tmp
    ls *.log | wc -l
)

# Parallel subshells
(sleep 1 && echo "job 1 done") &
(sleep 2 && echo "job 2 done") &
wait                            # wait for both to finish
```

### Command Group `{}`
```bash
{ echo "start"; date; echo "end"; }    # must have trailing semicolon or newline
{ echo "line1"; echo "line2"; } > output.txt   # redirect group output

# Conditional group
[[ -f file.txt ]] || { echo "File missing!"; exit 1; }
```

---

## 16. Pattern Matching & Globbing

### Basic Globs
```bash
ls *.txt                    # all .txt files
ls file?.txt                # file + one char + .txt
ls [abc].txt                # a.txt or b.txt or c.txt
ls [a-z].txt                # any lowercase letter
ls [!a-z].txt               # not lowercase letter
ls {*.txt,*.log}            # multiple patterns
```

### Extended Globbing (requires `shopt -s extglob`)
```bash
shopt -s extglob

?(pattern)     # zero or one match
*(pattern)     # zero or more matches
+(pattern)     # one or more matches
@(pattern)     # exactly one match
!(pattern)     # anything except pattern

ls !(*.txt)            # all files except .txt
ls +(img*)             # files starting with img
rm *.{bak,tmp}         # remove multiple extensions
```

### Glob Options
```bash
shopt -s nullglob        # no-match glob expands to nothing (not literal pattern)
shopt -s globstar        # ** matches across directories
shopt -s dotglob         # globs match hidden files
shopt -s nocaseglob      # case-insensitive globbing
shopt -s failglob        # error if glob matches nothing

# Example: recursive glob
shopt -s globstar
ls **/*.txt              # all .txt files in any subdirectory
```

### `[[ =~ ]]` Regex Matching
```bash
email="user@example.com"
if [[ "$email" =~ ^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$ ]]; then
    echo "Valid email"
fi

# Capture groups via BASH_REMATCH
str="Date: 2024-01-15"
if [[ "$str" =~ ([0-9]{4})-([0-9]{2})-([0-9]{2}) ]]; then
    echo "Year: ${BASH_REMATCH[1]}"
    echo "Month: ${BASH_REMATCH[2]}"
    echo "Day: ${BASH_REMATCH[3]}"
fi
```

---

## 17. Regular Expressions

### `grep` Regex
```bash
grep "^Error" app.log              # lines starting with Error
grep "error$" app.log              # lines ending with error
grep "err.r" app.log               # . matches any char
grep "colou\?r" file.txt           # u is optional (BRE)
grep -E "colou?r" file.txt         # ERE (no backslash)
grep -E "cat|dog" file.txt         # alternation
grep -E "[0-9]{3}-[0-9]{4}" file  # phone pattern
grep -P "\d{4}-\d{2}-\d{2}" file  # Perl regex
```

### `sed` Regex
```bash
sed 's/[[:space:]]\+/ /g' file.txt          # normalize spaces
sed 's/[[:upper:]]/\l&/g' file.txt          # lowercase first of uppercase
sed -E 's/(error|warn)/[&]/g' file.txt      # bracket matches
sed -n '/^START/,/^END/p' file.txt          # range pattern
sed 's/\(.\{80\}\).*/\1/' file.txt          # truncate to 80 chars (BRE)
sed -E 's/(.{80}).*/\1/' file.txt           # same in ERE
```

### `awk` Regex
```bash
awk '/^ERROR/' log.txt                       # lines matching
awk '!/^#/' config.txt                       # lines not matching
awk '$3 ~ /^[0-9]+$/' data.txt              # field matches regex
awk 'BEGIN{IGNORECASE=1} /error/' log.txt   # case-insensitive
```

---

## 18. Error Handling

### Exit Codes
```bash
command
echo "Exit code: $?"        # 0 = success, non-zero = failure

# Check explicitly
if ! mkdir /tmp/test; then
    echo "Failed to create directory"
    exit 1
fi

# Short-circuit
command || { echo "Command failed"; exit 1; }
```

### `set -e` and Overrides
```bash
set -e                       # exit on any error

# Temporarily ignore errors
set +e
command_that_might_fail
set -e

# Or use || true
command_that_might_fail || true

# Function that can fail
if ! result=$(command 2>&1); then
    echo "Failed: $result"
fi
```

### `trap` for Cleanup
```bash
function cleanup() {
    echo "Cleaning up..."
    rm -f /tmp/tmpfile.$$
}

trap cleanup EXIT            # run on exit (any cause)
trap cleanup INT TERM        # run on Ctrl+C or kill

# Trap with inline function
trap 'echo "Error on line $LINENO"; exit 1' ERR

# Reset a trap
trap - EXIT
```

### Error Handling Template
```bash
#!/usr/bin/env bash
set -euo pipefail

readonly SCRIPT_DIR="$(cd "$(dirname "${BASH_SOURCE[0]}")" && pwd)"
readonly TMPFILE=$(mktemp)

function cleanup() {
    rm -f "$TMPFILE"
}

function error() {
    echo "ERROR: $*" >&2
    exit 1
}

trap cleanup EXIT
trap 'error "Failed on line $LINENO"' ERR

# Your script logic here
```

---

## 19. Debugging

### `set -x` — Trace Execution
```bash
set -x                          # enable
command1
command2
set +x                          # disable

# Run script in debug mode
bash -x script.sh

# Debug a section only
set -x
critical_command
set +x
```

### `set -v` — Verbose Mode
```bash
set -v                          # print each line before executing
bash -v script.sh
```

### `set -n` — Dry Run (Syntax Check)
```bash
bash -n script.sh               # check syntax without running
```

### Debugging Output
```bash
echo "DEBUG: var=$var" >&2      # write debug to stderr
printf "DEBUG: %s=%s\n" "var" "$var" >&2

# Conditional debug
DEBUG=${DEBUG:-0}
function debug() {
    [[ $DEBUG -eq 1 ]] && echo "DEBUG: $*" >&2
}

debug "Processing file: $file"
```

### `declare -p` — Inspect Variables
```bash
name="Alice"
declare -p name                 # declare -- name="Alice"

arr=(a b c)
declare -p arr                  # declare -a arr=([0]="a" [1]="b" [2]="c")
```

---

## 20. Parameters & Arguments

### Positional Parameters
```bash
echo "Script: $0"
echo "First arg: $1"
echo "All args: $@"
echo "All args: $*"
echo "Count: $#"
echo "Fifth: ${5:-default}"

# Shift
while [[ $# -gt 0 ]]; do
    echo "Arg: $1"
    shift
done
```

### `getopts` — Parse Options
```bash
while getopts "hvf:o:" opt; do
    case $opt in
        h) echo "Usage: script [-h] [-v] [-f file] [-o output]"; exit 0 ;;
        v) VERBOSE=1 ;;
        f) INPUT_FILE="$OPTARG" ;;
        o) OUTPUT_FILE="$OPTARG" ;;
        ?) echo "Unknown option: $opt"; exit 1 ;;
    esac
done

shift $(( OPTIND - 1 ))         # remove processed options
echo "Remaining args: $@"
```

### Long Options with `getopt`
```bash
TEMP=$(getopt -o hvf:o: --long help,verbose,file:,output: -- "$@")
eval set -- "$TEMP"

while true; do
    case "$1" in
        -h|--help)    usage; exit 0 ;;
        -v|--verbose) VERBOSE=1; shift ;;
        -f|--file)    FILE="$2"; shift 2 ;;
        -o|--output)  OUTPUT="$2"; shift 2 ;;
        --)           shift; break ;;
        *)            break ;;
    esac
done
```

### `set --` to Reset Positional Params
```bash
set -- "a" "b" "c"
echo "$1 $2 $3"                 # a b c
```

---

## 21. Built-in Commands

### `declare` / `typeset`
```bash
declare -i num=42               # integer
declare -r CONST="fixed"        # readonly
declare -a arr=()               # indexed array
declare -A dict=()              # associative array
declare -l lower                # auto-lowercase
declare -u upper                # auto-uppercase
declare -x EXPORTED="val"       # export
declare -f function_name        # print function definition
declare -F                      # list all function names
```

### `eval`
```bash
cmd="echo hello"
eval "$cmd"                     # echo hello (use with caution!)

var="greeting"
eval "${var}='Hello'"
echo "$greeting"                # Hello

# Safer alternative: use indirect expansion or namrefs
```

### `exec`
```bash
exec > output.txt 2>&1          # redirect all output for this shell
exec 3< file.txt                # open file on fd 3
read -u 3 line                  # read from fd 3
exec 3<&-                       # close fd 3

exec bash                       # replace current shell with new bash
exec python3 script.py          # replace shell with python process
```

### `ulimit` — Resource Limits
```bash
ulimit -a                       # show all limits
ulimit -n 65535                 # max open files
ulimit -u 200                   # max user processes
ulimit -s unlimited             # unlimited stack size
ulimit -f 1024                  # max file size in blocks (512B)
```

### `type` / `command` / `hash`
```bash
type ls                         # ls is aliased to `ls --color=auto`
type -t ls                      # alias
command ls                      # bypass alias, run the real command
command -v python3              # like which, returns path
hash                            # show hashed command locations
hash -r                         # clear hash table
```

### `read` Advanced
```bash
read -p "Prompt: " var
read -s -p "Password: " pass
read -t 30 -p "30s timeout: " ans
read -n 1 char                  # single character
read -d '' multiline            # read until null byte
read -a array                   # read words into array
IFS=: read -r user pass uid gid <<< "root:x:0:0"   # split on :
```

### `printf` Advanced
```bash
printf "%d\n" 0x1F              # 31 (hex to decimal)
printf "0x%X\n" 31              # 0x1F (decimal to hex)
printf "%010.4f\n" 3.14         # 0003.1400
printf "%q\n" "hello world"     # shell-quoted: 'hello world'
printf -v myvar "Value: %d" 42  # store in variable (no echo needed)
```

---

## 22. Shell Options

### `shopt` — Bash-Specific Options
```bash
shopt -s option          # enable
shopt -u option          # disable
shopt option             # query

shopt -s autocd          # cd by typing directory name
shopt -s cdspell         # fix minor spelling errors in cd
shopt -s cmdhist         # save multi-line commands as one history entry
shopt -s dotglob         # globs include hidden files
shopt -s extglob         # extended globbing patterns
shopt -s failglob        # error if glob matches nothing
shopt -s globstar        # ** recursive globbing
shopt -s histappend      # append to history file (don't overwrite)
shopt -s nocaseglob      # case-insensitive globbing
shopt -s nullglob        # unmatched globs expand to empty
shopt -s progcomp        # programmable completion
```

### `set` — POSIX Options
```bash
set -e                   # exit on error (errexit)
set -u                   # error on unset variables (nounset)
set -x                   # trace commands (xtrace)
set -v                   # verbose
set -n                   # no execution (dry run)
set -o pipefail          # pipe fails if any command fails
set -o noglob            # disable globbing
set -o noclobber         # prevent > from overwriting files
set +e                   # disable errexit
```

---

## 23. Job Control

```bash
command &                        # run in background
jobs                             # list background jobs
jobs -l                          # with PIDs
fg %1                            # bring job 1 to foreground
bg %1                            # send job 1 to background
kill %1                          # kill job 1
disown %1                        # detach job from shell (survives logout)
Ctrl+Z                           # suspend foreground job
Ctrl+C                           # kill foreground job

# Wait for specific job
wait %1
wait $!                          # wait for last background job
wait                             # wait for all background jobs
echo "Exit: $?"

# Run parallel jobs with limit
run_with_limit() {
    local limit=$1
    shift
    local jobs=0
    for cmd in "$@"; do
        eval "$cmd" &
        (( jobs++ ))
        if [[ $jobs -ge $limit ]]; then
            wait -n                 # wait for any one job (Bash 4.3+)
            (( jobs-- ))
        fi
    done
    wait
}
```

---

## 24. Signal Handling (trap)

### Signal Names
```bash
kill -l                          # list all signal names

# Common signals:
# SIGHUP (1)   — terminal hangup, also reload config
# SIGINT (2)   — Ctrl+C
# SIGQUIT (3)  — Ctrl+\
# SIGTERM (15) — graceful termination (default kill)
# SIGKILL (9)  — force kill (cannot be trapped)
# SIGUSR1/2    — user-defined
# SIGPIPE (13) — broken pipe
# SIGCHLD (17) — child process ended
```

### `trap` Usage
```bash
trap 'echo "Ctrl+C pressed"' INT           # handle Ctrl+C
trap 'echo "Done"; cleanup' EXIT           # always run on exit
trap 'echo "Error on line $LINENO"' ERR    # handle errors
trap 'reload_config' HUP                   # reload on HUP
trap '' INT                                # ignore Ctrl+C

# Multiple signals
trap cleanup EXIT INT TERM

# Reset a trap
trap - INT

# Trap in functions
cleanup_and_exit() {
    echo "Signal received, cleaning up..."
    rm -rf /tmp/workdir.$$
    exit 0
}
trap cleanup_and_exit INT TERM
```

### Graceful Shutdown Pattern
```bash
#!/usr/bin/env bash

RUNNING=true

function stop_gracefully() {
    echo "Stopping..."
    RUNNING=false
}

trap stop_gracefully SIGTERM SIGINT

while $RUNNING; do
    echo "Working..."
    sleep 1
done

echo "Exited cleanly"
```

---

## 25. Coprocesses

```bash
# Launch a coprocess (bidirectional pipe)
coproc PROC { cat; }

echo "hello" >&"${PROC[1]}"     # write to coprocess stdin
read -ru "${PROC[0]}" line       # read from coprocess stdout
echo "Got: $line"

kill $PROC_PID

# More practical example with bc
coproc BC { bc -l; }
echo "scale=4; 22/7" >&"${BC[1]}"
read -ru "${BC[0]}" result
echo "Pi ≈ $result"
kill $BC_PID
```

---

## 26. Associative Arrays (Dictionaries)

```bash
# Declare
declare -A person
person[name]="Alice"
person[age]=30
person[city]="New York"

# Or initialize with literal
declare -A colors=(
    [red]="#FF0000"
    [green]="#00FF00"
    [blue]="#0000FF"
)

# Access
echo "${person[name]}"           # Alice
echo "${colors[red]}"            # #FF0000

# All keys
echo "${!person[@]}"             # name age city

# All values
echo "${person[@]}"              # Alice 30 New York

# Number of elements
echo "${#person[@]}"             # 3

# Check if key exists
[[ -v person[name] ]] && echo "key exists"

# Iterate
for key in "${!person[@]}"; do
    echo "$key = ${person[$key]}"
done

# Delete element
unset person[city]

# Delete entire array
unset person

# Merge arrays
declare -A merged=()
for k in "${!arr1[@]}"; do merged[$k]="${arr1[$k]}"; done
for k in "${!arr2[@]}"; do merged[$k]="${arr2[$k]}"; done
```

---

## 27. File Operations in Scripts

### Temp Files & Directories
```bash
tmpfile=$(mktemp)                   # /tmp/tmp.XXXXXXXX
tmpdir=$(mktemp -d)                 # /tmp/tmp.XXXXXXXX (directory)
tmpfile=$(mktemp /tmp/myapp.XXXXXX) # custom prefix
trap "rm -f $tmpfile" EXIT          # always clean up

# Named temp file
tmpfile=$(mktemp -p /var/tmp myapp.XXXXXX)
```

### Reading Files Safely
```bash
# Read line by line
while IFS= read -r line; do
    echo "Line: $line"
done < file.txt

# Read with null delimiter (handles filenames with spaces)
while IFS= read -r -d '' file; do
    echo "File: $file"
done < <(find . -name "*.txt" -print0)

# Read first line
IFS= read -r first_line < file.txt

# Read all into variable
content=$(< file.txt)               # faster than cat
```

### Writing Files
```bash
echo "content" > file.txt           # write
echo "more" >> file.txt             # append
printf "formatted %s\n" "data" > file.txt

# Write multiple lines
cat > config.ini <<EOF
[server]
host = localhost
port = 8080
EOF

# Atomic write (avoid partial writes)
tmpfile=$(mktemp)
echo "data" > "$tmpfile"
mv "$tmpfile" config.txt            # atomic rename
```

### File Locking
```bash
LOCKFILE=/tmp/myapp.lock

# Simple lock with mkdir
if mkdir "$LOCKFILE" 2>/dev/null; then
    trap "rmdir '$LOCKFILE'" EXIT
    echo "Got lock"
    # ... do work ...
else
    echo "Already running"
    exit 1
fi

# flock (more robust)
exec 200>"$LOCKFILE"
flock -n 200 || { echo "Already running"; exit 1; }
echo $$ > "$LOCKFILE"
```

---

## 28. Networking in Bash

### `/dev/tcp` and `/dev/udp`
```bash
# Check if port is open
(echo >/dev/tcp/localhost/80) 2>/dev/null && echo "port 80 open"

# Simple HTTP request
exec 3<>/dev/tcp/example.com/80
printf "GET / HTTP/1.0\r\nHost: example.com\r\n\r\n" >&3
cat <&3
exec 3>&-

# Check multiple ports
for port in 22 80 443 8080; do
    (echo >/dev/tcp/192.168.1.100/$port) 2>/dev/null \
        && echo "Port $port: open" \
        || echo "Port $port: closed"
done
```

### `curl` in Scripts
```bash
# Check HTTP status
status=$(curl -s -o /dev/null -w "%{http_code}" https://example.com)
[[ "$status" == "200" ]] || { echo "Site down: $status"; exit 1; }

# Download with error handling
if ! curl -fsSL -o file.zip https://example.com/file.zip; then
    echo "Download failed"
    exit 1
fi

# API call
response=$(curl -s -H "Authorization: Bearer $TOKEN" \
    -H "Content-Type: application/json" \
    https://api.example.com/data)

echo "$response" | jq '.items[].name'
```

### Sending Mail (mailx/sendmail)
```bash
echo "Body text" | mail -s "Subject" user@example.com
echo "Body" | sendmail user@example.com
```

---

## 29. Date & Time in Scripts

```bash
# Current timestamp
now=$(date +%s)                     # Unix epoch
date "+%Y-%m-%d"                    # 2024-01-15
date "+%Y-%m-%d %H:%M:%S"          # 2024-01-15 10:30:00
date "+%A, %B %d %Y"               # Monday, January 15 2024

# Date arithmetic
yesterday=$(date -d "yesterday" +%Y-%m-%d)
next_week=$(date -d "+7 days" +%Y-%m-%d)
two_hours_ago=$(date -d "2 hours ago" +%s)

# Measure execution time
start=$(date +%s%N)                 # nanoseconds
sleep 1
end=$(date +%s%N)
elapsed=$(( (end - start) / 1000000 ))
echo "Elapsed: ${elapsed}ms"

# Or use SECONDS built-in
SECONDS=0
sleep 2
echo "Elapsed: ${SECONDS}s"

# Format epoch to date
date -d @1705276200 "+%Y-%m-%d %H:%M:%S"

# ISO 8601 with timezone
date --iso-8601=seconds             # 2024-01-15T10:30:00+00:00
```

---

## 30. Common Script Patterns & Recipes

### Script Template
```bash
#!/usr/bin/env bash
set -euo pipefail
IFS=$'\n\t'

readonly SCRIPT_NAME=$(basename "$0")
readonly SCRIPT_DIR="$(cd "$(dirname "${BASH_SOURCE[0]}")" && pwd)"

function usage() {
    cat <<EOF
Usage: $SCRIPT_NAME [OPTIONS] <arg>

Options:
  -h, --help     Show this help
  -v, --verbose  Verbose output
  -o <file>      Output file
EOF
}

VERBOSE=0
OUTPUT=""

while [[ $# -gt 0 ]]; do
    case "$1" in
        -h|--help) usage; exit 0 ;;
        -v|--verbose) VERBOSE=1; shift ;;
        -o) OUTPUT="$2"; shift 2 ;;
        --) shift; break ;;
        -*) echo "Unknown option: $1" >&2; usage; exit 1 ;;
        *) break ;;
    esac
done

function log() { echo "[$(date '+%H:%M:%S')] $*"; }
function info() { log "INFO: $*"; }
function warn() { log "WARN: $*" >&2; }
function error() { log "ERROR: $*" >&2; exit 1; }
function debug() { [[ $VERBOSE -eq 1 ]] && log "DEBUG: $*" || true; }
```

### Retry Logic
```bash
function retry() {
    local max=$1
    local delay=$2
    shift 2
    local attempt=1
    until "$@"; do
        if (( attempt >= max )); then
            echo "Command failed after $max attempts" >&2
            return 1
        fi
        echo "Attempt $attempt failed, retrying in ${delay}s..."
        sleep "$delay"
        (( attempt++ ))
    done
}

retry 5 3 curl -fsSL https://example.com/api/health
```

### Parallel Processing
```bash
# Simple background jobs
pids=()
for item in "${items[@]}"; do
    process "$item" &
    pids+=($!)
done

# Wait and check
for pid in "${pids[@]}"; do
    wait "$pid" || echo "Job $pid failed"
done

# With concurrency limit
MAX_JOBS=4
running=0

for file in *.txt; do
    process_file "$file" &
    (( running++ ))
    if (( running >= MAX_JOBS )); then
        wait -n                      # wait for any job to finish
        (( running-- ))
    fi
done
wait
```

### Progress Bar
```bash
function progress_bar() {
    local current=$1
    local total=$2
    local width=50
    local percent=$(( current * 100 / total ))
    local filled=$(( current * width / total ))
    local bar=$(printf '%*s' "$filled" | tr ' ' '█')
    local empty=$(printf '%*s' "$(( width - filled ))")
    printf "\r[%s%s] %d%%" "$bar" "$empty" "$percent"
    [[ $current -eq $total ]] && echo
}

total=100
for i in $(seq 1 $total); do
    sleep 0.05
    progress_bar $i $total
done
```

### Configuration File Parsing
```bash
# Parse KEY=VALUE config file
function load_config() {
    local config_file=$1
    while IFS='=' read -r key value; do
        [[ "$key" =~ ^[[:space:]]*# ]] && continue   # skip comments
        [[ -z "$key" ]] && continue                    # skip empty lines
        key=$(echo "$key" | tr -d '[:space:]')
        value=$(echo "$value" | sed 's/^[[:space:]]*//;s/[[:space:]]*$//')
        export "$key=$value"
    done < "$config_file"
}

load_config app.conf
echo "$DATABASE_HOST"
```

### Spinner for Long-Running Commands
```bash
function spinner() {
    local pid=$1
    local chars="⠋⠙⠹⠸⠼⠴⠦⠧⠇⠏"
    local i=0
    while kill -0 "$pid" 2>/dev/null; do
        printf "\r%s Working..." "${chars:$i:1}"
        i=$(( (i + 1) % ${#chars} ))
        sleep 0.1
    done
    printf "\r✓ Done!          \n"
}

long_command &
spinner $!
wait $!
```

### Colorized Output
```bash
RED='\033[0;31m'
GREEN='\033[0;32m'
YELLOW='\033[1;33m'
BLUE='\033[0;34m'
BOLD='\033[1m'
NC='\033[0m'    # No Color

echo -e "${GREEN}✓ Success${NC}"
echo -e "${RED}✗ Error${NC}"
echo -e "${YELLOW}⚠ Warning${NC}"
echo -e "${BOLD}Bold text${NC}"

function print_color() {
    local color=$1
    shift
    echo -e "${color}$*${NC}"
}

print_color "$GREEN" "All tests passed"
print_color "$RED" "Build failed"
```

### Validate Required Tools
```bash
function require_tools() {
    local missing=()
    for tool in "$@"; do
        command -v "$tool" &>/dev/null || missing+=("$tool")
    done
    if [[ ${#missing[@]} -gt 0 ]]; then
        echo "Required tools not found: ${missing[*]}" >&2
        exit 1
    fi
}

require_tools curl jq git docker
```

### Self-Updating Script
```bash
function update_self() {
    local script_url="https://example.com/scripts/myscript.sh"
    local tmpfile
    tmpfile=$(mktemp)
    curl -fsSL "$script_url" -o "$tmpfile"
    chmod +x "$tmpfile"
    mv "$tmpfile" "$0"
    echo "Script updated"
    exec "$0" "$@"              # restart with new version
}
```

### Logging to File and Terminal
```bash
LOG_FILE="/var/log/myapp.log"

exec > >(tee -a "$LOG_FILE") 2>&1    # all output goes to file AND terminal

echo "Script started at $(date)"
echo "This appears on screen and in log"
```

### Version Comparison
```bash
function version_ge() {
    # Returns 0 if $1 >= $2 (semantic version)
    printf '%s\n%s\n' "$2" "$1" | sort -V -C
}

if version_ge "$(bash --version | head -1 | grep -oP '\d+\.\d+')" "4.3"; then
    echo "Bash 4.3+ features available"
fi
```

### JSON Generation (without jq)
```bash
function to_json() {
    local name="$1"
    local value="$2"
    printf '"%s": "%s"' "$name" "$value"
}

printf '{\n  %s,\n  %s\n}\n' \
    "$(to_json "name" "Alice")" \
    "$(to_json "city" "New York")"
```

### Checksum Validation
```bash
function verify_checksum() {
    local file=$1
    local expected_sha256=$2
    local actual
    actual=$(sha256sum "$file" | awk '{print $1}')
    if [[ "$actual" != "$expected_sha256" ]]; then
        echo "Checksum mismatch for $file" >&2
        echo "  Expected: $expected_sha256" >&2
        echo "  Actual:   $actual" >&2
        return 1
    fi
    echo "Checksum OK: $file"
}
```

---

*Last updated: 2026 | Bash 5.x | Works on Linux and macOS (minor differences in date/sed on macOS)*
