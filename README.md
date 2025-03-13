# snakegame

Features
 The modes of game:
o Box Level: A level where the snake moves within fixed walls.
o Room Level: A level with obstacles.
 Speed Options:
o Slow
o Medium
o Fast
 Gameplay:
o Snake’s length grows by eating food.
o Tracks the current score and high score.
o Ends when the snake collides with itself or walls.
 Replay Option:
o Players can choose to replay after the game ends.

How to Play
1. Start the Game:
o Press any key to start.
2. Choose Speed:
o 1 for Slow.
o 2 for Medium.
o 3 for Fast.

3. Select Level:
o 1 Box Level.
o 2 Room Level.
4. Game Controls:
o W - Move Up.
o S - Move Down.
o A - Move Left.
o D - Move Right.
5. Objective:
o Eat food (*) to grow longer.
o Avoid collisions with walls or your snake&#39;s body.

Files
 File.asm: Main source code for the game.
 Irvine32.inc: Library for basic I/O and graphical functions (required to run the program).
 Snake Data: Includes messages, walls, rooms, and other game elements.




































SCRIPT



#!/bin/bash

# Function to calculate the sum of digits in a string
sum_of_digits() {
    local num="$1"
    local sum=0
    for (( i=0; i<${#num}; i++ )); do
        sum=$((sum + ${num:i:1}))
    done
    echo "Sum of digits: $sum"
}

# Function to copy specific files
copy_specific_files() {
    read -p "Enter the file name to search for: " filename
    read -p "Enter the target directory to copy the file to: " target_dir
    if [[ ! -d "$target_dir" ]]; then
        echo "Target directory does not exist. Creating it..."
        mkdir -p "$target_dir"
    fi
    
    for file in *; do
        if [[ -f "$file" && ( "$file" =~ ^[A-Z] && "$file" =~ [MADS] || "$file" == *.txt ) ]]; then
            if [[ "$file" == "$filename" ]]; then
                cp "$file" "$target_dir"
                echo "$file copied to $target_dir"
                return
            fi
        fi
    done
    echo "File not found or does not meet criteria."
}

# Function to create a user and set password
create_user() {
    read -p "Enter new username: " username
    sudo useradd "$username"
    echo "User $username created."
    sudo passwd "$username"
}

# Function to find and kill a process
kill_process() {
    read -p "Enter the name of the process to kill: " process_name
    pid=$(pgrep "$process_name")
    if [[ -n "$pid" ]]; then
        echo "Process $process_name found with PID: $pid"
        sudo kill "$pid"
        echo "Process $process_name killed."
    else
        echo "Process $process_name not found."
    fi
}

# Function to copy a directory
copy_directory() {
    read -p "Enter the source directory: " src_dir
    read -p "Enter the destination directory: " dest_dir
    if [[ -d "$src_dir" ]]; then
        cp -r "$src_dir" "$dest_dir"
        echo "Directory $src_dir copied to $dest_dir"
    else
        echo "Source directory does not exist."
    fi
}

# Function to write to and read from a file
write_read_file() {
    read -p "Enter the filename: " filename
    echo "Enter text to write to the file (Press Ctrl+D to stop):"
    while read line; do
        echo "$line" >> "$filename"
    done
    echo "Contents of the file $filename:"
    while read -r line; do
        echo "$line"
    done < "$filename"
}

# Function to replace a word in a file
replace_word_in_file() {
    read -p "Enter the filename: " filename
    read -p "Enter the word to replace: " old_word
    read -p "Enter the new word: " new_word
    if [[ -f "$filename" ]]; then
        sed -i "s/$old_word/$new_word/g" "$filename"
        echo "Replaced all occurrences of '$old_word' with '$new_word' in $filename."
    else
        echo "File does not exist."
    fi
}

# Function to demonstrate find command usage
find_commands_demo() {
    echo "Executing various find commands:"
    
    # Find all files in the current directory
    find . -type f
    
    # Find all directories in the current directory
    find . -type d
    
    # Find files by name
    find . -name "example.txt"
    
    # Find files ignoring case
    find . -iname "example.txt"
    
    # Find files by extension
    find . -name "*.txt"
    
    # Find empty files
    find . -type f -empty
    
    # Find files modified in the last 7 days
    find . -mtime -7
    
    # Find files owned by a specific user
    find . -user $USER
    
    # Find files with specific permissions
    find . -perm 644
    
    # Find and delete .tmp files
    find . -name "*.tmp" -delete
    
    # Find files and execute a command
    find . -name "*.log" -exec rm {} \;
}

# Main script execution
read -p "Enter a number as a string: " input_number
sum_of_digits "$input_number"

copy_specific_files

create_user

kill_process

copy_directory

write_read_file

replace_word_in_file

find_commands_demo






C CODES






#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <ctype.h>

int sum_of_digits(int num) {
    int sum = 0;
    while (num > 0) {
        sum += num % 10;
        num /= 10;
    }
    return sum;
}

int main() {
    int pipefd1[2], pipefd2[2];
    pid_t pid;
    char num_str[20];
    
    if (pipe(pipefd1) == -1 || pipe(pipefd2) == -1) {
        perror("Pipe failed");
        exit(1);
    }

    pid = fork();
    
    if (pid < 0) {
        perror("Fork failed");
        exit(1);
    } else if (pid == 0) {  // Child Process
        close(pipefd1[1]);  // Close write end of first pipe
        close(pipefd2[0]);  // Close read end of second pipe

        read(pipefd1[0], num_str, sizeof(num_str));  
        close(pipefd1[0]);  

        int num = atoi(num_str);  

        write(pipefd2[1], &num, sizeof(num));  
        close(pipefd2[1]);  
        
        exit(0);
    } else {  // Parent Process
        close(pipefd1[0]);  
        close(pipefd2[1]);  

        printf("Enter a number: ");
        scanf("%s", num_str);

        write(pipefd1[1], num_str, strlen(num_str) + 1);  
        close(pipefd1[1]);  

        int received_num;
        read(pipefd2[0], &received_num, sizeof(received_num));  
        close(pipefd2[0]);  

        printf("Received integer from child: %d\n", received_num);
        printf("Sum of digits: %d\n", sum_of_digits(received_num));
    }

    return 0;
}









______________________________________




#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/wait.h>

int main() {
    pid_t pid1, pid2, pid3;

    pid1 = fork();
    if (pid1 == 0) {
        execlp("ls", "ls", "-l", NULL);
        perror("execlp failed");
        exit(1);
    }

    pid2 = fork();
    if (pid2 == 0) {
        execlp("date", "date", NULL);
        perror("execlp failed");
        exit(1);
    }

    pid3 = fork();
    if (pid3 == 0) {
        execlp("whoami", "whoami", NULL);
        perror("execlp failed");
        exit(1);
    }

    // Parent process waits for all children to complete
    wait(NULL);
    wait(NULL);
    wait(NULL);

    return 0;
}





______________________________________________________




#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/wait.h>

void execute_command(char *command, char *arg1, char *arg2) {
    pid_t pid = fork();

    if (pid == 0) {  // Child process
        execlp(command, command, arg1, arg2, NULL);
        perror("execlp failed");
        exit(1);
    } else if (pid > 0) {  // Parent process
        wait(NULL);
    } else {
        perror("Fork failed");
    }
}

int main() {
    printf("Executing different Linux commands:\n");

    execute_command("ls", "-l", NULL);       // List files in long format
    execute_command("pwd", NULL, NULL);      // Print working directory
    execute_command("whoami", NULL, NULL);   // Show current user
    execute_command("date", NULL, NULL);     // Show current date and time
    execute_command("ps", "aux", NULL);      // Show running processes
    execute_command("df", "-h", NULL);       // Show disk usage
    execute_command("free", "-m", NULL);     // Show memory usage
    execute_command("uname", "-a", NULL);    // Show system information
    execute_command("uptime", NULL, NULL);   // Show system uptime
    execute_command("hostname", NULL, NULL); // Show hostname
    execute_command("cal", NULL, NULL);      // Show calendar
    execute_command("id", NULL, NULL);       // Show user ID
    execute_command("who", NULL, NULL);      // Show logged-in users
    execute_command("echo", "Hello, World!", NULL); // Print a message
    execute_command("env", NULL, NULL);      // Show environment variables
    execute_command("lsblk", NULL, NULL);    // Show disk partitions
    execute_command("groups", NULL, NULL);   // Show user groups

    printf("All commands executed.\n");

    return 0;
}

___________________________________________________________________________

MAKEFILE




# Using Makefile, GCC Commands, and Header Files in C

## 1. Steps to Use a Makefile in Linux

A **Makefile** automates the compilation process by defining rules for compiling and linking C files. Follow these steps:

### **Step 1: Create a Makefile**

1. Open a terminal and navigate to the project directory.
2. Create a `Makefile` using a text editor:
   ```bash
   nano Makefile
   ```
3. Define rules inside the Makefile.

### **Step 2: Define Compilation Rules**

Example Makefile:

```make
# Compiler
CC = gcc
# Compiler Flags
CFLAGS = -Wall -Werror -g
# Output Executable
TARGET = my_program
# Object Files
OBJ = main.o utils.o

# Rule to Build the Executable
$(TARGET): $(OBJ)
	$(CC) $(OBJ) -o $(TARGET)

# Rule to Compile Each .c File into .o Object File
%.o: %.c
	$(CC) $(CFLAGS) -c $< -o $@

# Clean Rule to Remove Object Files and Executable
clean:
	rm -f $(OBJ) $(TARGET)
```

### **Step 3: Run the Makefile**

- **To compile and build the program:**
  ```bash
  make
  ```
- **To clean compiled files:**
  ```bash
  make clean
  ```

---

## 2. Compiling and Linking C Files Using GCC Commands

GCC (**GNU Compiler Collection**) is used to compile and link C programs from the command line.

### **Step 1: Compile C Files into Object Files**

```bash
gcc -c main.c utils.c
```

This generates `main.o` and `utils.o`.

### **Step 2: Link Object Files into an Executable**

```bash
gcc main.o utils.o -o my_program
```

Now, `my_program` is the final executable.

### **Step 3: Run the Program**

```bash
./my_program
```

---

## 3. Creating and Using Header Files in C

Header files allow code modularity and reusability.

### **Step 1: Create a Header File**

Create `utils.h`:

```c
#ifndef UTILS_H
#define UTILS_H

// Function prototype
t int add(int a, int b);

#endif // UTILS_H
```

### **Step 2: Create the Corresponding C File**

Create `utils.c`:

```c
#include "utils.h"

int add(int a, int b) {
    return a + b;
}
```

### **Step 3: Include the Header File in the Main Program**

Create `main.c`:

```c
#include <stdio.h>
#include "utils.h"

int main() {
    int result = add(5, 7);
    printf("Sum: %d\n", result);
    return 0;
}
```

### **Step 4: Compile and Link Using CLI**

```bash
gcc -c main.c utils.c
gcc main.o utils.o -o my_program
./my_program
```

Now, the program will print:

```bash
Sum: 12
```

---

## Conclusion

- **Makefile** automates compilation.
- **GCC commands** can compile and link C files manually.
- **Header files** allow modularity and reusability.

This guide ensures efficient compilation and project management in C programming!


_______________________________________________________________________

MAKEFILE 2




**Steps to Create and Use a Makefile**

### **Step 1: Create the Makefile**
1. Open a terminal in your working directory.
2. Create a new file named `Makefile` (without any extension).
   - Use the command:
     ```sh
     touch Makefile
     ```
3. Open the `Makefile` in a text editor:
   - Using `nano`:
     ```sh
     nano Makefile
     ```
   - Or using `vim`:
     ```sh
     vim Makefile
     ```
   - Or using VS Code:
     ```sh
     code Makefile
     ```
4. Copy and paste the following content into the `Makefile`:

   ```make
   all: compile

   compile:
       gcc test.c -o test

   object:
       gcc -c test.c -o test.o

   assembly:
       gcc -S test.c -o test.s

   clean:
       rm -f test test.o test.s
   ```
5. Save the file and exit the editor.

### **Step 2: Running the Makefile**
- **To compile the program** (generate an executable `test`):  
  ```sh
  make compile
  ```
- **To generate only the object file `test.o`**:  
  ```sh
  make object
  ```
- **To generate the assembly file `test.s`**:  
  ```sh
  make assembly
  ```
- **To clean up generated files**:  
  ```sh
  make clean
  ```

---

### **How to Link Object Files into the Main Executable**
When you compile a `.c` file into an object file (`.o`), it contains machine code but isn't yet executable. To generate the final executable, you must link the object file.

#### **Steps to Link the Object File:**
1. First, generate the object file:
   ```sh
   gcc -c test.c -o test.o
   ```
2. Link the object file to create the final executable:
   ```sh
   gcc test.o -o test
   ```
   - This command takes `test.o` and links it into the executable `test`.
3. Run the executable:
   ```sh
   ./test
   ```

---

### **Updating the Makefile for Separate Compilation and Linking**
If you want to keep compilation and linking separate in the Makefile:

```make
all: compile

compile: object
	gcc test.o -o test

object:
	gcc -c test.c -o test.o

assembly:
	gcc -S test.c -o test.s

clean:
	rm -f test test.o test.s
```

Now, running `make compile` will first generate `test.o` and then link it to produce `test`.

This Makefile provides a structured way to compile, generate object files, and link them efficiently.

Let me know if you need further clarification! 🚀

