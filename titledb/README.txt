####
# README for use on install-titledb 
###
Requirements - 
    Maven installed on your machine and in your path
    access to the web as wel
    Java installed on your machine and either in your path or with  JAVA_HOME set

The install-titledb script is used to take one or more human readable *.tdb files in TDB1.0 syntax and generates a 
machine readable XML file or files for use by a LOCKSS2.0 daemon to identify information about archival units that are available
to a particular LOCKSS network.  The script uses the java class files found in lockss-tdb-processor which is installed
or updated using Maven.  A template for a configuration file is available (lockss.txt) and can be customized to the system and
network settings.

In its basic usage, the script will loop over all files with the suffix ".tdb" from the designated TDBDIR and use them to generate an
XML file OFILE which must end in ".xml" and which is placed in the output directory ODIR. The output file will be modified to the 
designated group permission (WEBGRP) and optional SELinux security context before being moved. Only those archival unit
declarations that meet the release parameters TDBXMLOPTS are included in the output file.  The script will also process any TDB files found
in first-level subdirectories below TDBDIR and place the results in analogously named output XML files.

The script expects to get configuration settings from a file defined by a network name which is the only necessary run-time argument.
The configuration file should be the name of the network with a ".txt" suffix.  

Example 1:
    install-titledb --help 
to see run-time arguments with a brief explanation of defaults

Example 2: 
   install-titledb lockss
pulls necessary configuration settings lockss.txt and uses the arguments defined there or the defaults

Example 3:
   install-titledb -outdir ./test-output -tdbdir ./test-tdb lockss
pulls necessary configuration settings from lockss.txt and then processes the tdb files found in "./test-tdb" and places the resulting titledb.xml 
file in "./test-output"

Example 4:
   install-titled -dry-run lockss --testing
pulls necessary configuration settings from lockss.txt and then processes the tdb files from the designated TDBDIR (or default) and leave the
resulting output files in temporary dictory and display its location.  Override the default TDB parameters (--production) and use the given 
--testing parameter to determine which AUs to process.

The configuration file must be present to set the no-default-value settings for WEBGRP and the current TDBXMLVERSION (currently 1.2.0).
The configuration can be used to establish a non-default MAVEN repository location, otherwise the "~/.m2/repository" will be assume by Maven.
Additionally, the configuration can optionally set values that could also be set on the command line - TDBDIR (location of the tdb files to process), 
ODIR (the output directory), the name of the output file (OFILE), and the type of AUs to include (TDBXMLOPTS).  Command line arguments take precedence
over configuration file settings. Defaults are only applied if values aren't given.
DEFAULT VALUES:
  NETWORK - given as an argument to install-titledb - provides the name of the configuration file and some default values
  WEBGRP - no default, must be set in the configuration file
  TDBXMLVERSION - no default, must be set in the configuration file
  MAVENDIR - Maven default is ~/.m2/repository
  TDBDIR - ./tdb/$NETWORK
  ODIR - .
  OFILE - titledb.xml (and titledb-<subdir>.xml for any optional subdirectory tdb files)
  TDBXMLOPTS - "--production" 



Information about TDBXMLOPTS:
   The arguments indicate which AUs should be made available to the LOCKSS technology. The default is AUs that have been marked as ready for "production".
   Another options that might make sense  "--testing --production"  (for a network established to test content prior to release to the full network.)

For more information about TDB files and their status, refer to the documentation on TDB1.0 Syntax and Creating TDB Files for your network.    



