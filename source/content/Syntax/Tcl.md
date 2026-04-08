
A Practical Guide to Tcl Scripting: Syntax and Templates
Tcl (Tool Command Language) is a powerful, dynamic scripting language known for its simple syntax and easy integration with other applications, especially in the domain of electronic design automation (EDA) and testing. This guide provides a collection of the most common syntax patterns and templates.

1. Tcl Fundamentals: The "Everything is a Command" Principle
In Tcl, every statement is a command, including variable assignments, loops, and procedure definitions. A command is a list of words separated by spaces, where the first word is the command name and subsequent words are its arguments.

Basic Script Structure
```tcl
# The 'puts' command prints a string to standard output.
puts "Hello, World!"
```

Variables and Substitution
`set`: Assigns a value to a variable. Quotes are optional unless there are spaces/special characters.
`$`: Substitutes the value of a variable.
`[]`: Command Substitution. Executes the command inside the brackets and substitutes its result.
`{}`: Groups words into a single argument without performing substitutions. Treats the content as a literal string.
`""`: Groups words into a single argument with substitutions.

```tcl
# Variable assignment
set user "Alex" # Quotes are optional unless there are spaces/special characters

# Variable substitution
puts "Hello, $user"

# Command substitution
set num_files [llength [glob *.tcl]]
puts "There are $num_files Tcl files in this directory."

# Grouping with "" (substitutes $user)
set message "User: $user"
puts $message # Prints "User: Alex"

# Grouping with {} (does NOT substitute $user)
set literal_message {User: $user}
puts $literal_message # Prints "User: $user"
```

2. Variables and Data Structures
Simple Variables and Expressions
```tcl
set a 10
set b 5

# The 'expr' command evaluates a mathematical expression
set sum [expr {$a + $b}] # The literal "$a + $b" is evaluated by expr's internel parser
set product [expr {$a * $b}]

puts "Sum is: $sum"
```

Lists
```tcl
# Create a list
set my_list {apple banana cherry}
# or
set my_list [list "apple" "banana" "cherry"]

# Add an element to a list
lappend my_list "orange"

# Get the length of a list
set len [llength $my_list]

# Get an element by index (0-based)
set first_item [lindex $my_list 0]
puts "The first item is: $first_item"
```

Arrays (Associative Arrays)
Arrays in Tcl are key-value pairs, similar to dictionaries or hash maps in other languages.
```tcl
# Set array elements
set user_data(name) "Alice"
set user_data(id) "12345"
set user_data(email) "alice@example.com"

# Access an array element
puts "User's email is: $user_data(email)"

# Check if a key exists
if {[info exists user_data(id)]} {
    puts "User ID exists."
}

# Get a list of all keys in the array
set all_keys [array names user_data]
puts "Available user data keys: $all_keys" # name id email

# A handy way to print all array contents
parray user_data
```

3. Control Structures
Note the required spacing and the use of {} to group the command blocks.
```tcl
set score 85

if {$score >= 90} {
    puts "Grade: A"
} elseif {$score >= 80} {
    puts "Grade: B"
} else {
    puts "Grade: C or lower"
}
```

`foreach` Loop (for lists)
Loops can be controlled using `break` and `continue`.
```tcl
set servers {serverA serverB serverC}

foreach server $servers {
    puts "Processing server: $server"
}
```

`for` Loop (C-style)
```tcl
# Loop from 0 to 4
for {set i 0} {$i < 5} {incr i} {
    puts "Iteration: $i"
}
```

`while` Loop
```tcl
set count 3
while {$count > 0} {
    puts "Countdown: $count"
    incr count -1
}
```

`switch` Statement
A powerful way to match a value against multiple patterns.

```tcl
set fruit "apple"

switch $fruit {
    apple {
        puts "It's a red fruit."
    }
    banana {
        puts "It's a yellow fruit."
    }
    grape -
    cherry {
        puts "It's a small fruit."
    }
    default {
        puts "Unknown fruit."
    }
}
```

