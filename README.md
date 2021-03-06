# WebDAV-Kerberos #

WebDAV-Kerberos is a Kerberised subclass of the davlib.DAV class found in the
Python_WebDAV_Library package. This module depends on Python_WebDAV_Library
and PyKerberos. Python 3 is not (yet) supported. Installing and configuring
Kerberos properly is left as an exercise for the reader.


## Dependencies ##

* Python_WebDAV_Library (tested with version 0.4.2)
* [PyKerberos](https://svn.calendarserver.org/repository/calendarserver/PyKerberos/trunk/) (tested with revision 11110)



## Usage ##

The interface is exactly the same as [davlib.DAV](http://bazaar.launchpad.net/~datafinder-team/python-webdav-lib/trunk/view/head:/lib/davlib.py).

Krb5DAV includes an extra constructor argument and an extra function. Specify
the *principal* constructor argument to set the client user principal name
you wish to connect as. Omitting this argument will cause the Kerberos client
to use the principal of the current user.

The **Krb5DAV.whoami()** function will return the authenticated user principal
name. If called before authentication the function will return the value
of the *principal* constructor argument, which may be the empty string if
you omitted the argument.


## Examples ##

```python
from krb5dav import Krb5DAV

# Connect to SharePoint with the credentials of the current user. You must
# have a fresh ticket in your Kerberos credentials cache for this to work.
dav = Krb5DAV('sharepoint.example.com', protocol='http')

response = dav.get('/MySite/Home/Shared%20Documents/foo.docx')
with open('/tmp/foo.docx', 'wb') as outfile:
    outfile.write(response.read())
dav.close()

# Connect to SharePoint with specific credentials. You must have a Kerberos
# keytab file with the principal's key, and the current user must have read
# access to it.
dav = Krb5DAV('sharepoint.example.com', protocol='http', principal='jdoe@EXAMPLE.COM')

with open('/tmp/foo.docx', 'rb') as infile:
    buf = infile.read()
dav.put('/MySite/Home/Shared%20Documents/foo2.docx', buf)
dav.close()
```
