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


