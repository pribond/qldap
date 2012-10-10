qldap
=====

LDAP query program (standalone or to use with mutt)


What is it?
-----------

Qldap is a program to query an LDAP directory. It is typically used
to query people phone numbers or mail addresses.

Configuration for LDAP directory is specified in your $HOME/.qldaprc
file. You may specify multiple output formats.

Standalone use
--------------

Example:

	$ qldap david
	... (many lines)
	DAVID Pierre (MAI)                 pda@unistra.fr                 MAI 0368854650
	... (many lines)

You can also use the specialized "phone" format if you have defined
it in your $HOME/.qldaprc configuration file:

Example:

	$ qldap -m phone david
	... (many lines)
	0368854650 DAVID Pierre (MAI)
	... (many lines)

Use with mutt
-------------

The [mutt mailer](http://www.mutt.org) has the ability to use an
external query program. To use qldap with mutt, just put in your
mutt configuration file:

	set query_command = "qldap -m mutt '%s'"

This line tells mutt to call qldap when:
* you press the [Ctrl][T] key when composing the mail recipient address 
* you press the [Q] key

Please note that this command uses the "mutt" mode, which must be
specified in your $HOME/.qldaprc configuration file.

Special notice:
* mutt use tabs to separate columns. So, be careful to use a
    real tab (not a \t or multiple spaces) in "fmt" line in
    your configuration file
* mutt drops the first line. So, you need to force qldap to
    output a first, unused line (verbose.mutt yes)

Configuration file
------------------

Each configuration file line in your $HOME/.qldaprc has the syntax:

	key[.mode]	value

Valid keys are:

<table>
   <tr><td> url</td>
	<td> URL of your LDAP directory. This URL must be ldap:// or ldaps://</td></tr>
   <tr><td> base</td>
	<td> base DN of your LDAP directory</td></tr>
   <tr><td> binddn (optional)</td>
	<td> needed if your LDAP directory needs authentication</td></tr>
   <tr><td> passwd (optional)</td>
	<td> needed if your LDAP directory needs authentication</td></tr>
   <tr><td> filter</td>
	<td> LDAP search filter</td></tr>
   <tr><td> fmt</td>
	<td> format of output lines</td></tr>
   <tr><td> sort</td>
	<td> sort criterion</td></tr>
   <tr><td> verbose</td>
	<td> specify "yes" qldap must output a first line giving output details</td></tr>

You can specify different formats for output lines or sort criteria.
These formats are used with qldap "-m" option. For example:

	# default mode
	fmt             %cn$-34.34s %mail$
	sort		cn
	# "phone" mode
	fmt.phone	%telephoneNumber$-10.10s %cn$s
	sort.phone	telephoneNumber

The "fmt" and "sort" lines specify a default mode, and the "fmt.phone"
and "sort.phone" specify a new "phone" mode.

License
-------

BSD
