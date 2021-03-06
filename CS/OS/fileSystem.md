# ๐ File System

## ๐ Table of Contents
> File and File System

> File system Implementation

>> Allocation of File Data in Disk

>>> Contiguous Alloc

>>> Linked Alloc

>>> Indexed Alloc

> Directory system Implementation


<br><br>

## ๐ File and File System
- File 
    - A named collection of related information
    - ์ผ๋ฐ์ ์ผ๋ก ๋นํ๋ฐ์ฑ ๋ณด์กฐ๊ธฐ์ต์ฅ์น์ ์ ์ฅ
    - ์ด์์ฒด์ ๋ ๋ค์ํ ์ ์ฅ ์ฅ์น๋ฅผ file์ด๋ผ๋ ๋์ผํ ๋ผ๋ฆฌ์  ๋จ์๋ก ๋ณผ ์ ์๊ฒ ํด์ค
    - Operation
        - crete, read, write, reposition, delete, open, close ๋ฑ
- File attribute(metadata)
    - ํ์ผ ์์ฒด์ ๋ด์ฉ์ด ์๋๋ผ ํ์ผ์ ๊ด๋ฆฌํ๊ธฐ ์ํ ๊ฐ์ข ์ ๋ณด๋ค
        - ํ์ผ ์ด๋ฆ, ์ ํ, ์ ์ฅ ์์น, ํ์ผ ์ฌ์ด์ฆ
        - ์ ๊ทผ ๊ถํ(์ฝ๊ธฐ/์ฐ๊ธฐ/์คํ), ์๊ฐ (์์ฑ/๋ณ๊ฒฝ/์ฌ์ฉ), ์์ ์ ๋ฑ
- File system
    - ์ด์์ฒด์ ์์ ํ์ผ์ ๊ด๋ฆฌํ๋ ๋ถ๋ถ
    - ํ์ผ ๋ฐ ํ์ผ์ ๋ฉํ๋ฐ์ดํฐ, ๋๋ ํ ๋ฆฌ ์ ๋ณด ๋ฑ์ ๊ด๋ฆฌ
    - ํ์ผ์ ์ ์ฅ ๋ฐฉ๋ฒ ๊ฒฐ์ 
    - ํ์ผ ๋ณดํธ ๋ฑ

### Directory and Logical Disk
- Directory
    - ํ์ผ์ ๋ฉํ๋ฐ์ดํฐ ์ค ์ผ๋ถ๋ฅผ ๋ณด๊ดํ๊ณ  ์๋ ์ผ์ข์ ํน๋ณํ ํ์ผ
    - ๊ทธ ๋๋ ํ ๋ฆฌ์ ์ํ ํ์ผ ์ด๋ฆ ๋ฐ ํ์ผ attribute๋ค
    - operation
        - search for a file, create a file, delete a file
        - list a directory, rename a file, traverse the file system
- Partition(=Logical Disk)
    - ํ๋์ (๋ฌผ๋ฆฌ์ ) ๋์คํฌ ์์ ์ฌ๋ฌ ํํฐ์์ ๋๋๊ฒ ์ผ๋ฐ์ 
    - ์ฌ๋ฌ ๊ฐ์ ๋ฌผ๋ฆฌ์ ์ธ ๋์คํฌ๋ฅผ ํ๋์ ํํฐ์์ผ๋ก ๊ตฌ์ฑํ๊ธฐ๋ ํจ
    - (๋ฌผ๋ฆฌ์ ) ๋์คํฌ๋ฅผ ํํฐ์์ผ๋ก ๊ตฌ์ฑํ ๋ค ๊ฐ๊ฐ์ ํํฐ์์ file system์ ๊น๊ฑฐ๋ swapping ๋ฑ ๋ค๋ฅธ ์ฉ๋๋ก ์ฌ์ฉํ  ์ ์์

### File Operation
> Create, Delete, Open, Close, REad, Write, Append, Seek, Get attributes, Set attributes, Rename

### open()

> ![open](img/filesystem/open.png)

