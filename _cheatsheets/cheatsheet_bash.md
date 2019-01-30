---
layout: default
title: cheatsheet_bash
---

# test

## string comparation
### Best pratice
```bash
if [ "x$STRING1" = "x$STRING2" ]; then
    echo "the strings are equal"
fi
```

### You should know
```bash

if [ -n "$STRING" ]; then
    echo "the length of \$STRING is nonzero"
fi
if [ -z "$STRING" ]; then
    echo "the length of \$STRING is zero"
fi

if [ "x$STRING1" = "x$STRING2" ]; then
    echo "the strings are equal"
fi

if [ "x$STRING1" != "x$STRING2" ]; then
    echo "the strings are not equal"
fi
```

### Reference
* [string w/o quoting](https://stackoverflow.com/questions/3869072/test-for-non-zero-length-string-in-bash-n-var-or-var) 

## integer comparation
```bash
if [ INTEGER1 -eq INTEGER2 ]; then
    echo "INTEGER1 is equal to INTEGER2"
fi

if [ INTEGER1 -ge INTEGER2 ]; then
    echo "INTEGER1 is greater than or equal to INTEGER2"
fi
if [ INTEGER1 -gt INTEGER2 ]; then
    echo "INTEGER1 is greater than INTEGER2"
fi
if [ INTEGER1 -le INTEGER2 ]; then
    echo "INTEGER1 is less than or equal to INTEGER2"
fi
if [ INTEGER1 -lt INTEGER2 ]; then
    echo "INTEGER1 is less than INTEGER2"
fi
if [ INTEGER1 -ne INTEGER2 ]; then
    echo "INTEGER1 is not equal to INTEGER2"
fi
```
## file testing
```console
       FILE1 -ef FILE2
              FILE1 and FILE2 have the same device and inode numbers

       FILE1 -nt FILE2
              FILE1 is newer (modification date) than FILE2

       FILE1 -ot FILE2
              FILE1 is older than FILE2

       -b FILE
              FILE exists and is block special

       -c FILE
              FILE exists and is character special

       -d FILE
              FILE exists and is a directory

       -e FILE
              FILE exists

       -f FILE
              FILE exists and is a regular file

       -g FILE
              FILE exists and is set-group-ID

       -G FILE
              FILE exists and is owned by the effective group ID

       -h FILE
              FILE exists and is a symbolic link (same as -L)

       -k FILE
              FILE exists and has its sticky bit set

       -L FILE
              FILE exists and is a symbolic link (same as -h)

       -O FILE
              FILE exists and is owned by the effective user ID

       -p FILE
              FILE exists and is a named pipe

       -r FILE
              FILE exists and read permission is granted

       -s FILE
              FILE exists and has a size greater than zero

       -S FILE
              FILE exists and is a socket

       -t FD  file descriptor FD is opened on a terminal

       -u FILE
              FILE exists and its set-user-ID bit is set

       -w FILE
              FILE exists and write permission is granted

       -x FILE
              FILE exists and execute (or search) permission is granted

```


# source vs export
_source_ variable from subprocess

_export_ variable to subprocess
```bash
## subprocess.sh

#!/bin/sh

VAR_SUB="variable_in_subprocess"

echo "    [sub] VAR_TOP is: $VAR_TOP"

```
```bash
## topprocess.sh

#!/bin/sh

# normal:
echo "Normal calling:"
./subprocess.sh
echo "    [top] VAR_SUB is: $VAR_SUB"

# export:
unset VAR_TOP VAR_SUB
echo "Export variable to sub-process:"
export VAR_TOP="variable_in_topprocess"
./subprocess.sh
echo "    [top] VAR_SUB is: $VAR_SUB"

# source:
unset VAR_TOP VAR_SUB
echo "Source variable from sub-process:"
source ./subprocess.sh
echo "    [top] VAR_SUB is: $VAR_SUB"

# export and source:
unset VAR_TOP VAR_SUB
echo "Export and source:"
export VAR_TOP="variable_in_topprocess"
source ./subprocess.sh
echo "    [top] VAR_SUB is: $VAR_SUB"

# mid
echo "3 level calling:"
unset VAR_TOP VAR_SUB
export VAR_TOP="variable_in_topprocess"
source ./midprocess.sh
echo "    [top] VAR_TOP is: $VAR_TOP"
```

```bash
## midprocess.sh

#!/bin/sh

echo "    [mid] VAR_TOP is $VAR_TOP"
VAR_TOP="variable_in_midprocess"
./subprocess.sh
```

```bash
## testing result:

$ ./topprocess.sh
Normal calling:
    [sub] VAR_TOP is:
    [top] VAR_SUB is:
Export variable to sub-process:
    [sub] VAR_TOP is: variable_in_topprocess
    [top] VAR_SUB is:
Source variable from sub-process:
    [sub] VAR_TOP is:
    [top] VAR_SUB is: variable_in_subprocess
Export and source:
    [sub] VAR_TOP is: variable_in_topprocess
    [top] VAR_SUB is: variable_in_subprocess
3 level calling:
    [mid] VAR_TOP is variable_in_topprocess
    [sub] VAR_TOP is: variable_in_midprocess
    [top] VAR_TOP is: variable_in_midprocess
```

## reference
* [A good answer about source and export](https://askubuntu.com/questions/862236/source-vs-export-vs-export-ld-library-path)


# expr
```console
$ expr \( 1 + 2 \) \* 3
9
$ expr 1 + 2  \* 3
7
```

# Redirection
```console
cmd >   output.txt    # stdout to output file
cmd >>  output.txt    # stdout append to output file
cmd 2>  output.txt    # stderr to output file
cmd 2>> output.txt    # stderr append to output file
cmd &>  output.txt    # stdout & stderr to output file
cmd &>> output.txt    # stdout & stderr append to output file

cmd |  tee    output.txt
cmd |  tee -a output.txt
cmd |& tee    output.txt
cmd |& tee -a output.txt
```
# Array
```bash
$ # create an array
$ array1=("value1" "value2" "value3")
$ # append to the array
$ array1=(${array1[@]} "value4")
$ # print elements in array1:
$ echo "elements in array1 are: ${array1[@]}"
elements in array1 are: value1 value2 value3 value4
$ # print the number of elements in the array:
$ echo "number of elements in array1 is ${#array1[@]}"
number of elements in array1 is 4
$ # print length of an element of array1:
$ echo "length of element 0 in array1 is ${#array1[0]}"
length of element 0 in array1 is 6
$ # loop in the array:
$ for e in ${array1[@]}; do
>  echo $e
> done
value1
value2
value3
value4

```
# Argument Parsing
Copied from stackoverflow, see reference.

```bash
## myscript.sh

#!/bin/bash
# saner programming env: these switches turn some bugs into errors
set -o errexit -o pipefail -o noclobber -o nounset

! getopt --test > /dev/null 
if [[ ${PIPESTATUS[0]} -ne 4 ]]; then
    echo 'I’m sorry, `getopt --test` failed in this environment.'
    exit 1
fi

OPTIONS=dfo:v
LONGOPTS=debug,force,output:,verbose

# -use ! and PIPESTATUS to get exit code with errexit set
# -temporarily store output to be able to check for errors
# -activate quoting/enhanced mode (e.g. by writing out “--options”)
# -pass arguments only via   -- "$@"   to separate them correctly
! PARSED=$(getopt --options=$OPTIONS --longoptions=$LONGOPTS --name "$0" -- "$@")
if [[ ${PIPESTATUS[0]} -ne 0 ]]; then
    # e.g. return value is 1
    #  then getopt has complained about wrong arguments to stdout
    exit 2
fi
# read getopt’s output this way to handle the quoting right:
eval set -- "$PARSED"

d=n f=n v=n outFile=-
# now enjoy the options in order and nicely split until we see --
while true; do
    case "$1" in
        -d|--debug)
            d=y
            shift
            ;;
        -f|--force)
            f=y
            shift
            ;;
        -v|--verbose)
            v=y
            shift
            ;;
        -o|--output)
            outFile="$2"
            shift 2
            ;;
        --)
            shift
            break
            ;;
        *)
            echo "Programming error"
            exit 3
            ;;
    esac
done

# handle non-option arguments
if [[ $# -ne 1 ]]; then
    echo "$0: A single input file is required."
    exit 4
fi

echo "verbose: $v, force: $f, debug: $d, in: $1, out: $outFile"
```
```bash
myscript.sh -vfd ./foo/bar/someFile -o /fizz/someOtherFile
myscript.sh -v -f -d -o/fizz/someOtherFile -- ./foo/bar/someFile
myscript.sh --verbose --force --debug ./foo/bar/someFile -o/fizz/someOtherFile
myscript.sh --output=/fizz/someOtherFile ./foo/bar/someFile -vfd
myscript.sh ./foo/bar/someFile -df -v --output /fizz/someOtherFile

# all return

verbose: y, force: y, debug: y, in: ./foo/bar/someFile, out: /fizz/someOtherFile
```
## References:
* [answer on stackoverflow](https://stackoverflow.com/questions/192249/how-do-i-parse-command-line-arguments-in-bash/29754866#29754866)
# References:
* [Bash Reference Manual](https://www.gnu.org/software/bash/manual/bash.html)
* [string w/o quoting](https://stackoverflow.com/questions/3869072/test-for-non-zero-length-string-in-bash-n-var-or-var) 
