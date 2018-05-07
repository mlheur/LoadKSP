<pre>================================================================================
 1. Install PERL for windows; in this example it's installed at
    C:\Perl64\bin\perl.exe
=====
 2. Open command prompt as admin.
===
 2a.  Associate the .sfs extension with KSPSaveFile file type.
      Run the following assoc command.
        C:\> <b>assoc .sfs=KSPSaveFile</b>
        .sfs=KSPSaveFile
===
 2b.  Set up file type KSPSaveFile to open with this perl script.
      Run the following ftype command.  Substitute your location of perl and
      this script as appropriate.  In this example the perl script has been
      saved at C:\Users\Guest\bin\LoadKSP.pl
        C:\> <b>ftype KSPSaveFile=C:\Perl64\bin\perl.exe "C:\Users\Guest\bin\LoadKSP.pl" "%1"</b>
        KSPSaveFile=C:\Perl64\bin\perl.exe "C:\Users\Guest\bin\LoadKSP.pl" "%1"
=====
 3. Double click your desired save file.
================================================================================</pre>
