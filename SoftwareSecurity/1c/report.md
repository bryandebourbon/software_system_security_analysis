[back to Manifest](/a2/README.md)

#  Software Running on Server
1. [PostgreSQL](#postgresql)
2. [PHP](#php)
3. [Apache](#apache)
4. [Ubuntu](#ubuntu)
 
# FIX LINKSSSS!!!!!!

## PostgreSQL 


[back to software](#software-running-on-server)

1. __Running Version:__

  `PostgreSQL 8.3.7`

2. __Current Version:__

  [PostgreSQL 9.4.5](http://www.postgresql.org/docs/9.4/static/release-9-4-5.html)

3. __Vulnerabilty Tradeoffs:__

  Key: Costs (-) and Benefits (+) 

  | [Running Version](http://www.cvedetails.com/vulnerability-list/vendor_id-336/product_id-575/version_id-81449/Postgresql-Postgresql-8.4.html)  |  [Current Version](https://www.cvedetails.com/vulnerability-list/vendor_id-336/product_id-575/Postgresql-Postgresql.html)   |
  |:------------------------------:|:------------------------------:|
  | - [CVE-2013-1902; CVSS 10](https://www.cvedetails.com/cve/CVE-2013-1902/):  insecure temporary files with predictable filenames, which has unspecified impact and attack vectors related to "graphical installers for Linux and Mac OS X." 	  | +  [CVE-2013-5288; CVSS 6.4] (https://www.cvedetails.com/cve/CVE-2013-5288/): before 9.4.5 allows attackers to cause a denial of service (server crash) or read arbitrary server memory via a "too-short" salt.	           |   
  |  - [CVE-2013-1903; CVSS 10](https://www.cvedetails.com/cve/CVE-2013-1903/):  incorrectly provides the superuser password to scripts related to "graphical installers for Linux and Mac OS X," which has unspecified impact and attack vectors | +  [CVE-2013-5289; CVSS 6.4] (https://www.cvedetails.com/cve/CVE-2013-5289/): before 9.4.5 allow attackers to cause a denial of service (server crash) via unspecified vectors, which are not properly handled in (1) json or (2) jsonb values.	   |  
  |  - [CVE-2013-0255; CVSS 6.8](https://www.cvedetails.com/cve/CVE-2013-0255/):  does not properly declare the enum_recv function in backend/utils/adt/enum.c, which causes it to be invoked with incorrect arguments and allows remote authenticated users to cause a denial of service (server crash) or read sensitive process memory via a crafted SQL command|         +    |   
  |  - [CVE-2012-0868; CVSS 6.8](https://www.cvedetails.com/cve/CVE-2012-0868/): allows user-assisted remote attackers to execute arbitrary SQL commands via a crafted file containing object names with newlines |     +      |   
  |  - [CVE-2012-0866; CVSS 6.5] (https://www.cvedetails.com/cve/CVE-2012-0866/):allows remote authenticated users to execute otherwise restricted triggers on arbitrary data by installing the trigger on an attacker-owned table |    +       |   

  
  <INSERT GRAPHS>
4.  __Advice:__ 

  A breif survey over Common Vulnerabilities and Exposures databases reveals th. Therefore marginal benefits of upgrading   the current version seems to outweight the marginal costs (security faults above and migration) upgrading to the latest   version is *__recommended__*.
  
<mention dates released here for better arguments, remeber that after 5 years goes unsupported>
<portfolio for newest version has no significant developements yet>
<mention the the good ones (esp with ubuntu) had lower scores >

## PHP


[back to software](#software-running-on-server)

1. __Running Version:__

  `PHP 5.2.4`

2. __Current Version:__

  [PHP 5.6.15](http://php.net/archive/2015.php#id2015-10-29-2)
  
3. __Vulnerabilty Tradeoffs:__

  Key: Costs (-) and Benefits (+) 

 [Running Version](http://www.cvedetails.com/vulnerability-list.php?vendor_id=74&product_id=128&version_id=47471&page=1&hasexp=0&opdos=0&opec=1&opov=0&opcsrf=0&opgpriv=0&opsqli=0&opxss=0&opdirt=0&opmemc=0&ophttprs=0&opbyp=0&opfileinc=0&opginf=0&cvssscoremin=0&cvssscoremax=0&year=0&month=0&cweid=0&order=3&trc=18&sha=9d8f2dcdd959622955c5467d56f10865dc5539f5)  |  [Current Version](https://www.cvedetails.com/vulnerability-list/vendor_id-74/product_id-128/version_id-170304/PHP-PHP-5.5.15.html)   |
  |:------------------------------:|:------------------------------:|
  | - [CVE-2008-0599; CVSS 10](http://www.cvedetails.com/cve/CVE-2008-0599/):  does not properly consider operator precedence when calculating the length of PATH_TRANSLATED, which might allow remote attackers to execute arbitrary code via a crafted URI.	  | +	  [CVE-2014-3669; CVSS 7.5](https://www.cvedetails.com/cve/CVE-2014-3669/):   allows remote attackers to cause a denial of service (application crash) or possibly execute arbitrary code via an argument to the unserialize function that triggers calculation of a large length value.       |   
  | - [CVE-2008-5557; CVSS 10](http://www.cvedetails.com/cve/CVE-2008-5557/):  allows context-dependent attackers to execute arbitrary code via a crafted string containing an HTML entity, which is not properly handled during Unicode conversion	  | +[CVE-2014-8142; CVSS 7.5](https://www.cvedetails.com/cve/CVE-2014-8142/): 	 allows remote attackers to execute arbitrary code via a crafted unserialize call that leverages improper handling of duplicate keys within the serialized properties of an object |   
  | - [CVE-2007-1581; CVSS 9.3](http://www.cvedetails.com/cve/CVE-2007-1581/):   allows context-dependent attackers to execute arbitrary code by interrupting the hash_update_file function via a userspace (1) error or (2) stream handler, which can then be used to destroy and modify internal resources. 	  | +	  |   
  | - [CVE-2008-3658; CVSS 7.5](http://www.cvedetails.com/cve/CVE-2008-3658/):   Buffer overflow in the imageloadfont function in ext/gd/gd.c in PHP 4.4.x before 4.4.9 and PHP 5.2 before 5.2.6-r6 allows context-dependent attackers to cause a denial of service (crash) and possibly execute arbitrary code via a crafted font file. 	  | +	  |   
  | - [CVE-2010-2225; CVSS 7.5](http://www.cvedetails.com/cve/CVE-2010-2225/):   allows remote attackers to execute arbitrary code or obtain sensitive information via serialized data, related to the PHP unserialize function.	  | +	  |  
4.  __Advice:__

  Based on the table above table, the marginal costs of keeping the current 
  version seems to outweight the marginal benefits, thus a upgrade to the
  current version is *__strongly recommended__*.

Apache
---------------------

[back to software](#software-running-on-server)

1. __Running Version:__

  `Apache 2.2.8`

2. __Current Version:__

  [Apache 2.4.17](https://httpd.apache.org/download.cgi#apache24)

3. __Vulnerabilty Tradeoffs:__

  Key: Costs (-) and Benefits (+) 

  | [Running Version](https://www.cvedetails.com/vulnerability-list.php?vendor_id=45&product_id=66&version_id=77221&page=1&hasexp=0&opdos=0&opec=0&opov=0&opcsrf=0&opgpriv=0&opsqli=0&opxss=0&opdirt=0&opmemc=0&ophttprs=0&opbyp=0&opfileinc=0&opginf=0&cvssscoremin=0&cvssscoremax=0&year=0&month=0&cweid=0&order=3&trc=32&sha=d0e301c374dd499819d172fc448bd3fe742d64d0)  |  [Current Version](https://www.cvedetails.com/vulnerability-list/vendor_id-45/product_id-66/version_id-132629/Apache-Http-Server-2.4.2.html)   |
  |:------------------------------:|:------------------------------:|
  | - [CVE-2011-3192; CVSS 7.8](https://www.cvedetails.com/cve/CVE-2011-3192/):  allows remote attackers to cause a denial of service (memory and CPU consumption) via a Range header that expresses multiple overlapping ranges | + [CVE-2015-3183; CVSS 5.0](https://www.cvedetails.com/cve/CVE-2015-3183/): does not properly parse chunk headers, which allows remote attackers to conduct HTTP request smuggling attacks via a crafted request, related to mishandling of large chunk-size values and invalid chunk-extension characters in modules/http/http_filters.c.    |   
  | - [CVE-2013-2249; CVSS 7.5](https://www.cvedetails.com/cve/CVE-2013-2249/):   proceeds with save operations for a session without considering the dirty flag and the requirement for a new session ID, which has unspecified impact and remote attack vectors.  | +      |   
  | - [CVE-2009-1890; CVSS 7.1](https://www.cvedetails.com/cve/CVE-2009-1890/):  does not properly handle an amount of streamed data that exceeds the Content-Length value, which allows remote attackers to cause a denial of service (CPU consumption) via crafted requests  | +      |   
  | - [CVE-2009-1891; CVSS 7.1](https://www.cvedetails.com/cve/CVE-2009-1891/):  compresses large files until completion even after the associated network connection is closed, which allows remote attackers to cause a denial of service (CPU consumption). | +      |   
  | - [CVE-2012-0883; CVSS 6.9](https://www.cvedetails.com/cve/CVE-2012-0883/):  places a zero-length directory name in the LD_LIBRARY_PATH, which allows local users to gain privileges via a Trojan horse DSO in the current working directory during execution of apachectl. | +      |   
  
4.  __Advice:__

  Based on the table above table, the marginal costs of keeping the current 
  version seems to outweight the marginal benefits, thus a upgrade to the
  current version is *recommended*.

Ubuntu
---------------------

[back to software](#software-running-on-server)

1. __Running Version:__

  `Ubuntu 8.04`

2. __Current Versions:__ 
  + [Ubuntu Server 14.04.3 LTS] (http://www.ubuntu.com/download/server)
  + [Ubuntu Server 15.10] (http://www.ubuntu.com/download/server)

3. __Vulnerabilty Tradeoffs:__

  Key: Costs (-) and Benefits (+) 
  
  | [Running Version](https://www.cvedetails.com/vulnerability-list.php?vendor_id=4781&product_id=20550&version_id=144157&page=1&hasexp=0&opdos=0&opec=0&opov=0&opcsrf=0&opgpriv=0&opsqli=0&opxss=0&opdirt=0&opmemc=0&ophttprs=0&opbyp=0&opfileinc=0&opginf=0&cvssscoremin=0&cvssscoremax=0&year=0&month=0&cweid=0&order=3&trc=13&sha=0b76ae423ecaba9203cc63eab8e210212296bba2)  |  [Ubuntu Server 14.04.3 LTS](https://www.cvedetails.com/vulnerability-list/vendor_id-45/product_id-66/version_id-132629/Apache-Http-Server-2.4.2.html)   | [Ubuntu Server 15.10] (http://www.ubuntu.com/download/server)
  |:------------------------------:|:------------------------------:|:------------------------------:|
  | - [CVE-2008-4063; CVSS 9.3](https://www.cvedetails.com/cve/CVE-2008-4063/):  remote attackers to cause a denial of service (memory corruption and application crash) or possibly execute arbitrary code via vectors related to the layout engine and (1) a zero value of the "this" variable in the nsContentList::Item function; (2) interaction of the indic IME extension, a Hindi language selection, and the "g" character; and (3) interaction of the nsFrameList::SortByContentOrder function with a certain insufficient protection of inline frames. | + |  + |   
  | - [CVE-2012-3406; CVSS 6.8](https://www.cvedetails.com/cve/CVE-2012-3406/):   does not "properly restrict the use of" the alloca function when allocating the SPECS array, which allows context-dependent attackers to bypass the FORTIFY_SOURCE format-string protection mechanism and cause a denial of service (crash) or possibly execute arbitrary code via a crafted format string using positional parameters and a large number of format specifiers | + |  + |   
  | - [CVE-2013-1899; CVSS 6.5](https://www.cvedetails.com/cve/CVE-2013-1899/):  allows remote attackers to cause a denial of service (file corruption), and allows remote authenticated users to modify configuration settings and execute arbitrary code, via a connection request using a database name that begins with a "-" (hyphen). | + | +  |   
 | - [CVE-2011-3152; CVSS 6.4](https://www.cvedetails.com/cve/CVE-2011-3152/):  does not verify the GPG signature before extracting an upgrade tarball, which allows man-in-the-middle attackers to (1) create or overwrite arbitrary files via a directory traversal attack using a crafted tar file, or (2) bypass authentication via a crafted meta-release file. | + |  + |   
 

4.  __Advice:__

  Based on the table above table, the marginal costs of keeping the current 
  version seems to outweight the marginal benefits. Additionally, the long
  term support version of software guarentees longer support thus a upgrade to the
  *Ubuntu Server 14.04.3 LTS* is recommended.

