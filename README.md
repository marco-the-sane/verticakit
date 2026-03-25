# Readme for verticakit

0. Preparation tasks to be executed with sudo privileges - as root:
    * yum / apt install unixODBC-devel : install the ODBC stack provided by unixODBC.org
    * run `touch /etc/vertica.ini` to create an empty Vertica ODBC client config file.
    * Use `chown` to make the owner of the Vertica stack - usually dbadmin of all `*.ini` files in the `/etc/` directory.

1. Place the verticakit.tar.gz tarball into the home directory of the user working with the vertica stack - often dbadmin.
   
2. Unpack the tarball in dbadmin's home directory with "tar -xvzf verticakit.tar.gz"
   in "~/.odbc_profile", edit the section for vsql: 
    * change the VSQL_HOST variable to the name or IP address of your Vertica node
    * change the VSQL_DATABASE variable to the name of your database
    * change the VSQL_PASSWORD variable to your dbadmin password
    normally, these settings can be used by the odb and dbman sections of this profile file
4. source the freshly edited file, with ". .odbc_profile"
5. open `/etc/odbc.ini` for editing
6. In that odbc.ini file, 
   * in section [ODBC Data Sources], change the name of the data source to "<your db name> = Vertica database"
   * change the section [vmartdb]: 
     > change [vmartdb] to [<your db name>], 
     > and in the section, change  "Database = vmartdb" to "Database = <your db name>"
     > and also change "Servername =  ... " to "Servername = <name or IP address of your Vertica node> "
7. Save and quit odbc.ini
8. go to dbadmin's home directory, and add a last line to ".bash_profile" or ".profile", whichever applies, to contain:
   . $HOME/.odbc_profile
   This ensures you have your ODBC settings loaded every time you log in as dbadmin
Contents of the bin directory - a survival kit:
* d2l               C executable; generates CREATE TABLE from a parsed CSV file
* dbman             Marco's ODBC based SQL client; powerful output formatter and code generator; and yes, SQL command interpreter
* ep.pl             parameter marker expander. Takes a query template with ${param} markers; expands them using a parameter file
* fmtmsecs          formats milliseconds to D HH:MI:SS.FFFFF format
* fmtsecs           formats seconds to D HH:MI:SS.FFFFF format
* odb               Maurizio's ODBC based client; multi-threaded query driver and data mover; and yes, SQL command interpreter