- ๋ฉํ๋ฐ์ดํฐ๋ฅผ ๋ฉ๋ชจ๋ฆฌ๋ก ์ฌ๋ ค๋๋ ๊ฒ. openํ๊ฒ ๋๋ฉด file์ metadata๊ฐ ๋ฉ๋ชจ๋ฆฌ๋ก ์ฌ๋ผ์จ๋ค.
- open("a/b/c")
    - ๋์คํฌ๋ก๋ถํฐ ํ์ผ c์ ๋ฉํ๋ฐ์ดํฐ๋ฅผ ๋ฉ๋ชจ๋ฆฌ๋ก ๊ฐ์ง๊ณ  ์ด
    - ์ด๋ฅผ ์ํ์ฌ directory path๋ฅผ search
        - ๋ฃจํธ ๋๋ ํ ๋ฆฌ "/"๋ฅผ openํ๊ณ  ๊ทธ ์์์ ํ์ผ 'a'์ ์์น ํ๋
        - ํ์ผ 'a'๋ฅผ openํ ํ readํ์ฌ ๊ทธ ์์์ ํ์ผ 'b'์ ์์น ํ๋..
    - directory path์ search์ ๋๋ฌด ๋ง์ ์๊ฐ ์์
        - open์ read/write์ ๋ณ๋๋ก ๋๋ ์ด์ ์
        - ํ๋ฒ openํ ํ์ผ์ read/write์ ๋ณ๋๋ก ๋๋ ์ด์ ์
    - open file table
        - ํ์ฌ open๋ ํ์ผ๋ค์ ๋ฉํ๋ฐ์ดํฐ ๋ณด๊ด์ (in memory)
        - ๋์คํฌ์ ๋ฉํ๋ฐ์ดํฐ๋ณด๋ค ๋ช ๊ฐ์ง ์ ๋ณด๊ฐ ์ถ๊ฐ
            - openํ ํ๋ก์ธ์ค์ ์
            - file offset :ํ์ผ ์ด๋ ์์น ์ ๊ทผ ์ค์ธ์ง ํ์(๋ณ๋์ ํ์ด๋ธ ํ์)
    - file descriptor (file handle, file control block)
        - open file table์ ๋ํ ์์น ์ ๋ณด(ํ๋ก์ธ์ค ๋ณ)

> ![dirFile](img/filesystem/openFile.png)

### File Protection
- ๊ฐ ํ์ผ์ ๋ํด ๋๊ตฌ์๊ฒ ์ด๋ค ์ ํ์ ์ ๊ทผ์ ํ๋ฝํ  ๊ฒ์ธ๊ฐ?
- Access Control ๋ฐฉ๋ฒ
    - Access control matrix
        - Access control list :ํ์ผ๋ณ๋ก ๋๊ตฌ์๊ฒ ์ด๋ค ์ ๊ทผ ๊ถํ์ด ์๋ ํ์
        - capability :์ฌ์ฉ์๋ณ๋ก ์์ ์ด ์ ๊ทผ ๊ถํ์ ๊ฐ์ง ํ์ผ ๋ฐ ํด๋น ๊ถํ ํ์
    - grouping
        - ์ ์ฒด user๋ฅผ owner, group, public์ ์ธ ๊ทธ๋ฃน์ผ๋ก ๊ตฌ๋ถ
        - ๊ฐ ํ์ผ์ ๋ํด ์ธ ๊ทธ๋ฃน์ ์ ๊ทผ ๊ถํ(rwx)์ 3๋นํธ์ฉ์ผ๋ก ํ์ 
        - UNIX
    - password
        - ํ์ผ๋ง๋ค pwd๋ฅผ ๋๋ ๋ฐฉ๋ฒ
        - ๋ชจ๋  ์ ๊ทผ ๊ถํ์ ๋ํด ํ๋์ pwd :all or nothing
        - ์ ๊ทผ ๊ถํ๋ณ pwd :์๊ธฐ ๋ฌธ์ , ๊ด๋ฆฌ ๋ฌธ์ 

