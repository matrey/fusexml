# fusexml
Expose some files in a custom folder structure, all configured by a XML file

### Requirements

* python2, fuse
* python fuse (on ubuntu ```sudo apt-get install python-fuse```)
* fuse.py from [fusepy](https://www.stavros.io/posts/python-fuse-filesystem/)

### Usage

```
./fusexml.py <XML tree> <mount point>
```

Some remarks:
* we read the XML configuration file once, at startup
* we do not validate whether the "realpath" targets exist or not
* to keep it simple, the mounted filesystem is read-only

### XML configuration file format

The root tag can have an optional "basepath" attribute, that will prefix all file's "realpath"

```
<root basepath="/dev/shm/">
  <Dir name="folder1">
    <File name="file1A.xml" realpath="test.xml"/>
    <File name="file1B.xml" realpath="test.xml"/>
    <File name="file1C.xml" realpath="test.xml"/>
    <File name="file1D.xml" realpath="test.xml"/>
    <File name="file1E.xml" realpath="test.xml"/>
    <File name="file1F.xml" realpath="test.xml"/>
    <Dir name="folder11">
      <File name="file11A.xml" realpath="test.xml"/>
      <File name="file11B.xml" realpath="test.xml"/>
    </Dir>
  </Dir>
  <Dir name="folder2">
    <File name="file2A.xml" realpath="test.xml"/>
    <File name="file2B.xml" realpath="test.xml"/>
    <File name="file2C.xml" realpath="test.xml"/>
    <File name="file2D.xml" realpath="test.xml"/>
    <File name="file2E.xml" realpath="test.xml"/>
    <File name="file2F.xml" realpath="test.xml"/>
  </Dir>
  <Dir name="folder3 with spaces">
    <File name="file 3 A.xml" realpath="test.xml"/>
    <File name="file 3 B.xml" realpath="test.xml"/>
    <File name="file 3 C.xml" realpath="test.xml"/>
    <File name="file 3 D.xml" realpath="test.xml"/>
    <File name="file 3 E.xml" realpath="test.xml"/>
    <File name="file 3 F.xml" realpath="test.xml"/>
  </Dir>
  <Dir name="folder4"/>
  <File name="fileA.xml" realpath="test.xml"/>
</root>
```

The XML above gives the following result:

```
$ find .
.
./folder1
./folder1/file1A.xml
./folder1/file1B.xml
./folder1/file1C.xml
./folder1/file1D.xml
./folder1/file1E.xml
./folder1/file1F.xml
./folder1/folder11
./folder1/folder11/file11A.xml
./folder1/folder11/file11B.xml
./folder2
./folder2/file2A.xml
./folder2/file2B.xml
./folder2/file2C.xml
./folder2/file2D.xml
./folder2/file2E.xml
./folder2/file2F.xml
./folder3 with spaces
./folder3 with spaces/file 3 A.xml
./folder3 with spaces/file 3 B.xml
./folder3 with spaces/file 3 C.xml
./folder3 with spaces/file 3 D.xml
./folder3 with spaces/file 3 E.xml
./folder3 with spaces/file 3 F.xml
./folder4
./fileA.xml
$ ls -laht folder3\ with\ spaces/
total 0
dr-xr-xr-x 2 mathieu mathieu 0 Jun  4 18:44 .
dr-xr-xr-x 2 mathieu mathieu 0 Jun  4 18:44 ..
-r--r--r-- 2 mathieu mathieu 8 Jun  4 18:44 file 3 A.xml
-r--r--r-- 2 mathieu mathieu 8 Jun  4 18:44 file 3 B.xml
-r--r--r-- 2 mathieu mathieu 8 Jun  4 18:44 file 3 C.xml
-r--r--r-- 2 mathieu mathieu 8 Jun  4 18:44 file 3 D.xml
-r--r--r-- 2 mathieu mathieu 8 Jun  4 18:44 file 3 E.xml
-r--r--r-- 2 mathieu mathieu 8 Jun  4 18:44 file 3 F.xml
$ cat folder3\ with\ spaces/file\ 3\ A.xml 
<root/>
$ rm fileA.xml 
rm: cannot remove ‘fileA.xml’: Read-only file system
```

### Reading list

* [Writing a FUSE filesystem in Python](https://www.stavros.io/posts/python-fuse-filesystem/) for an example of passthrough code
* [Python FUSE](http://www.slideshare.net/matteobertozzi/python-fuse) lists how FUSE commands are called (slides 21 and 22)
* [redditVFS](https://github.com/redditvfs/redditvfs) for an example of fake filesystem
