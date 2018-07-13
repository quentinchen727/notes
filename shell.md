## Zshell
Zshell is more rigid.
A conditional expression is used with the [[ compund command to test atributes of `files` and to compare `strings`: if [[ -f foo && -f too]]
If wanting to run two commands: if command1 && command2;then blabla;fi

# Paremeter substitution
${parameter-default}  # if the parameter not set, use default
${parameter:-default} # if not set and set but null, use default

# Piping
|& # piping error information into next command
