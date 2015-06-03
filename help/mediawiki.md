Former NESCent PIs will desire access to their NESCent project wikis
so that the wiki may be consulted or continued after NESCent closes.
Following is how this comes about.

### Request the dump

The PI writes to the help desk (help@nescent.org) providing the URL of
the wiki in question, e.g. https://www.nescent.org/wg_mywiki.

### Create the wiki pages XML archive

The help desk creates the XML archive as follows:

    cd /home/www/webapps/wg/mywiki 
    php maintenance/dumpBackup.php --full --conf LocalSettings.php >/tmp/mywiki.xml
    gzip /tmp/mywiki.xml

   (The destination file needn't go in /tmp; you can put it wherever you like.)

'mywiki' is whatever appears in the URL after 'wg_'.

The resulting file will be in /tmp/mywiki.xml.gz.

In the unlikely event it's needed, detailed documentation on the 'dumpBackup.php' script and MediaWiki
backups generally is [here](https://www.mediawiki.org/wiki/Manual:Backing_up_a_wiki).

### Create the uploads directory archive

This second archive is important because it contains all uploaded
files, including data and images.

The help desk creates a tarball of the 'images' directory:

    tar czf /tmp/mywiki.tgz images

### Transmit the archive files to NESCent

The help desk uploads the two archive files, mywiki.xml.gz and
mywiki.tgz, to duke.box.net, in coordination with the NESCent archiving team.

### Obtain the two archive files to NESCent personnel

The NESCent archiving team provides the two archive files to the PI by
enabling download from duke.box.net.

### Select server and install Mediawiki 

Someone who I'll call the 'technician,' either the PI or someone
enlisted to help him/her, finds a suitable server or hosting service.

If Mediawiki and its required support (e.g. MySQL) are not installed, install it.

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
installing Mediawiki 1.19.5, even though it is not the latest version.

### Set up a Mediawiki instance

The technician sets up a Mediawiki instance per the instructions for doing so (e.g. [here](https://www.mediawiki.org/wiki/Manual:Installation_guide)).
The web-based setup dialog prompts for configuration information of
various kinds.

It is probably a good idea to give the wiki the same name ('mywiki' in the example) that it had at NESCent,
if possible.  This name will become the name of the directory
containing all files related to the wiki (I believe the setup dialog asks for the name; not sure about this).

### Unpack the pages archive

Let /path/to/mywiki.xml.gz and /path/to/mywiki.tgz be the locations of
the two archive files on the system where the wiki is to be set up.
These could go anywhere.

First unpack the XML file.  Assuming 'mywiki' is the directory
containing the wiki's 'LocalSettings.php' file, and mywiki.xml.gz is the .xml.gz
file received from NESCent, do the following:

    cd mywiki
    gunzip /path/to/mywiki.xml.gz

You'll now have a big XML file called mywiki.xml.

### Restore the pages from the XML file

Restore the wiki database from file mywiki.xml using the importDump.php command that is part of the Mediawiki software.  See documentation [here](https://www.mediawiki.org/wiki/Manual:Importing_XML_dumps).

    php maintenance/importDump.php --dbpass wikidb_userpassword --quiet wikidb mywiki.xml

### Restore the uploads from images.tgz

Unpack the 'images' directory (that's where all uploads are, not just images) from the wiki directory archive:

    tar xzf /path/to/mywiki.tgz images

### Take it out for a spin

That should do it.
