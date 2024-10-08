# BatchSequenceFileCreator v1.1
A batch script to create a specified number of files with sequence naming. Created to assist with the creation of BatchFileRenamer (https://github.com/colvdv/BatchFileRenamer).

# Instructions
Put the script into a .bat file, or download the latest release. Open the .bat file to run the script.

## Running The Script
Follow the prompts, reading the directions carefully. If you aren't getting your desired result, look at what the script is doing and adjust your inputs accordingly.

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
