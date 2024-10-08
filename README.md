# BatchSequenceFileCreator v1.1
A batch script to create a specified number of files with sequencial naming.

# Instructions
Put the script into a .bat file, or download the latest [release](https://github.com/colvdv/BatchSequenceFileCreator/releases). Open the .bat file to run the script.

## Running The Script
Follow the prompts, reading the directions carefully. If you aren't getting your desired result, look at what the script is doing and adjust your inputs accordingly.

## How It Works
This script is a batch file designed to create a specified number of files with custom filenames based on user input. Here's a breakdown of what it does:

1. Setup:
   - `@echo off`: Prevents commands from being shown in the command prompt.
   - `setlocal enabledelayedexpansion`: Enables the use of delayed variable expansion, allowing the script to modify and use variables within loops.

2. Getting User Input:
   - The script prompts the user for several pieces of information:
     - Number of files to create.
     - Part 1 of the filename (static text).
     - Part 2, which includes a static part and a placeholder for a sequential number (e.g., "S01E").
     - Part 3 of the filename (static text).
     - File extension (without the dot).

3. Generating Filenames:
   - The script constructs filenames in a loop based on the user inputs.
   - It formats the sequential part with leading zeros (e.g., "01", "02", etc.).
   - Each filename is constructed in the format: `Part1 + Part2 + FormattedSequentialNumber + Part3 + .fileExt`.

4. Display Filenames:
   - The script echoes the full paths of the files that would be created.

5. Confirmation:
   - It prompts the user to confirm whether they want to create the files.

6. File Creation:
   - If the user confirms (by entering "Y"), it creates empty files at the specified paths.
   - If the user declines, it cancels the file creation process.

7. Exit:
   - Finally, it waits for the user to press any key before exiting.

### Example:
If the user inputs:
- Number of files: "3"
- Part 1: "This.Is.Part.1."
- Part 2: "S01E"
- Sequential Part Input: "the 'E' in 'S01E' as 'E'"
- Part 3: ".This.Is.Part-3"
- File extension: "txt"

The script would create the following files:
- `This.Is.Part.1.S01E01.This.Is.Part-3.txt`
- `This.Is.Part.1.S01E02.This.Is.Part-3.txt`
- `This.Is.Part.1.S01E03.This.Is.Part-3.txt`

These files would be created in the directory where the script is located.

## The Script
```
@echo off
setlocal enabledelayedexpansion

:: Get the directory of the script
set "scriptDir=%~dp0"

:: Prompt user for number of files
set /p numFiles="Enter the number of files to create: "

:: Prompt user for part 1
set /p part1="Enter the first part of the filename (e.g., This.Is.Part.1.): "

:: Prompt user for part 2
set /p part2="Enter the second part of the filename (e.g., S01) followed by a placeholder for the sequential part (e.g., E) (Example: Enter 'S01E' if desired sequence output is 'S01E01', 'S01E02', 'S01E03', and so on.): "

:: Prompt user for which part of part 2 is sequential
set /p seqPart="Which part should be sequential (e.g., the 'E' in 'S01E' as 'E') (follow this same format): "

:: Prompt user for part 3
set /p part3="Enter the third part of the filename (e.g., .This.Is.Part-3): "

:: Prompt user for the file desired extension
set /p fileExt="Enter the desired file extension (without the dot, e.g., txt if you want .txt files): "

:: Prepare to show the filenames
echo.
echo The following files will be created:
echo.

:: Create an array-like structure to hold the filenames
set "files="

:: Loop to generate the filenames
for /L %%i in (1,1,%numFiles%) do (
    set "formattedSeq=0%%i"  :: Create the sequential part with leading zero
    if %%i geq 10 (
        set "formattedSeq=%%i"
    )

    :: Construct the filename using the static part and the formatted sequence
    set "filename=!part1!!part2!!formattedSeq!!part3!.%fileExt%"
    set "fullpath=!scriptDir!!filename!"
    
    echo !fullpath!
    
    :: Store the fullpath in the variable for later use
    set "files[%%i]=!fullpath!"
)

echo.
set /p confirm="Do you want to proceed with creating these files? (Y/N): "

if /I "!confirm!"=="Y" (
    for /L %%i in (1,1,%numFiles%) do (
        set "currentFile=!files[%%i]!"
        echo Creating file: !currentFile!
        echo. > "!currentFile!"
    )
    echo Done.
) else (
    echo File creation canceled.
)

echo Press any key to exit..
pause > nul
endlocal
```
