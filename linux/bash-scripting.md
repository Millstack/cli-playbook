# Bash Scripting Basics

## 1. Fundamentals & The Environment

**The Shell**: Linux uses the Bourne Again Shell (Bash), an evolution of the original Bourne Shell (`sh`).

**Locating Services** : Before interacting with scripts or binaries, use the which command to locate the executable file associated with a command.

```bash
which bash  # Returns the path, usually /usr/bin/bash
```

**The `.sh` Extension**: While not mandatory for Linux to run the file, it is a professional convention for human readability and IDE syntax highlighting.
It serves as metadata for:
- **Readability**: Instantly identifies the file type for developers.
- **Syntax Highlighting**: Triggers text editors (like VS Code or Vim) to apply color coding.

<br>

**The Shebang (`#!`)**
Every professional script starts with a Shebang.
- **Purpose**: It tells the OS exactly which interpreter to use or Tells the kernel which interpreter to use to execute the script.
- **Convention**: Always place this on Line 1.
- *Why it's a convention*: Even if your current shell is Bash, another user might be using Zsh or Dash. The Shebang ensures the script runs in the intended environment every time.

<br>

Users should start every file with this professional block, to give an overview idea about why the script is written and its purpose

```bash
#!/bin/bash
# =================================================================
# Script Name:  file_name.sh
# Description:  eg: download and execute .net runtime on ec2
# Author:       author_nme
# Date:         09-02-2026
# =================================================================
set -e  # Exit immediately if a command exits with a non-zero status.
```

<br>

## 2. Professional Script Execution

There are two primary ways to execute your scripts:
1. **The Direct Method**: `sh file_name.sh` (Runs via the interpreter).
   ```bash
    sh file_name.sh
   ```
2. **The Executable Method**: `./file_name.sh` (Requires the file to have execute permissions).
   ```bash
    ./file_name.sh
   ```
3. **Using `sudo`** to give admin persmission while executing the file (possibly commands on admin level)
   ```bash
    sudo ./file_name.sh
   ```
4. Executing script from **different directory**, but one must know the file path to execute it from different location
   ```bash
     # you should have the file and directory permission to able to access and then exuete the intended file
    sudo /home/ubuntu/folder_name/file_name.sh
   ```

<br>

> [!NOTE]
> - first check the newly created or existing file permission  
> - Use `chmod 755 file_name.sh` (to make a script executable).  
> - also check the user & groups to make sure, you can execute/edit the file  

<br>

## 3. Data Handling & Variables

Variables allow your scripts to be dynamic rather than hard-coded. Following are the use of variables
- to hold data one time, and repeate the variable instead of hard-coded data throughout the script
- to get dyunamic values (not knwon while creating the script / output of a command)
- assign value once, use it multiple times

<br>

### Types of Variables:
1. `User Defined`: Created by user eg:
   ```bash
   #!/bin/bash
   name='nginx' #string value
   versionnn=2 #numeric value
   ```
3. `Environment`: Built-in system variables eg:
   ```bash
   #!/bin/bash
   $USER #current user
   $HOME #home directory
   ```
5. `Readonly`: Constants that cannot be changed once defined, eg:
   ```bash
   #!/bin/bash
   readonly varName='Value'
   ```
<br>


### User Input
Instead of hard-coding values in a script, capture them from the user (while running the script command)
Types of user input:
1. Standard
   ```bash
   #!/bin/bash
   echo "Enter Service Name :"
   read varName
   ```
3. Professional (Prompted)
   ```bash
   #!/bin/bash
   read -p "Enter Service Name: " serviceName
   # combines echo and read
   ```

<br>

## 4. Script Arguments (Runtime Inputs)

Arguments allow you to pass data into a script at the moment of execution using an index-based approach.  
In Bash scripting, arguments passed to a script are referred to as `Positional Parameters` ($0, $1, $2, etc.)  
They allow your scripts to be dynamic—instead of hardcoding a filename, you pass it as an argument so the script can work on any file.  