### Access Methods
- Sequential access, ์์ฐจ ์ ๊ทผ
    - read all bytes/records from the beginning
    - ์ค๊ฐ์ ํ์ผ์ ๊ฑด๋ ๋์ด์ ์ฝ์ ์ ์๋ค.
    - ์นด์ธํธ ํ์ดํ๊ฐ ๋งค๊ฐ์ฒด๋ผ๋ฉด ํธ๋ฆฌํจ.
- Random access, ์ง์  ์ ๊ทผ
    - Bytes/records read in any order
    - ์ด๋ ํ ์์๋ผ๋ ์ฝ์ ์ ์๋ค(db๋ random access๊ฐ ํ์์ ์)
    - LP ๋ ์ฝ๋ ํ๊ณผ ๊ฐ์ด ์ ๊ทผ

<br><br><br>

## ๐ File system implementation
> ํ์ผ์ ํฌ๊ธฐ๋ ๋์ผํ์ง ์์ง๋ง ๋์คํฌ์ ๋์ผํ ํฌ๊ธฐ์ ์ ์ฅ ๋จ์๋ก ํ์ผ์ ์ ์ฅํ๊ณ  ์์. ์ ์ฅ ๋จ์ = block ๋จ์

### Allocation of File Data in Disk
- Contiguous Alloc
- Linked Alloc
- Indexed Alloc


#### Contiguous Allocation, ์ฐ์ ํ ๋น
> ![conti](img/filesystem/conti.jpeg)

- ์ฅ์ 
    - ๊ตฌํ์ด ์ฝ๋ค
        - ๋์คํฌ์ ์ฃผ์๊ฐ ๊ฐ ์ฒซ ๋ฒ์งธ ๋ธ๋ก์.
        - ํ์ผ์ ์ด๋ฆ ์์น ์ ๋ณด๋ฅผ ๋๋ ํ ๋ฆฌ๊ฐ ๊ฐ์ง๊ณ  ์์
    - ์ฝ๊ธฐ ์ฑ๋ฅ์ด ๋น ๋ฅด๋ค.(Fase io) (์ค์๊ฐ์ฉ์ผ๋ก ์ฌ์ฉ ์๋๋ฉด run ์ค์ด๋ process์ swapping์ฉ์ผ๋ก ์ฌ์ฉํ  ์ ์๋ค.)
    - Direct access ๊ฐ๋ฅ
- ๋จ์ 
    - ์๊ฐ์ด ์ง๋ ์๋ก ๋์คํฌ ํํธํ๊ฐ ๋ฐ์ํ  ์ ์๋ค. (์ธ๋ถ ์กฐ๊ฐ ๋ฐ์)
    - ํ์ผ์ ํฌ๊ธฐ๋ฅผ ํค์ฐ๋๋ฐ ์ ์ฝ์ด ์๋ค.
    - ํ์ผ์ ํฌ๊ธฐ๋ฅผ ํค์ฐ๋ ์ค์ด๋๋์ ๋ฐ๋ผ hole ๋ฐ์

      
#### Linked Allocation, ์ฐ๊ฒฐ ํ ๋น
> ![linked](img/filesystem/linked.jpeg)

- ์ฅ์ 
    - ์ธ๋ถ ๋จํธํ ๋ฐ์ํ์ง ์๋๋ค
- ๋จ์ 
    - No random access
    - reliability ๋ฌธ์ ๊ฐ ์๋ค.
        - ํ sector๊ฐ ๊ณ ์ฅ๋ pointer๊ฐ ์ ์ค๋๋ฉด ๋ง์ ๋ถ๋ถ์ ์์
    - pointer๋ฅผ ์ํ ๊ณต๊ฐ์ด block์ ์ผ๋ถ๊ฐ ๋์ด ๊ณต๊ฐ ํจ์จ์ฑ์ ๋จ์ด๋จ๋ฆผ
        - 512 bytes/sector, 4byte/pointer
- ๋ณํ
    - file-allocation table (FAT) ํ์ผ ์์คํ
        - ํฌ์ธํฐ๋ฅผ ๋ณ๋์ ์์น์ ๋ณด๊ดํ์ฌ reliability์ ๊ณต๊ฐํจ์จ์ฑ ๋ฌธ์  ํด๊ฒฐ

