# IonTorrent-VariantCaller


### get reference genome hg19 from Ion Torrent, Length: 868170684 (828M)
$ wget http://ionupdates.com/reference/hg19.zip
$ unzip hg19.zip

### create hg19.fasta.fai
$ samtools faidx hg19.fasta    

$ md5sum hg19.zip hg19.fasta hg19.fasta.fai
21afdf14b6b3734e0d25157e60bdfb24  hg19.zip
d6851f9f4537ff4e9beb5b7a08b89230  hg19.fasta
cdcd61262b6f4af8e8ac6c2be88a161a  hg19.fasta.fai

$ ls -l
-rw-r--r-- 1 ionadmin ionadmin 3147289017 Mar 22  2011 hg19.fasta
-rw-rw-r-- 1 ionadmin ionadmin        788 Nov 18 16:18 hg19.fasta.fai
-rw-rw-r-- 1 ionadmin ionadmin  868170684 Jul  8  2014 hg19.zip



See also https://ioncommunity.thermofisher.com/community/products/software/torrent-variant-caller
