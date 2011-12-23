README UUSSH                                                    Stuart Lynne
                                                Fri Dec 23 00:30:44 PST 2011 


The two programs:

        ruussh
        uussh

Are used to send a directory to another system via ssh, run a command and
return the resulting files.

The original use case was to allow for signing driver files. While the 
Windows DDK tools work reasonably well in Wine under Linux, allowing 
drivers to be easily built, getting them signed with the SignTool utility
was problematical as there are various missing DLL's or unimplemented
entry points.

These programs allowed the driver kit to be assembled in Linux, and then
sent to a local Windows system (with Cygwin openssh installed) where a
short script would run SignTool. The modified files are then sent back.:x


Usage: ruussh inputdir outputdir user@remote command arg1 arg2 .... 

1. Pipes the contents of the specified input directory as a TGZ stream 
2. To an ssh invocation of the uussh command with the command and args.
3. Captures the returned TGZ stream and unpacks in the outputdir.


Usage: (cd inputdir; tar cfz script files... - ) | 
        ssh user@remote uussh command args... | 
        (cd outputdir; tar xfz - )

1. Files received on STDIN as a TGZ stream are unpacked in a temporary directory.
2. The specified command and args are run.
3. The contents of the directory are send to STDOUT as a TGZ stream.
4. The temporary directory is removed.

