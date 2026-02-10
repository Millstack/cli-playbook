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

## 3. Script Formatting In Editor

* **Single Line**: Use `#` for logic explanation
* **Block Comments**: Use a "`Here Document`" to disable large chunks of code. The first word/character should also be same at last.
  `example:`
  ```bash
   << COMMENT
      Everything in here is ignored by the shell.
      Useful for documentation or debugging.
   COMMENT
  ```

<br>

## 4. Text Editor: **Vim**

In the world of Linux and Cloud Engineering, the ability to edit files directly in the terminal is a non-negotiable skill. When you are SSH’d into a remote AWS EC2 instance or a production server, you won’t have a GUI (Graphical User Interface). You only have the command line.

### 1. Text Editors in the Industry
a. **CLI-Based** (Terminal): Operates entirely within the `shell`. Essential for remote server management
   * **`Vim/Vi`**: The industry standard. High learning curve, but extremely powerful
   * **`Nano`**: Simple and beginner-friendly, but lacks advanced automation features
   
<br>

b. **GUI-Based** (Local Development): Used on your local machine to write code before pushing to the cloud
   * **`VS Code`**: The most popular choice for Full Stack engineers due to its massive extension library
   * **`Sublime Text`**: Lightweight and fast

**Why Vim is Used Industry-Wide**  
* Vim is pre-installed on almost every Linux distribution and macOS
* **Resource Efficient**: It uses almost no RAM or CPU
* **Speed**: Once learned, you can edit code without ever touching your mouse
* **Ubiquity**: Whether you are on a legacy server from 2010 or a brand new Docker container, vi or vim will be there

### 2. Mastering Vim Logic: The "Modes"

Unlike VS Code, Vim has Modes. You cannot just start typing
1. **Normal Mode** (`Esc`): For navigating and deleting
2. **Insert Mode** (`i`): For actually typing text
3. **Command Mode** (`:`): For saving, quitting, and settings

### 3. Essential Vim Shortcuts Reference

Navigation (Normal Mode)