4. Procedures (Functions)
Procedures are defined with proc.
```tcl
# --- Define a procedure with one required and one optional argument
proc greet {name {greeting "Hello"}} {
    return "$greeting, $name!"
}

# --- Call the procedure
set message1 [greet "World"]
puts $message1 ;# Prints "Hello, World!"

set message2 [greet "Alice" "Greetings"]
puts $message2 ;# Prints "Greetings, Alice!"

# Use sepcial name 'args' to accent any number of trailing arguments
proc sum {args} {
    set total 0 
    foreach num $args {
        incr total $num 
    } 
    return $total 
} 
puts [sum 10 20 30] ;# Output: 60

# Use 'global' keyword to make variable access value outside its scope
proc example {args} {
    global var
    puts var
}
```

5. File I/O Recipes
Reading a File Line by Line
```tcl
set filename "my_data.txt"

if {![file exists $filename]} {
    puts "Error: File '$filename' not found."
    return
}

set f [open $filename r]
while {[gets $f line] >= 0} {
    # Process each line
    puts "Read line: $line"
}
close $f
```

Writing to a File
```tcl
set filename "output.log"

# 'w' for write (overwrites), 'a' for append
set f [open $filename w]

puts $f "This is the first line."
puts $f "This is the second line."

close $f
puts "Wrote data to $filename"
```

6. String Manipulation
Basic String Commands
```tcl
set my_string "This is a Tcl script"

puts "Length: [string length $my_string]"
puts "Substring: [string range $my_string 5 6]" ;# Prints "is"
puts "Index of 'Tcl': [string first "Tcl" $my_string]"

Regular Expression Matching
set text "My IP is 192.168.1.100"

# Use regexp to check for a match and extract parts
if {[regexp {([0-9]{1,3})\.([0-9]{1,3})\.([0-9]{1,3})\.([0-9]{1,3})} $text match first_octet]} {
    puts "IP Address Found: $match"
    puts "First octet is: $first_octet"
}
```

7. Executing External Commands
The exec command runs an external program. It's often wrapped in a catch block to handle errors gracefully.
```tcl
# The 'catch' command executes a script and traps any errors.
# It returns 0 if no error, non-zero otherwise. The result or error message is stored in 'result'.
if {[catch {exec ls -l *.tcl} result]} {
    puts "Error executing command: $result"
} else {
    puts "Command successful. Output:"
    puts $result
}
```

Automatically created variables in a script.
- `$argc` - Number of command-line arguments passed to the script.
- `$argv` - List of command-line arguments.
- `$argv0` - Name of the script being executed.
- `$env` - Array containing environment variables. Access like `$env(PATH`, `$env(HOME)`
- `$auto_path` - list of directories to search for packages

Other Notes
- User defined namespaces can be used to avoid conflicting the global space
- Weird ass notation for scope names with `::`. The first `::` refers to the global namespace, so you can also write `namespace eval ::reporting` to define the namespace relative to the global scope, instead of the possible sub-scope when it is "imported".
```tcl
# 'namespace eval' is the command name
namespace eval reporting {
    # This variable lives inside the 'reporting' namespace
    set version 1.0

    # This procedure lives inside the 'reporting' namespace
    proc generate {data} {
        # ... implementation ...
        return "Report v$::reporting::version generated."
    }
}

# To access the variable:
puts $::reporting::version

# To call the procedure:
set report [::reporting::generate "some_data"]
puts $report
```

- Importing packages (packages are not the same as user defined namespaces)
```tcl
# Request version 8.6 or newer of the Tk package.
package require Tk 8.6

# Now that the package is loaded, we can use commands from it, like 'label'.
label .myLabel -text "Hello, GUI World!"
pack .myLabel
```

# List of common commands

This guide provides a detailed list of common Tcl commands whose primary purpose is to **return a value**, making them ideal candidates for use within `[]` command substitution blocks.

## 1. List Manipulation

These commands are the workhorses for handling ordered collections of data.