#### Indexed Allocation, ์ธ๋ฑ์ค๋ ํ ๋น
> ![indexed](img/filesystem/indexed.jpeg)

- ์ฅ์ 
    - ์ธ๋ถ ์กฐ๊ฐ์ด ๋ฐ์ํ์ง ์๋๋ค.
    - direct access ๊ฐ๋ฅ
- ๋จ์ 
    - small file์ ๊ฒฝ์ฐ ๊ณต๊ฐ ๋ญ๋น(์ค์ ๋ก ๋ง์ file๋ค์ด small)
    - too large file์ ๊ฒฝ์ฐ ํ๋์ block์ผ๋ก index๋ฅผ ์ ์ฅํ๊ธฐ์ ๋ถ์กฑ
        - ํด๊ฒฐ ๋ฐฉ์
            1. linked scheme
            2. multi-level index

## UNIX ํ์ผ์์คํ์ ๊ตฌ์กฐ
> ![unix](img/filesystem/unixF.png)

- Boot block :๋ถํ์ ํ์ํ ์ ๋ณด
- Super block :ํ์ผ ์์คํ์ ๊ดํ ์ด์ฒด์ ์ธ ์ ๋ณด๋ฅผ ๋ด๊ณ  ์์
- Inode list :ํ์ผ ์ด๋ฆ์ ์ ์ธํ ํ์ผ์ ๋ชจ๋  ๋ฉํ ๋ฐ์ดํฐ๋ฅผ ์ ์ฅ
- Data block :ํ์ผ์ ์ค์  ๋ด์ฉ์ ๋ณด๊ด
- directory์ metadata์ ์ฅํ์ง ์๊ณ  inode ๋ฒํธ๋ฅผ directory๊ฐ ์ ์ฅํ๊ณ  ์์.

## FAT file system(windows)
> ![windows](img/filesystem/windows.png)

- Linked Allocation ๋ณํํ์ฌ ์ฌ์ฉ
- Boot block 
- FAT
- Root directory
- Data block

### Windows vs. UNIX
> ![windMac](img/filesystem/windMac.jpeg)

### Free Space management
> ๋น์ด์๋ ๋ธ๋ก ์ด๋ป๊ฒ ๊ด๋ฆฌ ํ  ๊ฒ์ธ๊ฐ.
- Bitmap
> ![bitmap](img/filesystem/bitmap.png)

    - bitmap์ ๋ธ๋ก ๋ณ๋ก ๋นํธ๋ฅผ ๋ฌ์ ์ฌ์ฉ์ค์ธ์ง ์๋์ง ๊ตฌ๋ถ
    - ๋ถ๊ฐ์ ์ธ ๊ณต๊ฐ ํ์
    - ์ฐ์์ ์ธ n๊ฐ์ free block์ ์ฐพ๋๋ฐ ํจ๊ณผ์ 
- Linked list
    - ๋ชจ๋  free block๋ค์ ๋งํฌ๋ก ์ฐ๊ฒฐ
    - ์ฐ์์ ์ธ ๊ฐ์ฉ๊ณต๊ฐ์ ์ฐพ๋ ๊ฒ์ ์ฝ์ง ์๋ค
    - ๊ณต๊ฐ์ ๋ญ๋น๊ฐ ์๋ค
- Grouping
    - linked list ๋ฐฉ๋ฒ์ ๋ณํ
    - ์ฒซ๋ฒ์งธ free block์ด n๊ฐ์ pointer๋ฅผ ๊ฐ์ง
        - n-1 ํฌ์ธํฐ๋ free data block์ ๊ฐ๋ฆฌํด
        - ๋ง์ง๋ง pointer๊ฐ ๊ฐ๋ฆฌํค๋ block์ ๋ ๋ค์ N pointer๋ฅผ ๊ฐ์ง
- Counting
    - ํ๋ก๊ทธ๋จ๋ค์ด ์ข์ข ์ฌ๋ฌ ๊ฐ์ ์ฐ์์ ์ธ block์ ํ ๋นํ๊ณ  ๋ฐ๋ฉํ๋ค๋ ์ฑ์ง์ ์ฐฉ์
    - first free block, # of contiguous free blocks๋ฅผ ์ ์ง


