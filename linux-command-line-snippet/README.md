# Linux Command Line Snippet

## Introduction

Here I put some frequent command I used

### Count the files in a directory

```ls directory | wc -l```

Note:

`ls:` list files in dir

`-1:` (that's a ONE) only one entry per line. Change it to -1a if you want hidden files too

`| :` pipe output onto...

`wc:` "wordcount"

`-l:` count `l`ines.

### Set up diffrent gcc and g++

```
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-5 50 --slave /usr/bin/g++ g++ /usr/bin/g++-5
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-7 70 --slave /usr/bin/g++ g++ /usr/bin/g++-7
```
`--slave` : make the gcc-5 and g++5 change together