| Command     | Syntax & Purpose                                                                                                         | Example & Return Value                                                    |
| :---------- | :----------------------------------------------------------------------------------------------------------------------- | :------------------------------------------------------------------------ |
| **llength** | `llength list` <br> Returns the number of elements in a list.                                                            | `set n [llength {a b c}]` <br> *Returns:* `3`                             |
| **lindex**  | `lindex list index` <br> Returns the element at a specific `index`.                                                      | `set item [lindex {a b c} 1]` <br> *Returns:* `b`                         |
| **lrange**  | `lrange list first last` <br> Returns a new list containing a range of elements.                                         | `set sub [lrange {a b c d} 1 2]` <br> *Returns:* `b c`                    |
| **lsearch** | `lsearch ?options? list pattern` <br> Finds an element matching a `pattern` and returns its index, or `-1` if not found. | `set idx [lsearch {a b c} b]` <br> *Returns:* `1`                         |
| **lsort**   | `lsort ?options? list` <br> Returns a new list with the elements sorted.                                                 | `set sorted [lsort -integer {5 1 3}]` <br> *Returns:* `1 3 5`             |
| **join**    | `join list ?joinString?` <br> Joins the elements of a list into a single string.                                         | `set str [join {a b c} ","]` <br> *Returns:* `a,b,c`                      |
| **split**   | `split string ?splitChars?` <br> Splits a string into a list.                                                            | `set list [split "a,b,c" ","]` <br> *Returns:* `a b c`                    |
| **list**    | `list ?arg1 arg2 ...?` <br> Creates a properly escaped list from its arguments.                                          | `set list [list "An item" "Another"]` <br> *Returns:* `{An item} Another` |

## 2. String Manipulation

Essential for parsing and formatting text.

| Command          | Syntax & Purpose                                                                                                                                | Example & Return Value                                                                    |
|:---------------- |:----------------------------------------------------------------------------------------------------------------------------------------------- |:----------------------------------------------------------------------------------------- |
| **string**       | `string sub-command string ?args?` <br> A suite of commands for string operations.                                                              |                                                                                           |
| &nbsp; `length`  | `string length string`                                                                                                                          | `set len [string length "Tcl"]` <br> *Returns:* `3`                                       |
| &nbsp; `range`   | `string range string first last`                                                                                                                | `set sub [string range "abcde" 1 3]` <br> *Returns:* `bcd`                                |
| &nbsp; `map`     | `string map {charMap} string` <br> Replaces multiple characters in one pass.                                                                    | `set new [string map {a 1 b 2} "abc"]` <br> *Returns:* `12c`                              |
| &nbsp; `trim`    | `string trim string ?chars?` <br> Removes leading/trailing characters (whitespace by default).                                                  | `set clean [string trim "  hi  "]` <br> *Returns:* `hi`                                   |
| &nbsp; `toupper` | `string toupper string` <br> Make an uppercase string.                                                                                          | `set var [string toupper "hi"]` <br> *Returns:* `HI`                                          |
| **format**       | `format formatString ?val...?` <br> Creates a formatted string like C's `sprintf`.                                                              | `set hex [format "0x%04X" 255]` <br> *Returns:* `0x00FF`                                  |
| **scan**         | `scan string format ?var...?` <br> Parses a string according to a format and populates variables. Returns the number of successful conversions. | `set n [scan "id:123" "id:%d" my_id]` <br> *Returns:* `1` (and sets `my_id` to `123`)     |
| **regexp**       | `regexp ?opts? exp str ?match...?` <br> Matches a regular expression. Returns `1` on success, `0` on failure. Populates match variables.        | `set found [regexp {(\d+)} "age:42" -> num]` <br> *Returns:* `1` (and sets `num` to `42`) |

## 3. File System Interaction

For querying and manipulating files and directories.

| Command          | Syntax & Purpose                                                                                                                                                                  | Example & Return Value                                                        |
| :--------------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :---------------------------------------------------------------------------- |
| **file**         | `file sub-command name ?args?` <br> A suite of commands for file attributes.                                                                                                      |                                                                               |
| &nbsp; `exists`  | `file exists name`                                                                                                                                                                | `set found [file exists data.txt]` <br> *Returns:* `1` or `0`                 |
| &nbsp; `size`    | `file size name`                                                                                                                                                                  | `set bytes [file size data.txt]` <br> *Returns:* The file size in bytes.      |
| &nbsp; `isdir`   | `file isdirectory name`                                                                                                                                                           | `set isdir [file isdirectory /etc]` <br> *Returns:* `1` or `0`                |
| &nbsp; `join`    | `file join path1 path2 ...` <br> Joins path components with the correct separator.                                                                                                | `set p [file join /home user file.txt]` <br> *Returns:* `/home/user/file.txt` |
| &nbsp; `dirname` | `file dirname path`                                                                                                                                                               | `set dir [file dirname /a/b/c.txt]` <br> *Returns:* `/a/b`                    |
| &nbsp; `tail`    | `file tail path`                                                                                                                                                                  | `set name [file tail /a/b/c.txt]` <br> *Returns:* `c.txt`                     |
| **glob**         | `glob ?switches? pattern` <br> Returns a list of files matching a wildcard pattern.                                                                                               | `set files [glob *.tcl]` <br> *Returns:* A list of Tcl files.                 |
| **pwd**          | `pwd` <br> Returns the current working directory.                                                                                                                                 | `set cwd [pwd]` <br> *Returns:* The full path of the current directory.       |
| **open**         | `open fileName ?access?` <br> Opens a file. Returns a channel handle (e.g., `file3`).                                                                                             | `set chan [open data.txt r]` <br> *Returns:* A channel ID like `file3`.       |
| **gets**         | `gets channelId ?varName?` <br> Reads a line from a channel. Returns the number of characters read, or a negative number on EOF. If `varName` is given, the line is stored there. | `set num_chars [gets $chan line]` <br> *Returns:* Number of chars read.       |