<br><br><br>

## ๐ Directory Implementation
- Linear list
    - <file name, file์ metadata>์ list
    - ๊ตฌํ์ด ๊ฐ๋จ
    - ๋๋ ํ ๋ฆฌ ๋ด์ ํ์ผ์ด ์๋์ง ์ฐพ๊ธฐ ์ํด์๋ linear search ํ์(time consuming)
- Hash table
    - Linear list + hashing
    - Hash table์ file name์ ์ด ํ์ผ์ linear list์ ์์น๋ก ๋ฐ๊พธ์ด์ค
    - search time์ ์์ฐ
    - collision ๋ฐ์ ๊ฐ๋ฅ 
- File์ metadata์ ๋ณด๊ด ์์น
    - ๋๋ ํ ๋ฆฌ ๋ด์ ์ง์  ๋ณด๊ด
    - ๋๋ ํ ๋ฆฌ์๋ ํฌ์ธํฐ๋ฅผ ๋๊ณ  ๋ค๋ฅธ ๊ณณ์ ๋ณด๊ด
        - inode, FAT
- Long file name์ ์ง์
    - <file name, file์ metadata>์ list์์ ๊ฐ entry๋ ์ผ๋ฐ์ ์ผ๋ก ๊ณ ์  ํฌ๋ฆญ
    - file name์ด ๊ณ ์  ํฌ๊ธฐ์ entry ๊ธธ์ด๋ณด๋ค ๊ธธ์ด์ง๋ ๊ฒฝ์ฐ entry์ ๋ง์ง๋ง ๋ถ๋ถ์ ์ด๋ฆ์ ๋ท๋ถ๋ถ์ด ์์นํ ๊ณณ์ ํฌ์ธํฐ๋ฅผ ๋๋ ๋ฐฉ๋ฒ
    - ์ด๋ฆ์ ๋๋จธ์ง ๋ถ๋ถ์ ๋์ผํ directory file์ ์ผ๋ถ์ ์กด์ฌ

<br><br><br>

### VFS and NFS

- Virtual File System(VFS)
    - ์๋ก ๋ค๋ฅธ ๋ค์ํ file system์ ๋์ผํ ์์คํ ์ฝ ์ธํฐํ์ด์ค(API)๋ฅผ ํตํด ์ ๊ทผํ  ์ ์๊ฒ ํด์ฃผ๋ OS์ layer
- Network File System(NFS)
> ![nfs](img/filesystem/nfs.png)

- ๋ถ์ฐ ์์คํ์์๋ ๋คํธ์ํฌ๋ฅผ ํตํด ํ์ผ์ด ๊ณต์ ๋  ์ ์์
- NFS๋ ๋ถ์ฐ ํ๊ฒฝ์์์ ๋ํ์ ์ธ ํ์ผ ๊ณต์  ๋ฐฉ๋ฒ์

### page cache and buffer cache
- Page cache
    - Virtual memory์ paging system์์ ์ฌ์ฉํ๋ page frame์ caching์ ๊ด์ ์์ ์ค๋ชํ๋ ์ฉ์ด
    - Memory-mapped I/O๋ฅผ ์ฐ๋ ๊ฒฝ์ฐ file์ IO์์๋ page cache์ฌ์ฉ
- Memory-mapped I/O
    - File์ ์ผ๋ถ๋ฅผ virtual memory์ mapping ์ํด
    - mapping์ํจ ์์ญ์ ๋ํ ๋ฉ๋ชจ๋ฆฌ ์ ๊ทผ ์ฐ์ฐ์ ํ์ผ์ ์์ถ๋ ฅ์ ์ํํ๊ฒ ํจ
