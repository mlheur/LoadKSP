#!/usr/bin/perl -w
################################################################################
# 1. Install PERL for windows; in this example it's installed at
#    C:\Perl64\bin\perl.exe
#####
# 2. Open command prompt as admin.
###
# 2a.  Associate the .sfs extension with KSPSaveFile file type.
#      Run the following assoc command.
#        C:\> assoc .sfs=KSPSaveFile
#        .sfs=KSPSaveFile
###
# 2b.  Set up file type KSPSaveFile to open with this perl script.
#      Run the following ftype command.  Substitute your location of perl and
#      this script as appropriate.  In this example the perl script has been
#      saved at C:\Users\Guest\bin\LoadKSP.pl
#        C:\> ftype KSPSaveFile=C:\Perl64\bin\perl.exe "C:\Users\Guest\bin\LoadKSP.pl" "%1"
#        KSPSaveFile=C:\Perl64\bin\perl.exe "C:\Users\Guest\bin\LoadKSP.pl" "%1"
#####
# 3. Double click your desired save file.
################################################################################

$|=1;

# TODO: check if assoc and ftype are set up yet
# TODO: then check if running as admin, if so set them up, else print instructions

use strict;
use File::Basename;
use File::Spec;

# TODO: give platform specific error messages.
die "Usage: $0 </path/to/ksp.sfs>" unless defined $ARGV[0];
die "Usage: $0 </path/to/ksp.sfs>" if defined $ARGV[1];


# TODO: Error handling needed.
# Currently assuming fully qualified path is provided each time, that's how windows explorer does it.
my %SaveData = ();
$SaveData{FQN} = $ARGV[0]; # Fully Qualified Name
$SaveData{BN} = basename( $SaveData{FQN}, ".sfs" ); #Base Name
$SaveData{Saves} = dirname( dirname( $SaveData{FQN} ) );
$SaveData{Profile} = basename( dirname( $SaveData{FQN} ) );
$SaveData{ALG} = File::Spec->catfile( $SaveData{Saves}, "AutoLoadGame.conf" );
$SaveData{GameDir} = dirname( $SaveData{Saves} );
# TODO: get platform specifc executable.
@{$SaveData{EXE}} = ( File::Spec->catfile( $SaveData{GameDir}, "KSP_x64.exe" ), "" );

# TODO: lots of validation that this is a genuine save path that will be used by the game.

# Write the AutoLoadGame.conf settings for this save
open( ALG, ">", $SaveData{ALG} ) or die "Unable to write AutoLoadGame.conf at [$SaveData{ALG}]";
printf ALG ( "game = %s\n", $SaveData{Profile} );
printf ALG ( "save = %s\n", $SaveData{BN} );
close( ALG );

die "KSP [$SaveData{EXE}[0]] not executable" unless -x $SaveData{EXE}[0];

# without chdir, certain log files will get created in the saves dir (or wherever CLI was issued from);
chdir( $SaveData{GameDir} ) or die "Unable to change to game dir [$SaveData{GameDir}]";
# start this copy of KSP
exec( @{$SaveData{EXE}} ) or die "Unable to execute KSP from [$SaveData{EXE}[0]]";
