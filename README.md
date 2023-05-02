Download Link: https://assignmentchef.com/product/solved-comp9044-assignment-2-sheeple-sheeple
<br>



This assignment aims to give you practice in Perl programming generally experience in translating between complex formats with Perl clarify your understanding of shell syntax &amp; semantics

<h1>Introduction</h1>

Your task in this assignment to write a shell compiler. Generally compilers take a high level language as input and output assembler, which can then can be directly executed. Your compiler will take shell scripts as input and output Perl. Such a translation is useful because programmers sometimes convert shell scripts to Perl

Possibly motivations including integration of the code to existing Perl, adding extra functionality more easily available from Perl or a need for data structures unavailable in shell.

Your task in this assignment is to automate this conversion. You must write a Perl program which takes as input a shell script and outputs an equivalent Perl program.

The translation of some shell code to Perl is straightforward. The translation of other shell code is difficult or infeasible. So your program will not be able to translate all shell code to Perl. But a tool that performs only a partial translation of shell to Perl could still be very useful.

You should assume the Perl code output by your program will be subsequently read and modified by humans. In other word you have to output readable Perl code. For example, you should preserve variable names and comments and the code you generate should be sensible formatted.

Your compiler must be written in Perl. You should call your Perl program sheeple.pl. It should operate as Unix filters typically do, read files specified on the command line and if none are specified it should read from standard input (Perl’s &lt;&gt; operator does this for you). For example:

<table width="961">

 <tbody>

  <tr>

   <td width="961">$ <strong>cat gcc.sh</strong> #!/bin/dash for c_file in *.c do     gcc -c $c_file done$ <strong>./sheeple.pl gcc.sh</strong> #!/usr/bin/perl -wforeach $c_file (glob(“*.c”)) {     system “gcc -c $c_file”;}</td>

  </tr>

 </tbody>

</table>

If you look carefully at the example above you will notice the Perl code does not have exactly the same semantics as the shell code if there are no <sub>.c</sub> files in the current directory the for loop in the shell program executes once and tries to compile a non-existent file named *.c whereas the Perl for loop does not execute.

This is a general issue with translating shell to Perl. In many cases the natural translation of the shell code will have slightly different semantics. For some purposes it might be desirable to produce more complex Perl code that matches the semantics exactly, e.g.

<table width="961">

 <tbody>

  <tr>

   <td width="961">#!/usr/bin/perl -w if (glob(“*.c”) {     foreach $c_file (glob(“*.c”)) {        system “gcc -c $c_file”;} } else {     system “gcc -c ‘*.c'”;}</td>

  </tr>

 </tbody>

</table>

This is not desirable for our purposes. Our goal is to produce the clearest most-human-readable code so the (simpler) first translation is more desirable. Subsets

The shell features you need to implement is described in the table below as a series of nested subsets.

It is suggested you tackle the subset in the order listed but this is not required

It is suggested you tackle the subset in the order listed but this is not required.

