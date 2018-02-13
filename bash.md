
# BASH SCRIPTING!


### BASIC BASH COMMANDS:

``` bash
.
:
alias
bind
break
builtin
cd
command
continue  
declare
echo	    print
enable
eval	    d
exec
exit
export
getopts
hash
help
let
local
logout
printf
pwd	    present working dir
read
readonly
return
set
shift
shopt
sleep 1		sleep 1 sec before proceeding
test
times
trap
type
typeset
unalias
unlimit
unmask
unset

```

### BASH SCRIPTING:

* execution occurs through forking: the parent process pauses while the child
  process runs through the script
* steps:
    - the shell reads in from a file or from user's terminal
    - input is broken up into words and operators separated by metacharacters
	   * alias expansion takes place
    - the shell parses (analyzes and substitutes) the tokens into simple commands
    - bash performs various expansions, breaking expanded tokens into lists of
      filenames, commands and args
    - redirection performed if necessary
    - commands are executed
    - optionally the shell waits for the command to complete and collects exit
      status.
* a simple shell command is a command followed by a line of args
* a more complex command may consist of:
    - a pipeline in which the output of one becomes the input of the second
    - a loop or conditional construct
* functions are executed in present shell context -- no new process is created
  to interpret them
* permissions: your script must have the correct permissions to run.
    - view perms with
	ls -l script.sh
    - try a line like:
	chmod u+x script.sh
* if you don't want the script to start in a new shell, you source it:
    `source script_name.sh` or `. script_name.sh`
* init scripts starts system services


### VARIABLES:

* global vs local
* types:
    - integer
    - string
    - constant
    - array
* variable names may contain digits but cannot start with one.
* the naming convention is all caps, but do what you want.
* in the shell, variables are set like so:
      VARNAME="value"
* print the variable:
      echo $VARNAME
* using unset will release those value names
      unset VARNAME
* environment variables: vars which have been exported to a subshell.
    - setting and exporting usually happen together:
      export VARNAME="value"
    - if a variable is modified in the subshell, the value is not effected in
      the parent
* 'export' keyword:
    - when a variable is declared, it is put into the environment of the current running script (bash session)
    - when the session closes, the variable is lost
    - unless! you put 'export' in front of the declaration -- this allows the variable to persist across sessions
    - which means: variables declared in your bashrc are available on the command line
* 'source' keyword:
    - if you run a script from the command line, any variable defined in the script are lost when the script concludes
    - if you run `$ source myscript.sh`, variables defined in the script can be referenced after the script executes
    - if you do not want variables to persist after the script, use the `local` keyword when declaring them.

### COMPARISON OPERATORS

* if you declare a variable without assigning it any value (or use an unknown variable name) and then run it through a binary comparison operator, it will yell at you
* this is true of args as well, so it's important to check that you either check that your variable has something before putting it in a binary conditional statement, OR! put your variable reference in double quotes to essentially "cast" it as an empty string if it has no assigned value already.

``` bash
local FOO

#FAILS:
if [ ${FOO} != "" ] ; then
    echo "foo is not equal to empty str"

# RETURNS WHAT YOU'D EXPECT:
if [ "${FOO}" != "" ] ; then
    echo "foo is not equal to empty str"    

```


### PARAMETERS

* parameters that are passed into a bash script are handled in the same way as defined variables -- that is,
  they can be referenced with dollar sign notation: $2 dereferences the 2nd parameter.
* the first parameter (0) is name of the script or program
* parameters containing spaces or special characters should be passed with single or double quotes
* referencing parameters:
  	      0,1,2...		the nth parameter
	      * 		positional parameters starting with 1
	      @ 		positional parameters starting with 1, if your parameters are likely to contain
	      			embedded blanks
	      #			the number of parameters, not including 0
* if you have more than 9, you cannot use $10 -- you'll have to process the first param and drop it 	      



MATHEMATICAL EXPRESSIONS:
* are expressed with double parens:
    $((VAR1 + 3)) will return the sum of $VAR1 and 3
    $((number%3)) will return the modulus of the number divided by 3
* anything other than 0 is evaluated as false.




FORMATTING:
* semicolons are used to separate two commands on the same line


FOR, IF THEN'S:
* the syntax is as follows:
    for VARIABLE in RANGE; do COMMAND done
* this prints out numbers one through one hundred:
    for number in {1..100}; do
	echo $number
    done
* as does this one:
    for number in {1..100}
    do
	echo $number
    done
* for/if then's could look something like:
    for VARIABLE in RANGE; do
	if
	then
	    COMMAND
	else
	    COMMAND
	fi
    done
* or it could be:
    for ; do
	if ; then
	elif ; then
	else
	fi
    done
* or we could have:
    for ; do
	if ; then
	fi
	if ; then
	fi
	if ; then
	else
	fi
    done