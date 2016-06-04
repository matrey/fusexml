# fusexml
Expose some files in a custom folder structure, all configured by a XML file

Requirements:
* python2, fuse
* python fuse (on ubuntu ```sudo apt-get install python-fuse```)
* fuse.py from [fusepy](https://www.stavros.io/posts/python-fuse-filesystem/)

Reading list
* [Writing a FUSE filesystem in Python](https://www.stavros.io/posts/python-fuse-filesystem/) for the passthrough code
* [Python FUSE](http://www.slideshare.net/matteobertozzi/python-fuse) lists how FUSE commands are called on slides 20 to 22
* [redditVFS](https://github.com/redditvfs/redditvfs) for an example of fake filesystem