1. Index Breakdown:
   - `$0`: The name of the script itself (example ./install-script.sh)
   - `$1`, `$2`, `$3`... `$9`: The arguments from index 1 to index 9
   - `${10}`, `${11}`...`${n}`: For any argument beyond 9, need to use curly braces

2. Special Helpers:
   - `$#`: Returns the number of arguments passed. Crucial for validation
   - `$@`: Represents all arguments as separate strings. Use this for loops
   - `$*`: Represents all arguments as a single string

3. The shift Command:
   This is used to "`pop`" the first argument off the list.  
   After running shift, what was `$2` becomes `$1`.  
   This is common in loops that process one argument at a time.

<br>

### Industrial Standard Approach

In professional environments (DevOps, System Admin), simply using `$1` and `$2` is often considered "quick and dirty."  
Standard scripts follow these rules: 

- `Validation First`: Always check if the required number of arguments is present using `$#`
- `Variable Assignment`: Immediately assign positional parameters to descriptive variables (e.g., DB_NAME=$1) so the code is readable
- `Using getopts`: For scripts with many options (like -f file.txt or -v), the industry standard is the getopts builtin. It handles flags and arguments much more robustly than manual indexing

example:
```bash
#!/bin/bash
# Example: Standard argument handling

# 1. Validate argument count
if [ "$#" -lt 2 ]; then
    echo "Usage: $0 <source_file> <target_dir>"
    exit 1
fi

# 2. Assign to descriptive variables
SOURCE_FILE="$1"
TARGET_DIR="$2"

echo "Moving $SOURCE_FILE to $TARGET_DIR..."
```

<br>

Brief Notes (Cheat Sheet)

### Bash Positional & Special Parameters Reference

| **Parameter**  | **Description**     | **Use Case**                                                                  |
| :---           | :---                | :---                                                                          |
| **`$0`**       | Script Name         | Used in usage messages: `echo "Usage: $0 [file]"`                             |
| **`$1 - $9`**  | Arguments 1-9       | Accessing specific inputs (e.g., source and destination).                     |
| **`${10}+`**   | Arguments 10+       | Accessing arguments beyond 9 (requires curly braces).                         |
| **`$#`**       | Argument Count      | Validation: `if [ "$#" -ne 2 ]; then exit 1; fi`                              |
| **`$@`**       | All Arguments       | Iterating: `for arg in "$@"; do ... done` (treats each as a separate word).   |
| **`$*`**       | All Arguments       | Single string: Used when you want all inputs as one blob of text.             |
| **`$?`**       | Exit Status         | Success check: `if [ $? -eq 0 ]; then echo "Success"; fi`                     |
| **`$$`**       | Process ID (PID)    | Creating unique temp files: `temp_file="/tmp/tmp.$$"`                         |
| **`$!`**       | Last Background PID | Tracking the ID of the last job sent to the background (`&`).                 |

<br>

### Standard Pattern

This shows how these parameters work together in a production-ready script
1. `The Usage Pattern`: Use `$0` and `$#` to guide the user
2. `The Wrapper Pattern`: Use `"$@"` to pass all arguments from one script/function to another command (maintaining spaces in filenames)
3. `The Cleanup Pattern`: Use `$$` to ensure that if multiple instances of your script run, they don't overwrite each other's temporary files

<br>

>[!NOTE]
> * Always use `"$1"` instead of `$1` to handle filenames that contain spaces
> * Use `--` (Guard) if you are passing arguments to another command to prevent them from being interpreted as flags
> * Define a `usage()` function that uses $0 to tell the user how to correctly run the script
> * `${10}` is mandatory; `$10` without braces is interpreted by Bash as `$1` followed by a literal `0`
> * Use the `shift` command to move arguments to the left (`$2` becomes `$1`) when processing long lists of files.