| Shortcut	| Action	                  | Cloud Use Case                               |
|-----------|--------------------------|----------------------------------------------|
| `gg`	   | Jump to the first line	| Checking the Shebang (#!/bin/bash).          |
| `G`	      | Jump to the last line	   | Checking the end of a long log file.         |
| `0`	      | Jump to start of line	   | Moving to the beginning of a command.        |
| `$`	      | Jump to end of line	   | Appending a redirect like 2> /dev/null.      |
| `w`	      | Jump word by word	      | Moving quickly through a long line of code   |

<br>

Editing & Survival

| Shortcut | Action                       | Note                                |
|----------|------------------------------|-------------------------------------|
| `i`      | Enter Insert Mode            | Start typing before the cursor.     | 
| `x`      | Delete a single character    | Quick fix for typos in Normal mode. | 
| `dd`     | Delete (Cut) an entire line  | Removes a line of code.             | 
| `yy`     | Copy (Yank) a line           | Used for duplicating config lines.  | 
| `p`      | Paste                        | Paste after the cursor.             | 
| `u`      | Undo                         | Revert your last mistake.           | 

<br>

Saving and Exiting (Command Mode)

| Command     | Action                                                | 
|-------------|-------------------------------------------------------|
| `:w `       | Save (Write).                                         | 
| `:q`        | Quit.                                                 | 
| `:w`q       | Save and Exit (The most used command).                | 
| `:q!`       | Force quit without saving (Use when you messed up).   | 

<br>

### 4. Searching & Large File Navigation

When a script or log file has 5,000 lines, you cannot scroll. You must `search`.

The Search Command: `/`

In Normal Mode, type `/ `followed by your `term` and hit Enter
* **Example**: `/DATABASE_URL` — This highlights all instances of that variable
* **Next Match**: Press `n` to jump to the next occurrence
* **Previous Match**: Press `N` (Shift + N) to jump back

<br>

**Partial Word & Pattern Searching**

* **Partial**: `/log` will find "logger", "logging", and "logfiles"
* **Start of Line**: `/^ERROR` finds only lines that start with the word "ERROR"
* **End of Line**: `/done$` finds only lines that end with the word "done"

<br>

## 5. The `.vimrc` Configuration File

* The `.vimrc` file is a script located in your home directory (`~/.vimrc` or `/home/ubuntu/.vimrc`)
* Vim reads this file every time it starts to apply your personal preferences.
* Why it matters for a Cloud Engineer:
  - When you are jumping between different environments, having a standard `.vimrc` makes every server feel like "home"
  - It ensures your tabs are consistent (so your Python/YAML code doesn't break) and makes code readable with line numbers.

<br>

### Creating your **`.vimrc`**
```bash
# Navigate to home and create the file
cd ~
vim .vimrc
```

Professional Industrial Standard Configuration
```bash
" --- Appearance ---
syntax on            " Enable syntax highlighting
set number           " Show line numbers on the left
set cursorline       " Highlight the current line for better visibility

" --- Indentation ---
set tabstop=4        " Number of spaces a tab counts for
set shiftwidth=4     " Number of spaces for auto-indent
set expandtab        " Convert tabs to spaces (Essential for YAML/CloudFormation)
set autoindent       " Copy indent from current line when starting a new one

" --- Search ---
set hlsearch         " Highlight all search matches
set incsearch        " Show matches as you type
set ignorecase       " Ignore case in search patterns
set smartcase        " Override 'ignorecase' if search contains capitals
```

<br>

>[!NOTE]
>* In the industry, engineers often keep their `.vimrc` in a GitHub Dotfiles repository.
>* When they join a new project or spin up a new server, they simply curl their config file from git repo

<br>

## 6. Data Handling & Variables

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
>[!WARNING]  
>  Assignment: No spaces around = (e.g., `NAME="MILIND"`, not `NAME = "MILIND"`)

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

## 7. Script Arguments (Runtime Inputs)

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
4. `Exit Status`: The most important "operator" in a sense is `$?`. In professional scripts, every major command is followed by a check to see if the previous `exit status` was `0`

<br>

>[!NOTE]
> * Always use `"$1"` instead of `$1` to handle filenames that contain spaces
> * Use `--` (Guard) if you are passing arguments to another command to prevent them from being interpreted as flags
> * Define a `usage()` function that uses $0 to tell the user how to correctly run the script
> * `${10}` is mandatory; `$10` without braces is interpreted by Bash as `$1` followed by a literal `0`
> * Use the `shift` command to move arguments to the left (`$2` becomes `$1`) when processing long lists of files.

<br>

## 8. Control Flow: Types of Operators

This is where your script begins to "think."  
Like any other programming langauge like Java or C#, there are operator types and this typesa are used for different conditions, to achieve different goals

>[!WARNING]
> * Bash operators are categorized by the type of `data` they manipulate
> * Using the wrong operator for the data type is the #1 cause of `bugs` in shell scripts.

<br>

**Types of Oeprators in bash scripting**

1. Test & Extended Test
   * `[ ]` Test
   * `[[ ]]` Extended Test
   * `[ ]` is the `POSIX` standard. It is portable but finicky with spaces and wildcards
   * `[[ ]]` is a Bash-specific keyword. It is safer, supports logical operators like `&&` and `||` natively, and handles empty variables better
   * Use `[[ ]]` for Bash scripts as per industry standard

2. **Arithmetic Operators**: Used for math.
   * **Context**: Must be used inside `(( ))` or with the let command
   * **Logic**: Standard math symbols apply (`+`, `-`, `*`, `/`, `%` `**`)

3. **Comparison Operators** (Integer vs. String)
   * **Integer**: Use mnemonic flags inside `[[ ]]`
     - `-eq` (Equal)
     - `-ne` (Not Equal)
     - `-gt` (Greater Than)
     - `-lt` (Less Than)
     - Alternative: Use `(( ))` for C-style math logic. example: `if (( var > 10 )); then`
   * **String**: Use symbols inside `[[ ]]` using `mathematical` symbols
     - `==` (Equal)
     - `!=` (Not Equal)
     - `<` (Greater Than)
     - `>` (Less Than)
     - `-z` (String is empty)
     - `-n` (String is NOT empty)

4. **File Test Operators** : These are unique to shell scripting and allow you to check the filesystem state before executing commands. It is one of the most powerful features for automation
   - `-f` (Check if file exists)
   - `-d` (Check if directory exists)
   - `-x` (Check if executable)

5. **Logical Operators**: Used to combine multiple tests
   * `&&` (AND): Run second command only if the first succeeds
   * `||` (OR): Run second command only if the first fails

<br>

### Industrial Standard Approaches

1. **The "Early Exit" Pattern**: Instead of nesting `if` statements deep, exit early if a condition isn't met. This keeps code readable (flat logic)
2. **Double Quoting**: Always quote variables inside `[ ]` to prevent errors if a variable is null or contains spaces
3. **Logical Combining**: Use `[[ $A == $B && $C == $D ]]` instead of nested if blocks
4. **Math Logic**: For any complex calculation or numeric comparison, use `(( ))`. It is faster and more readable for developers coming from C/Java/Python backgrounds.
5. **Exit Status**: The most important "operator" in a sense is `$?`. In professional scripts, every major command is followed by a check to see if the previous `exit status` was `0`

example of operators in bash script:
```bash
#!/bin/bash

# Industrial Standard: Robust File & Service Check
SERVICE_NAME="$1"

# 1. Validation: Ensure argument is provided
if [[ -z "$SERVICE_NAME" ]]; then
    echo "Error: No service name provided."
    echo "Usage: $0 <service_name>"
    exit 1
fi

# 2. Logic: Check status
if systemctl is-active --quiet "$SERVICE_NAME"; then
    echo "[OK] $SERVICE_NAME is running."
else
    echo "[WARN] $SERVICE_NAME is down. Restarting..."
    sudo systemctl restart "$SERVICE_NAME" || { echo "Failed to restart!"; exit 1; }
fi
```

<br>

Comprehensive Operators Table

| Category          | Operators                       | Syntax                    | Use Case                                   |
| :---              | :---                            | :---                      | :---                                       |            
| Arithmetic        | `+`, `-`, `*`, `/`, `%`         | `(( sum = a + b ))`       | Performing calculations                    |
| Integer           | `-eq`, `-ne`, `-lt`, `-gt`      | `[[ $age -gt 18 ]]`       | Comparing whole numbers                    |
| String            | `==`, `!=`, `<`, `>`            | `[[ $str1 == "admin" ]]`  | Validating text input                      |
| File Test         | `-e`, `-f`, `-d`, `-s`          | `[[ -s $logfile ]]`       | Checking if file exists and is not empty   |
| Logical           | `&&`, `\|\|`, `!`                 | `[[ $a && $b ]]`          | Combining multiple conditions              |
| Redirection       | `>`, `>>`, `2>`, `&>`           | `cmd &> /dev/null`        | Handling output and error logs             |

<br>

Comprehensive Comparition Table

| Comparison Type         | Syntax Example            | Notes & Description                                          |
| :---                    | :---                      | :---                                                         |
| Numeric                 | `[[ $a -eq $b ]]`         | Use `-gt` ( > ), `-lt` ( < ), `-ge` ( >= ), `-le` ( <=)      |
| String                  | `[[ "$a" == "$b" ]]`      | Always quote strings to handle spaces safely                 |
| Logical AND             | `[[ cond1 && cond2 ]]`    | Returns `true` only if all conditions are met                |
| Logical OR              | `[[ cond1 \|\| cond2 ]]`  | Returns `true` if at least one condition is met              |
| Null Check              | `[[ -z "$var" ]]`         | Returns true if the string length is zero (empty variable)   |
| File Exists             | `[[ -f "$file" ]]`        | `-f` regular file, `-d` directory, `-e` any existence        |

<br>

> [!NOTE]
> * Indentation: Use 2 or 4 spaces consistently. Never mix tabs and spaces
> * The `then` placement: The industry standard is to put then on the same line as the if using a semicolon: `if [[ ... ]]; then`
> * Exit Codes: Always provide a non-zero exit code (exit 1) when a script fails a conditional check
>    example:
>   ```bash
>   # 1. Validation: Ensure argument is provided
>   if [[ -z "$SERVICE_NAME" ]]; then
>    echo "Error: No service name provided."
>    echo "Usage: $0 <service_name>"
>    exit 1
>   fi
>   ```

<br>

## 9. Control Flow: Conditionals (if/else)

In industrial automation, the if statement is the "gatekeeper." It ensures that a script doesn't execute dangerous commands (like deleting a database) unless specific safety conditions are met.

### 1. The Simple `if` Statement (The Guard)

This is used for `mandatory checks`. If the condition isn't met, the script simply stops or skips the action.

`Industrial Example`: Root User Check  
Many system-level tasks (like installing packages or managing users) require root privileges.

```bash
#!/bin/bash

# Standard check: UID 0 is always the root user
if [[ $EUID -ne 0 ]]; then
   echo "ERROR: This script must be run as root (sudo)."
   exit 1
fi

echo "Access granted. Proceeding with system update..."
```

<br>

### 2. The `if-else` Statement (The Binary Choice)

This is used when you have two distinct paths: "`Success Path`" and "`Failure Path`"

`Industrial Example`: Connectivity Check  
A DevOps engineer might use this to verify if a remote server or API is reachable before starting a deployment.

```bash
#!/bin/bash

SERVER="google.com"

# -q (quiet) silences output; -c 1 sends one packet
if ping -c 1 "$SERVER" &> /dev/null; then
    echo "[SUCCESS] Network is up. Starting deployment..."
    # deploy_code_function
else
    echo "[FAILURE] Network is down. Aborting and logging error."
    exit 1
fi
```

<br>

### 3. The `if-elif-else` Ladder (The Multi-Decision)

This is used for complex scenarios where there are `more than` two possible states. The script checks conditions from top to bottom and stops at the first one that is true.

`Industrial Example`: Deployment Environment Selector  
In professional software engineering, code is usually deployed to different environments (Dev, Staging, Production) based on an input argument.  

```bash
#!/bin/bash

ENV=$1

if [[ "$ENV" == "prod" ]]; then
    echo "DEPLOING TO: Production (High Priority/Caution)"
    DATABASE_URL="prod-db.example.com"
elif [[ "$ENV" == "staging" ]]; then
    echo "DEPLOYING TO: Staging (Testing Environment)"
    DATABASE_URL="staging-db.example.com"
elif [[ "$ENV" == "dev" ]]; then
    echo "DEPLOYING TO: Development (Local/Sandbox)"
    DATABASE_URL="localhost"
else
    echo "Invalid environment: '$ENV'"
    echo "Usage: $0 {prod|staging|dev}"
    exit 1
fi
```

<br>

**Comparison Table**: Conditional Structures

| Structure     | Logic Type     | Best Use Case                                          |
| :---          | :---           | :---                                                   |
| `if`          | Single Gate    | Security checks (Root user, file existence)            |
| `if-else`     | Binary Choice  | Success/Failure (Ping check, Service status)           |
| `if-elif`     | Multi-Choice   | Environment selection, severity levels (High/Med/Low)  |

<br>

## 10. Control Flow: File Test Operators

In Linux, "`everything is a file`" and Test operators allow you to probe the metadata of a file or directory before performing an action.

1. Existence & Type:
   * `-e`: Checks if the entity exists (could be a file, directory, or socket
   * `-f`: Specifically checks if it is a regular file (not a directory)
   * `-d`: Specifically checks if it is a directory
2. Permissions:
   * `-r`: Checks if the file is readable by the user running the script
   * -`w`: Checks if the file is writable
   * `-x`: Checks if the file is executable
3. Content & Size:
   * `-s`: Checks if the file is not empty (size > 0). This is great for log rotation scripts
   * `-nt`: Checks if one file is newer than another (file1 -nt file2). Essential for build scripts or backups.

<br>

### Industrial Standard Approaches
* **The Sanity Check**: Professionals never assume a file exists. Every script starts by validating its dependencies
* **Temporary Files**: Using -e to check if a lock file exists prevents a script from running twice simultaneously
* **Error Silencing**: Often combined with `/dev/null` to keep the terminal clean during checks

`Example`: Database Backup Prep

```bash
#!/bin/bash
# Industrial Example: Database Backup Prep

BACKUP_DIR="/var/backups/db"
CONFIG_FILE="/etc/db_config.conf"

# 1. Check if config exists AND is readable
if [[ ! -r "$CONFIG_FILE" ]]; then
    echo "Error: Config file missing or not readable."
    exit 1
fi

# 2. Check if backup directory exists; if not, create it
if [[ ! -d "$BACKUP_DIR" ]]; then
    echo "Directory missing. Creating $BACKUP_DIR..."
    mkdir -p "$BACKUP_DIR"
fi

# 3. Check if we have write permissions to the directory
if [[ ! -w "$BACKUP_DIR" ]]; then
    echo "Error: No write access to $BACKUP_DIR."
    exit 1
fi
```
<br>

**File Test Operators Cheat Sheet**

| Operator	   |  Description	      | Industrial Use Case                                    |
| :---         | :---               | :---                                                   |
| `-e`	      | Entity exists	   | General check before any operation.                    |
| `-f`	      | Is a regular file	| Ensure you aren't trying to cat a directory.           |
| `-d`	      | Is a directory	   | Validation before cd or mkdir.                         |
| `-s`	      | File is not empty	| Checking if a log file actually has data to process.   |
| `-r`	      | Is readable	      | Checking if you can read a config file or API key.     |
| `-w`	      | Is writable	      | Checking if a log or temp folder is accessible.        |
| `-x`	      | Is executable	   | Verifying if a binary (like git or curl) is installed. |
| `-L`	      | Is a symbolic link	| Handling shortcuts/symlinks in system paths.           |

<br>

## 11. Control Flow: 



