<table width="961">

 <tbody>

  <tr>

   <td width="80">Subset</td>

   <td width="81">Syntax</td>

   <td width="102">Keywords</td>

   <td width="104">Builtins/Programs</td>

   <td width="100">Variables</td>

   <td width="494">Explanation</td>

  </tr>

  <tr>

   <td width="80"><a href="https://cgi.cse.unsw.edu.au/~cs2041/20T2/activities/sheeple/examples.html#subset0">0</a></td>

   <td width="81">=$#</td>

   <td width="102"> </td>

   <td width="104">echo</td>

   <td width="100"> </td>

   <td width="494">Only simple &amp; obvious statements.</td>

  </tr>

  <tr>

   <td width="80"><a href="https://cgi.cse.unsw.edu.au/~cs2041/20T2/activities/sheeple/examples.html#subset1">1</a></td>

   <td width="81">?* </td>

   <td width="102">fordo done</td>

   <td width="104">exit read cd</td>

   <td width="100"> </td>

   <td width="494">Only simple &amp; obvious statements.No nesting of for loops.? *   for file matching only.</td>

  </tr>

  <tr>

   <td width="80"><a href="https://cgi.cse.unsw.edu.au/~cs2041/20T2/activities/sheeple/examples.html#subset2">2</a></td>

   <td width="81">‘“</td>

   <td width="102">ifthen elseelif fiwhile</td>

   <td width="104">test expr</td>

   <td width="100">$1$2$3 …</td>

   <td width="494">Only simple &amp; obvious statements.No nesting of for/while/if statements.</td>

  </tr>

  <tr>

   <td width="80"><a href="https://cgi.cse.unsw.edu.au/~cs2041/20T2/activities/sheeple/examples.html#subset3">3</a></td>

   <td width="81">`$()$(())</td>

   <td width="102"> </td>

   <td width="104">echo -n</td>

   <td width="100">$#$*<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="7b5f3b">[email protected]</a></td>

   <td width="494">for/while/if statements can be nested.$() as equivalent to back quotes.$(()) for arithmetic.  as equivalent to test</td>

  </tr>

  <tr>

   <td width="80"><a href="https://cgi.cse.unsw.edu.au/~cs2041/20T2/activities/sheeple/examples.html#subset4">4</a></td>

   <td width="81">&lt; &gt;&amp;&amp;||;{}</td>

   <td width="102">case esac local return</td>

   <td width="104">mv chmodlsrm</td>

   <td width="100"> </td>

   <td width="494">) as part of case only.{} for functions only.Simple uses of mv/chmod/ls/rm with no command line options.</td>

  </tr>

 </tbody>

</table>

<h1>Examples</h1>

Some examples of shell code and possible translations are available <a href="https://cgi.cse.unsw.edu.au/~cs2041/20T2/activities/sheeple/examples.html">here</a> and as a <a href="https://cgi.cse.unsw.edu.au/~cs2041/20T2/activities/sheeple/examples.zip">zip</a> <a href="https://cgi.cse.unsw.edu.au/~cs2041/20T2/activities/sheeple/examples.zip">file </a>These examples should provide most the information you need to tackle subsets 0 &amp; 1.

Translating subsets 2-4 will require you to discover information from online or other resources. This is a deliberately part of the assignment.

The Perl you generate can and probably will be different to the examples you’ve been given.

But a good check of the Perl you generate is to execute it and then and then use <sub>diff</sub> to compare its output is identical to the output from the shell script Assumptions/Clarifications

Like all good programmers, you should make as few assumptions about your input as possible.

You can assume the code you are given is Shell that works with the version of on CSE systems (essentially POSIX compatible).

Other shells such as Bash contain many other features. If these features and not present in /bin/dash CSE machines you do not have to handle these

Notice for example the pipe operator (‘|’) does not appear above so you not need to translate pipelines. The right parenthesis (‘)’) is in the subset for use in case statements only.

You should implement the keywords &amp; builtins listed the above table directly in Perl, you cannot execute them indirectly via system or other Perl modules. For example, this is not an acceptable translation.

system “for c_file in *.c; do gcc -c $c_file; done”;

The only shell builtins which you must translate directly into Perl are:

exit read cd test expr echo

The builtins (exit read cd) must be translated into Perl to work. For example this Perl code does not work:

system “exit”;

The last 3 (test expr echo) can be, and often are implemented by stand-alone programs. So, for example, this Perl code will work:

system “echo hello world”;

Doing this will receive no marks instead of using system call you should translate uses of test expr and echo directly to Perl e g :

Doing this will receive no marks, instead of using system.call you should translate uses of test, expr and echo directly to Perl, e.g.:

print “hello world
”;

You do have to handle <em>test</em> operators such <sub>-r</sub> and <sub>-d</sub>. They look like options but are operators in <em>test</em>‘s expression language. There will be little testing of <em>test</em>‘s infrequently used operators.

You don’t have to handle <em>test</em>‘s -ef and -t operators as they don’t have a simple translation to Perl.

The only shell builtin option you need to handle is echo’s -n option.

You do not need to handle other echo options such as <sub>-e</sub>. You do not need to handle options for other builtins. For example you do not need to handle the various (rarely used) <em>read</em> options.

Bash has many special variables. You need only handle a few of these, which indicate the shell script’s arguments. These special variables need be translated:

$# $* <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="a98de9">[email protected]</a> $1 $2 $3 …

You can assume the shell scripts you are given execute correctly with /bin/dash on a CSE lab machine. You program does not have to detect errors in the shell script it is given.

You should assume as little as possible about the formatting of the shell script you are given. However most of the evaluation of your program will be on sensibly formatted shell scripts.

Most of the evaluation of your program will be on shell code that a programmer might normally write to solve a tasks. ocus on translating typical shell scripts, rather than unusual difficult-to-translate Shell.

It is a requirement that any comments in the shell code must be transferred to the Perl.

With some approaches it can be difficult to transfer comments in exactly the same position, in this case it is OK if comments are shifted to some degree.

The Perl generated must be sensible indented and formatted, if input the shell script is sensibly formatted.

If there are shell keywords, e.g. <sub>case</sub>, that you cannot translate the preferred behaviour is to include the untranslated shell construct as a comment. Other sensible behaviour is acceptable.

Your Perl (sheeple.pl) should work with the default Perl on a CSE lab machine. You are permitted to use any Perl modules installed on CSE systems.

The Perl you generate should also work with Perl on a CSE lab machine. The Perl you generate should not use Perl modules.

The Perl you generate should not generate warnings from Perl’s ‘-w’ flags unless the warning corresponds to an issue with the Shell. Hints

You should follow discussion about the assignment in the course forum. Questions about the assignment should be posted there so all students can see answers.

Get the easiest transformations working first, make simplifying assumptions as needed, and get some simple small shell scripts successfully transformed. Then look at handling more constructs and removing the assumptions.

If you want a good mark, you’ll need to be careful in your handling of syntax which has no special meaning in shell but does in Perl.

The bulk of knowledge about shell syntax &amp; semantics you need to know has been covered in lectures. But if you want to get a very high mark, you may need to discover more. Similarly much of the knowledge of Perl you need can be determined by looking at a few of the provided examples but if you want to get a very high mark you will need to discover more. This is deliberate as extracting this type of information from documentation is something you’ll do a lot of when you graduate. Demo Shell Scripts

You should submit five shell scripts named demo00.sh .. demo04.sh which your program translates correctly (or at least well). These should be realistic shell scripts containing features whose successful translation indicates the performance of your assignment. Your demo scripts don’t have to be original, e.g. they might be lecture examples. If they are not original they should be correctly attributed.

If your have implemented most of the subsets these should be longer shell scripts (20+ lines). They should if possible test many aspects of shell to Perl translation. Test Shell Scripts

You should submit five shell scripts named test00.sh .. test04.sh which each test a single aspect of translation. They should be short scripts containing shell code which is likely to be mis-translated. The test??.sh scripts do not have to be examples that your program translates successfully.

You may share your test examples with your friends but the ones you submit must be your own creation.

The test scripts should show how you’ve thought about testing carefully. They should be as short as possible (even just a single line).

<h1>Diary</h1>

You must keep notes on each piece of work you make on this assignment. The notes should include date, starting and finishing time, and a brief description of the work carried out. For example:

<table width="493">

 <tbody>

  <tr>

   <td width="87">Date</td>

   <td width="65">Start</td>

   <td width="62">Stop</td>

   <td width="99">Activity</td>

   <td width="180">Comments</td>

  </tr>

  <tr>

   <td width="87">29/09/15</td>

   <td width="65">16 00</td>

   <td width="62">17 30</td>

   <td width="99">coding</td>

   <td width="180">implemented exit</td>

  </tr>

  <tr>

   <td width="87">30/09/15</td>

   <td width="65">20 00</td>

   <td width="62">10 30</td>

   <td width="99">debugging</td>

   <td width="180">found bug in for loops</td>

  </tr>

 </tbody>

</table>

Include these notes in the files you submit as an ASCII file named diary.txt. Change Log

Version 0.1                                                     Initial release

(2020-07-22 17 00)

When you think your program is working, you can use autotest to run some simple automated tests:

$ <strong>2041 autotest sheeple</strong>


