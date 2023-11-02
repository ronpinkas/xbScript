# xbase-script

xBaseScript is an xBase language (dBase/FoxPro/Clipper/[x]Harbour Interpreter, 
implemenmted in the Clipper/Harbour/xHarbour dialects - including a complete
Clipper Pre-Processor (supporting the [x]Harbour extensions.

The implementation is mostly contained in a single PRG source file (pp.prg - 
as it started as a Pre-Processor implementation and later grew to a full interpreter
and a complete DOT Prompt/Run-Time Environment).

You can build it using Clipper/Harbour/xHarbour and probably using Xbase++ and 
other Clipper clones/supersets. 

Command line switches and syntax.


   PP filename[.ext] [-CCH] [-D<id>] [-D:E] [-D:M] [-D:P] [-H] [--help][-I<path>]
                     [-P] [-R] [-STRICT] [-U[<ch-file>]]

    -CCH     = Generate a .cch file (compiled command header).
    -D<id>   = #define <id>.
    -D:E     = Show tracing information into the Expression Scanner.
    -D:M     = Show tracing information into the Match Engine.
    -D:P     = Show tracing information into the Output Generator.
    -H       = Syntax and command line switches description.
    --help   = Syntax and command line switches description.
    -I<path> = #include file search path(s) (';' seperated).
    -P       = Generate .pp$ pre-processed output file.
    -R       = Run filename as a script.
    -STRICT  = Strict Clipper compatability (clone Clipper PreProcessor bugs).
    -U       = Use command definitions set in <ch-file> (or none).

PP has 3 personalities which are tied tightly together.

1. What is supposed to be 100% Clipper compatible Pre-Processor
   (with some extensions).

   Executing PP followed by a source file name  and the -P switch, will
   create <filename.pp$> which is the equivalent of the Clipper
   <filename.ppo> file.

   This syntax is:

     PP filename[.ext] -P

   In this mode these are the optional command line switches.

    -CCH     = Generate a .cch file (compiled command header).
    -D<id>   = #define <id>.
    -D:E     = Show tracing information into the Expression Scanner.
    -D:M     = Show tracing information into the Match Engine.
    -D:P     = Show tracing information into the Output Generator.
    -I<path> = #include file search path(s) (';' separated).
    -STRICT  = Strict Clipper compatability (clone Clipper PreProcessor bugs).
    -U       = Use command definitions set in <ch-file> (or none).

2. DOT prompt, which allows most of the Clipper syntax. Please
   report any syntax you expect to work, but is not supported.

   It does support IF [ELSE] [ELSEIF] ENDIF in DOT environment.

   Executing PP with no source filename will start the DOT prompt mode.

   In this mode you can execute a single line at a time by typing the line
   and pressing the [Enter] key.

   Additionally you may type:

     DO filename.prg [Enter]

   So that DOT will "run" the specified source file. This interpreter
   mode is subject to few limitations:

     a. It does support LOCAL/STATIC/PRIVATE/PUBLIC, but:

       - STATICs are actually implemented as publics.

       - LOCALS have scoping of locals but are implemented as privates
         so you can't have a LOCAL and a PRIVATE with the same name.

     b. Non-declared variables are auto-created on assignment in Harbour
        but NOT in Clipper (yet).

     c. It does support definition and execution of prg-defined
        FUNCTIONs/PROCEDUREs.

     d. It does support ALL control flow structures *except* BEGIN
        SEQUENCE [BREAK] [RECOVER] END SEQUENCE.

     e. The executed module is compiled with -n option (for now).

  This will create rp_dot.pp$ compilation trace file.

3. Finally, PP is a limited Clipper/Harbour/xBase Interpreter. Subject
   to those same few limitations it can execute most of Harbour syntax.
   Executing PP followed by a source file name and the -R switch will
   "RUN" that source (it will also create the rp_run.pp$ compilation
   trace file).

   This syntax is:

     PP filename[.ext] -R

   In this mode these are the optional command line switches.

    -CCH     = Generate a .cch file (compiled command header).
    -D<id>   = #define <id>.
    -D:E     = Show tracing information into the Expression Scanner.
    -D:M     = Show tracing information into the Match Engine.
    -D:P     = Show tracing information into the Output Generator.
    -I<path> = #include file search path(s) (';' separated).
    -P       = Generate .pp$ pre-processed output file.
    -STRICT  = Strict Clipper compatability (clone Clipper PreProcessor bugs).
    -U       = Use command definitions set in <ch-file> (or none).

     a. It does support LOCAL/STATIC/PRIVATE/PUBLIC, but:

       - STATICs are actually implemented as publics.

       - LOCALS have scoping of locals but are implemented as privates
         so you can't have a LOCAL and a PRIVATE with the same name.

     b. Non-declared variables are auto-created on assignment in Harbour
        but NOT in Clipper (yet).

     c. It does support definition and execution of prg-defined
        FUNCTIONs/PROCEDUREs as well as parameter passing and return values.

     d. It does support ALL control flow structures *except* BEGIN
        SEQUENCE [BREAK] [RECOVER] END SEQUENCE.

     e. The compiled module is automatically using -n (No implicit startup
        procedure) if the script starts with a Procedure/Function definition.

     f. Built-in OLE COM Client gateway is included when PP is compiled with
        Harbour and using -dWIN (harbour pp -dWIN -w ... )
