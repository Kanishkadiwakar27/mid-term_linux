                               MID TERM PROJECT

Name - Kanishka Diwakar
Course - B.Tech(CSE)
Sap id - 590029336
Batch - 78


1. Introduction

The Daily User Log Archiver is a shell scripting project designed to automate the process of recording, organizing, and managing system logs on a Linux system.
The project utilizes various Linux commands to collect essential system information, store it in structured log files, and automatically archive older logs.
It demonstrates fundamental Bash scripting concepts such as file manipulation, loops, conditions, and scheduling using cron.


2. Objectives

The main objectives of the project are:

To identify the current system user and log their activity.

To collect and store system information such as uptime, running processes, and disk usage.

To automatically manage and archive logs older than seven days.

To automate the script execution using a cron job that runs daily.

To demonstrate the use of shell commands, conditions, and automation in Linux.


3. Tools and Technologies Used

Tool / Command	Purpose

Whoami -	Identifies the current user.
Date -	Retrieves the current date and time.
Uptime -	Displays system uptime and load average.
Ps -	Lists running processes with CPU and memory usage.
Df -	Shows disk usage details.
mkdir, find, tar -	Used for file creation, searching, and archiving.
Crontab -	Schedules the script to run automatically every day.
Bash -	Shell interpreter used to execute the script.



4. Implementation

*The script begins by defining directory paths for storing logs and archives.
*It then checks if the directories exist and creates them if necessary.
*A new log file is generated daily with the current date as part of the filename (e.g., log_2025-10-19.txt).
*The script writes the following information into each log file:
Current date and user name

System uptime

Top five CPU-consuming processes

Disk usage summary

Logs older than seven days are automatically compressed into a .tar.gz archive using the tar command and stored in an archive subdirectory.
A find command is used to identify old log files based on modification time (-mtime +7).
Finally, a cron job is added to execute the script daily at 8:00 PM.

5. Results

Upon successful execution, the script creates a new log file each day in the ~/daily_logs directory.
Logs older than seven days are moved to the archive folder and compressed.
The output of each command (user, uptime, processes, disk usage) can be verified in the log file.
The cron job ensures the script runs daily, demonstrating automation and file management concepts effectively.



6. Conclusion

The Daily User Log Archiver successfully automates daily system monitoring and log management using Bash scripting.
It provides a simple yet effective solution for maintaining system logs without manual effort.
The project demonstrates practical knowledge of Linux commands, file handling, loops, conditions, and cron scheduling — fulfilling all midterm project requirements.

Script code_
#!/bin/bash
# =====================================================
# Daily User Log Archiver
# Author: Kanishka Diwakar
# Description:
#   This script logs system information daily, 
#   rotates logs older than 7 days, 
#   archives weekly logs, 
#   and can be scheduled with cron.
# =====================================================

# --- 1. Set up directories ---
LOG_DIR=~/daily_logs
ARCHIVE_DIR=$LOG_DIR/archive
mkdir -p "$LOG_DIR" "$ARCHIVE_DIR"

# --- 2. Log file name with date ---
LOGFILE="$LOG_DIR/log_$(date +%Y-%m-%d).txt"

# --- 3. Record basic system info ---
echo "===================================" >> "$LOGFILE"
echo "Date: $(date)" >> "$LOGFILE"
echo "User: $(whoami)" >> "$LOGFILE"
echo "Uptime: $(uptime)" >> "$LOGFILE"
echo "-----------------------------------" >> "$LOGFILE"

# --- 4. Record top 5 CPU processes ---
echo "Top 5 Processes (by CPU usage):" >> "$LOGFILE"
ps -eo pid,comm,%mem,%cpu --sort=-%cpu | head -n 6 >> "$LOGFILE"
echo "-----------------------------------" >> "$LOGFILE"

# --- 5. Record disk usage ---
echo "Disk Usage:" >> "$LOGFILE"
df -h >> "$LOGFILE"
echo "===================================" >> "$LOGFILE"
echo "Log saved to: $LOGFILE"
echo ""

# --- 6. Move old logs to archive (older than 7 days) ---
echo "Archiving logs older than 7 days..."
find "$LOG_DIR" -name "log_*.txt" -mtime +7 -exec mv {} "$ARCHIVE_DIR" \;
echo "Old logs moved to $ARCHIVE_DIR"
echo ""

# --- 7. Create weekly archive (Sunday) ---
DAY_OF_WEEK=$(date +%u)  # Sunday = 7
if [ "$DAY_OF_WEEK" -eq 7 ]; then
    TARFILE="$ARCHIVE_DIR/weeklylogs_$(date +%Y-%m-%d).tar.gz"
    tar -czf "$TARFILE" -C "$ARCHIVE_DIR" .
    echo "Weekly logs archived as: $TARFILE"
    echo ""
fi

# --- 8. Menu for manual actions (optional) ---
while true; do
    echo "========== Daily Log Menu =========="
    echo "1. View latest log"
    echo "2. Archive old logs now"
    echo "3. Clean all old logs"
    echo "4. Exit"
    read -p "Choose an option [1-4]: " choice

    case $choice in
        1)
          echo "----- Latest Log -----"
            cat "$LOGFILE"
            ;;
        2)
            echo "Manually archiving logs..."
            tar -czf "$ARCHIVE_DIR/manual_archive_$(date +%Y-%m-%d).tar.gz" "$LOG_DIR"/log_*.txt
            echo "Manual archive created!"
            ;;
        3)
            echo "Cleaning old logs..."
            find "$ARCHIVE_DIR" -type f -mtime +30 -delete
            echo "Old archives deleted!"
            ;;
        4)
            echo "Exiting..."
            break
            ;;
        *)
            echo "Invalid option, try again."
            ;;
    esac
done

exit 0

![alt text](script1-2.png)
![alt text](script2.png)

Script Execution_
![alt text](<script execution.png>)

Generated  log file_ 
![alt text](<generated logfile1.png>)
![alt text](<generated logfile2.png>)

Archive Directory_ 
![alt text](<archive directory.png>)


Crontab_
![alt text](<crontab setup.png>)