## 4. Introspection and State

For querying the state of the Tcl interpreter itself.

| Command | Syntax & Purpose | Example & Return Value |
| :--- | :--- | :--- |
| **info** | `info sub-command ?args?` <br> Provides information about the Tcl state. | |
| &nbsp; `exists`| `info exists varName` | `set v_exists [info exists my_var]` <br> *Returns:* `1` or `0` |
| &nbsp; `commands`| `info commands ?pattern?` | `set list_cmds [info commands l*]` <br> *Returns:* `list llength lindex lsort ...` |
| &nbsp; `procs`| `info procs ?pattern?` | `set my_procs [info procs]` <br> *Returns:* A list of all defined procedures. |
| &nbsp; `level` | `info level ?number?` | `set stack [info level 0]` <br> *Returns:* The command at the top of the call stack. |
| &nbsp; `script`| `info script` | `set script_path [info script]` <br> *Returns:* The path to the currently executing script. |

## 5. Execution and Flow Control

| Command   | Syntax & Purpose                                                                                                  | Example & Return Value                                                                                 |
|:--------- |:----------------------------------------------------------------------------------------------------------------- |:------------------------------------------------------------------------------------------------------ |
| **expr**  | `expr {expression}` <br> Evaluates a mathematical or logical expression.                                          | `set val [expr { (5 * 4) + 2 }]` <br> *Returns:* `22`                                                  |
| **catch** | `catch {script} ?resultVar?` <br> Executes a script and traps errors. Returns `0` on success, `1` on error.       | `set status [catch {set x [expr {1/0}]} msg]` <br> *Returns:* `1` (and sets `msg` to "divide by zero") |
| **exec**  | `exec ?switches? arg1 ?arg2 ...?` <br> Executes an external command. Returns the standard output of that command. | `set who [exec whoami]` <br> *Returns:* The current username.                                          |
| **incr**  | `incr varName ?increment?` <br> Increment `varName` by a specific amount. Default adds 1.                         | `set var [incr var 1]`                                                                                 |

## 6. Date and Time

| Command          | Syntax & Purpose                                                       | Example & Return Value                                                              |
| :--------------- | :--------------------------------------------------------------------- | :---------------------------------------------------------------------------------- |
| **clock**        | `clock sub-command ?args?` <br> Suite of commands for time operations. |                                                                                     |
| &nbsp; `seconds` | `clock seconds`                                                        | `set epoch [clock seconds]` <br> *Returns:* Integer seconds since 1970-01-01.       |
| &nbsp; `format`  | `clock format seconds ?opts?`                                          | `set date [clock format $epoch -format %Y-%m-%d]` <br> *Returns:* `2025-06-29`      |
| &nbsp; `scan`    | `clock scan dateString ?opts?`                                         | `set epoch [clock scan "2025-06-29"]` <br> *Returns:* The epoch time for that date. |

# Useful Vivado tcl commands

## Get objects

`get_cells`
`get_clocks`
`get_nets`
`get_pins`
`get_ports`
`get_timing_arcs`
`get_timing_paths`
`get_waves`

`all_clocks`
`all_cpus`
`all_dsps`
`all_fanin`
`all_fanout`
`all_ffs`
`all_hsios`
`all_inputs`
`all_latches`
`all_outputs`
`all_rams`
`all_registers`


## Selecting
`select_objects`
`highlight_objects`
`mark_objects`
`unselect_objects`
`unhighlight_objects`
`unmark_objects`
