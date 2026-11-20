# AWS Challenge Lab: Bash Shell Scripting Exercise

## Overview

This challenge lab demonstrates how to use Bash scripting to create batches of 25 empty files with sequentially increasing names. The script detects the highest existing file number and automatically continues from the next number whenever it is run.

## Objectives

- Create a Bash shell script.
- Use variables, command substitution, conditional statements, and a `for` loop.
- Create 25 empty (0-byte) files per execution using `touch`.
- Detect the highest existing file number and continue the sequence without manually entering each number.
- Verify the generated files using Linux commands.

## Environment

- **Platform:** AWS Academy / Vocareum lab environment
- **Operating system:** Amazon Linux EC2
- **User:** `ec2-user`
- **Shell:** Bash

## Repository contents

```text
.
├── README.md
└── create_files.sh
```

## Bash script

The script uses the prefix `Indrajit`. Change the `NAME` variable if you want to use a different prefix.

```bash
#!/bin/bash

# Set your name as the file prefix
NAME="Indrajit"

# Find the highest existing number
LAST_NUM=$(ls ${NAME}[0-9]* 2>/dev/null | grep -oE '[0-9]+$' | sort -n | tail -1)

# Determine the starting number
if [ -z "$LAST_NUM" ]; then
    START=1
else
    START=$((LAST_NUM + 1))
fi

# Create 25 empty files
for ((i=START; i<START+25; i++))
do
    touch "${NAME}${i}"
done

echo "Successfully created 25 files."
echo "Files created: ${NAME}${START} to ${NAME}$((START+24))"
```

## How it works

1. **Set the prefix:** The `NAME` variable defines the beginning of each filename.
2. **Find the highest number:** The command pipeline lists matching files, extracts the trailing number, sorts numerically, and selects the maximum.
3. **Choose the next number:** If no matching files exist, numbering starts at 1. Otherwise, it starts one higher than the current maximum.
4. **Create a batch:** The `for` loop runs 25 times and `touch` creates each empty file.
5. **Display the range:** The script prints the first and last filenames created in that execution.

## Run the lab

Create and enter a working directory:

```bash
mkdir -p bash-challenge
cd bash-challenge
```

Create the script (or save the supplied `create_files.sh` file in this directory), then make it executable:

```bash
chmod +x create_files.sh
```

Check the script syntax:

```bash
bash -n create_files.sh
```

Run the script:

```bash
./create_files.sh
```

Run it again to create the next batch of 25 files. Each execution continues from the highest existing number.

## Example execution

When run three times in a clean directory, the expected ranges are:

| Execution | Files created |
|---|---|
| 1 | `Indrajit1` – `Indrajit25` |
| 2 | `Indrajit26` – `Indrajit50` |
| 3 | `Indrajit51` – `Indrajit75` |

Example output for the first execution:

```text
Successfully created 25 files.
Files created: Indrajit1 to Indrajit25
```

## Verification

Display a long listing:

```bash
ls -lh
```

Count the generated files:

```bash
find . -maxdepth 1 -type f -name 'Indrajit[0-9]*' | wc -l
```

Count zero-byte generated files:

```bash
find . -maxdepth 1 -type f -name 'Indrajit[0-9]*' -size 0c | wc -l
```

### Verified lab result

The lab run produced **75 generated files**, and the zero-byte verification also returned **75**.

| Check | Result |
|---|---:|
| Generated files | 75 |
| Empty (0-byte) files | 75 |
| Files per execution | 25 |

## Concepts practiced

- Bash variables
- Command substitution
- Linux filename patterns and pipelines
- `grep`, `sort`, and `tail`
- Conditional statements (`if/else`)
- Arithmetic expansion
- `for` loops
- File creation with `touch`
- File listing and validation with `ls` and `find`

## Notes

- Run the script from the directory containing the generated files so it can detect the existing sequence.
- The script assumes the generated files use the selected prefix followed by a number.
- Run only one copy of the script at a time; simultaneous executions could select the same starting number.
- The script creates files in the current working directory.

---

**Lab:** Bash Shell Scripting Exercise  
**Cloud platform:** Amazon Web Services (AWS)