- Buffer cache
    - ํ์ผ์์คํ์ ํตํ IO ์ฐ์ฐ์ ๋ฉ๋ชจ๋ฆฌ์ ํน์  ์์ญ์ธ buffer cache ์ฌ์ฉ
    - file ์ฌ์ฉ์ locality ํ์ฉ
        - ํ๋ฒ ์ฝ์ด์จ block์ ๋ํ ํ์ ์์ฒญ์ buffer cache์์ ์ฆ์ ์ ๋ฌ
    - ๋ชจ๋  ํ๋ก์ธ์ค๊ฐ ๊ณต์ฉ์ผ๋ก ์ฌ์ฉ
    - replacement algorithm ํ์(LRU, LFU)
- Unified Buffer Caches
    - ์ต๊ทผ OS์์๋ ๊ธฐ์กด์ ๋ฒํผ ์บ์๊ฐ page cache์ ํตํฉ

> ![pbcache0](img/filesystem/pbCache2.png)

> ![pbcache](img/filesystem/pbCache.png)

#### program execution
> ![pe](img/filesystem/pe.png)

- ๋ฉ๋ชจ๋ฆฌ์ code๊ฐ ์์ฌ๋ผ์์์ผ๋ฉด file system์์ ๋ถ๋ฌ์ด
- memory mapped file์ด๋ผ๋ฉด swap area๋ก ๊ฐ๋ ๊ฒ์ด ์๋๋ผ ์์ ์ ์ฃผ์๊ณต๊ฐ์ ๋งคํ๋์ด ์์(?)

์ด์์ฒด์  ์ด๋ก ์ด ๋๋ฌ์ต๋๋ค. ๊ฐ์ฅ ๊ธธ๊ฒ ๋๊ปด์ง๋ ๊ณผ๋ชฉ์ด์ง ์์๋ ์ถ์ต๋๋ค. ๋ค๋ฅธ ์ ๋ช ๋ฆฌํฌ๋ฅผ ๋ณด๋ฉด ๋ค๋ฃจ์ง ์๋ ๊ณณ์ด ๋๋ค์์ธ ๊ฑฐ ๊ฐ์ต๋๋ค. ๊ทธ๋์ ๊ฐ์ธ์ ์ผ๋ก ์์ธํ ํ๋์ ์ฃผ์ ๋ฅผ ์๊ธฐ๋ณด๋ค ํฐ ํ์์ ์ด๋ค ๋ฐฉ์๊ณผ ๋ฐฉ๋ฒ์ ๊ฐ์ง๊ณ  ๊ตฌํ์ ํ  ์ ์๋์ง ์๋๊ฒ ์ค์ํ๋ค๊ณ  ์๊ฐํฉ๋๋ค ใใ 

<br><br>

## ๐ ์ฐธ๊ณ 
[๋ฐ๊ต์๋ ์ด์์ฒด์ ](https://core.ewha.ac.kr/publicview/)

<br>

## โ๏ธ ๋ฉด์  ์์ ์ง๋ฌธ

> 1. ํ์ผ ์์คํ์ด ๋ฌด์์ธ๊ฐ์?

> 2. Directory๊ฐ ๋ฌด์์ธ๊ฐ์?

> 3. ํ์ผ ์์คํ ๊ตฌํ ๋ฐฉ๋ฒ์๋ ๋ฌด์์ด ์๊ณ  ๊ฐ ๊ตฌํ ๋ฐฉ๋ฒ์ ์ค๋ชํด์ฃผ์ธ์

>> 3-1. Contiguous Alloc์ ํน์ง์ ๋ฌด์์ธ๊ฐ์?

>> 3-2. Linked Alloc์ ํน์ง์ ๋ฌด์์ธ๊ฐ์?

>> 3-3. Indexed Alloc์ ํน์ง์ ๋ฌด์์ธ๊ฐ์?

> 4. ๋ฉ๋ชจ๋ฆฌ์ ๋น๊ณต๊ฐ์ ์ด๋ป๊ฒ ๊ด๋ฆฌํ๋์? ๊ฐ ๊ด๋ฆฌ ๋ฐฉ๋ฒ์ ํน์ง์ ์ค๋ชํด์ฃผ์ธ์

> 5. ๋๋ ํ ๋ฆฌ์ ๊ตฌํ ๋ฐฉ๋ฒ์ ๋ํด์ ์ค๋ชํด์ฃผ์ธ์