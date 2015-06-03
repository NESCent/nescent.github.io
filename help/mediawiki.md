Former NESCent PIs will desire access to their NESCent project wikis
so that the wiki may be consulted or continued after NESCent closes.
Following is how this comes about.

## Request the dump

The PI writes to the help desk (help@nescent.org) providing the URL of
the wiki in question, e.g. https://www.nescent.org/wg_mywiki.

## Create the wiki pages XML archive

The help desk creates the XML archive as follows:

    cd /home/www/webapps/wg/mywiki 
    php maintenance/dumpBackup.php --full --conf LocalSettings.php >/tmp/mywiki.xml
    gzip /tmp/mywiki.xml

   (The destination file needn't go in /tmp; you can put it wherever you like.)

'mywiki' is what appears in the URL after 'wg_'.

The resulting file will be in /tmp/mywiki.xml.gz.

In the unlikely event it's needed, detailed documentation on the 'dumpBackup.php' script and MediaWiki
backups generally, you can consult [this
page](https://www.mediawiki.org/wiki/Manual:Backing_up_a_wiki).

## Create the uploads directory archive

This second archive is important because it contains all uploaded
files, including data and images.

The help desk creates a tarball of the 'images' directory:

    cd /home/www/webapps/wg
    tar czf /tmp/mywiki.tgz --exclude="LocalSettings.php*" mywiki/images

## Transmit the archive files to NESCent

The help desk uploads the two archive files, mywiki.xml.gz and
mywiki.tgz, to duke.box.net, in coordination with the NESCent archiving team.

## Obtain the archive files to NESCent personnel

The NESCent archiving team provides the two archive files to the PI by
enabling download from duke.box.net.

## Reconstitute the wiki 

It is possible to reconstitute the wiki in its entirety on a new
server, such as a commercial Mediawiki hosting service.

Beware of the risk of incompatibility between the Mediawiki
version used at NESCent, version 1.19.5, and the one you will get from
a commercial provider or from a fresh installation of mediawiki.
Sometimes there are incompatible changes between Mediawiki versions,
and these could lead to problems with logging in or displaying
content.  There is nothing to do about this other than to read
Mediawiki release notes, which will say when various incompatibilities
were introduced.  Changes prior to 1.19.5 don't matter; changes after
1.19.5 might.

If this turns out to be a serious problem, you should consider
installing Mediawiki 1.19.5.

### Select server and install Mediawiki 

Someone who I'll call the 'technician,' either the PI or someone
enlisted to help him/her, finds a suitable server or hosting service.

If Mediawiki and its required support (e.g. MySQL) are not installed, install it.

### Set up a Mediawiki instance

The technician sets up a Mediawiki instance per the instructions for doing so (e.g. [here](https://www.mediawiki.org/wiki/Manual:Installation_guide)).
The web-based setup dialog prompts for configuration information of
various kinds.

It is probably a good idea to give the wiki the same name (mywiki, see above) that it had at NESCent,
if possible.  This name will become the name of the directory
containing all files related to the wiki (I believe the setup dialog asks for the name; not sure about this).

### Unpack the pages archive

Now install the wiki page content.  First unpack the XML file.  Assuming
'mywiki' is the directory containing file 'LocalSettings.php', and
mywiki.xml.gz is the .xml.gz file received from NESCent,
do the following:

    cd mywiki
    gunzip mywiki.xml.gz

### Restore content from the XML dump

Restore the wiki database from file mywiki.xml using the importDump.php command; see documentation [here](https://www.mediawiki.org/wiki/Manual:Importing_XML_dumps).

    php maintenance/importDump.php --dbpass wikidb_userpassword --quiet wikidb mywiki.xml

Then unpack the images directory (that's where all uploads are, not just images) from the wiki directory archive:

    cd ..
    tar xzf mywiki.tgz mywiki/images

If there is some problem with how the wiki looks or behaves, it may be helpful to consult other files in mywiki.tgz, such as NESCent-LocalSettings.php.

### Using the old version of mediawiki that was used at NESCent

Unpack the wiki files archive:  Let 'mywiki' be the directory where the wiki is to be installed.

    cd mywiki/..
    tar xzf mywiki.tgz

Now copy NESCent-LocalSettings.php to LocalSettings.php, and edit it to set the database user and password and any other configuration required by the local installation.

Now restore the page content, as above:

    cd mywiki
    gunzip mywiki.xml.gz
    php maintenance/importDump.php --dbpass wikidb_userpassword --quiet wikidb mywiki.xml

### Examining files without installing mediawiki

The database dump (mywiki.xml) is a text file and contains the contents of all wiki pages.  If you are in a pickle and can't do a mediawiki install, you may be able to find what you want by examining the file in a text editor.