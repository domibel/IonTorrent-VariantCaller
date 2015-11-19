# IonTorrent-VariantCaller


#### Get reference genome hg19 from Ion Torrent, Length: 868170684 (828M)
```
$ wget http://ionupdates.com/reference/hg19.zip
$ unzip hg19.zip
```

##### Create hg19.fasta.fai
```
$ samtools faidx hg19.fasta    
```

##### Content
```
$ md5sum hg19.zip hg19.fasta hg19.fasta.fai
21afdf14b6b3734e0d25157e60bdfb24  hg19.zip
d6851f9f4537ff4e9beb5b7a08b89230  hg19.fasta
cdcd61262b6f4af8e8ac6c2be88a161a  hg19.fasta.fai

$ ls -l
-rw-r--r-- 1 ionadmin ionadmin 3147289017 Mar 22  2011 hg19.fasta
-rw-rw-r-- 1 ionadmin ionadmin        788 Nov 18 16:18 hg19.fasta.fai
-rw-rw-r-- 1 ionadmin ionadmin  868170684 Jul  8  2014 hg19.zip
```
#### Alternative: Get reference genome hg19 with TMAP index files from Ion Torrent, Length: 4613331358 (4.3G)

```
$ wget http://ionupdates.com/reference_downloads/hg19.zip
$ unzip hg19.zip
```

##### Content
```
$ md5sum *
d2dacce998d72687ca1f2dd28ce2280e  CreateSequenceDictionary.log
d264f416c20f7d144d74924ab7c356a0  hg19.dict
d6851f9f4537ff4e9beb5b7a08b89230  hg19.fasta
cdcd61262b6f4af8e8ac6c2be88a161a  hg19.fasta.fai
6903f05eef92e5e6f5a6bf890a01d6e6  hg19.fasta.tmap.anno
f9d7c7dedf322022a0393d99a629229b  hg19.fasta.tmap.bwt
19c79b65021812df67904f7448367d5f  hg19.fasta.tmap.pac
ebd201bec6945900ce9f26b62cf89915  hg19.fasta.tmap.sa
09b3113801b9c6ae69134f8712ef6a8c  hg19.info.txt
3bc8b68ce604c77f2fecd14ab20a13bc  hg19.md5sum.txt
3b725f35b395cf763bb7ac2110ab8d1a  hg19.zip
d41d8cd98f00b204e9800998ecf8427e  samtools.log
b9ba9df7fc3cd18883e7922bc6dd20d6  tmap.log
```
```
$ ls -l
total 13218824
-rwxr-xr-x 1 ionadmin ionadmin        624 May  6  2013 CreateSequenceDictionary.log
-rwxr-xr-x 1 ionadmin ionadmin       3174 May  6  2013 hg19.dict
-rwxr-xr-x 1 ionadmin ionadmin 3147289017 May  6  2013 hg19.fasta
-rwxr-xr-x 1 ionadmin ionadmin        788 May  6  2013 hg19.fasta.fai
-rwxr-xr-x 1 ionadmin ionadmin       3985 May  6  2013 hg19.fasta.tmap.anno
-rwxr-xr-x 1 ionadmin ionadmin 3453608024 May  6  2013 hg19.fasta.tmap.bwt
-rwxr-xr-x 1 ionadmin ionadmin  773923497 May  6  2013 hg19.fasta.tmap.pac
-rwxr-xr-x 1 ionadmin ionadmin 1547847008 May  6  2013 hg19.fasta.tmap.sa
-rwxr-xr-x 1 ionadmin ionadmin         92 May  6  2013 hg19.info.txt
-rwxr-xr-x 1 ionadmin ionadmin        642 May  6  2013 hg19.md5sum.txt
-rw-rw-r-- 1 ionadmin ionadmin 4613331358 Jul  8  2014 hg19.zip
-rwxr-xr-x 1 ionadmin ionadmin          0 May  6  2013 samtools.log
-rwxr-xr-x 1 ionadmin ionadmin      15170 May  6  2013 tmap.log
```

See also https://ioncommunity.thermofisher.com/community/products/software/torrent-variant-caller
