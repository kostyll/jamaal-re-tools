Vol-MsDecompress is a plugin to help forensics investigators recover compressed data in memory. Currently Vol-MsDecompress supports the lznt1 decompression algorithm, however xpress and xpress + huffman will be added soon. Vol-MsDecompress uses a slightly modified version of ms-compress (https://github.com/coderforlife/ms-compress). The modified version exports the lznt1_decompress_chunk function which Vol-MsDecompress uses to precisely decompress data. https://code.google.com/p/jamaal-re-tools/source/browse/volplugins/msdecompress/ms-voldecompress.zip)


Install: 
1) Download ms-voldecompress.zip  from the repository
2) Unzip the the file $ unzip ms-voldecompress.zip 
3) Build it 
$ ./build.sh 
Compiling dynamic shared library...
In file included from lzx.cpp:22:0:
etc..
4) Check for the lib and its exports 
$ ls -l libMSCompression.so 
-rwxrwxr-x 1 ff ff 113000 Sep 30 22:25 libMSCompression.so
$ nm -D libMSCompression.so | grep -i decompress
00000000000025d0 T decompress
0000000000003450 T lznt1_decompress
0000000000003080 T lznt1_decompress_chunk
0000000000006540 T lzx_cab_decompress
0000000000006010 T lzx_wim_decompress
0000000000011be0 T xpress_decompress
0000000000012d70 T xpress_huff_decompress
Copy libMSCompression.so to another location or just call it from its current location (next step will demonstrate)
 
5) Copy the msdecompress.py plugin to vols plugin directory
6) Example usage: 
python vol.py msdecompress -f ../zeus.vmem -D out4 -P ~/projects/mchammer/libMSCompression.so -A lznt1 -C -M 5
Options: 
-P path to shared object 
-A algorithm to use 
-C Dump the compress data along with the decompress data blocks
-M Specify the minimum compressed data to size to decompress in bytes (default is 20 bytes)
Sample Output 
Decompresing data for PID: 4, Process: System, Page: 0x81008000, Offset: 0x0
Decompresing data for PID: 4, Process: System, Page: 0x81009000, Offset: 0x4dc
Decompresing data for PID: 4, Process: System, Page: 0x8100a000, Offset: 0x1fd
Decompresing data for PID: 4, Process: System, Page: 0x8100b000, Offset: 0x49e
Decompresing data for PID: 4, Process: System, Page: 0x8100c000, Offset: 0x702
Decompresing data for PID: 4, Process: System, Page: 0x8100d000, Offset: 0x528
Decompresing data for PID: 4, Process: System, Page: 0x8100e000, Offset: 0x321
Decompresing data for PID: 4, Process: System, Page: 0x81011000, Offset: 0x36c
Decompresing data for PID: 4, Process: System, Page: 0x81013000, Offset: 0x1b3
Sample Output file list out4 directory: 225 compressed steams were recovered using LZNT1 for the Zeus.vmem sample. 
-rw-rw-r-- 1 ff ff 4099 Sep 30 21:13 1028_svchost.exe_0x81008000_0x0_lznt1__CompressedBuffer.bin
-rw-rw-r-- 1 ff ff 12288 Sep 30 21:13 1028_svchost.exe_0x81008000_0x0_lznt1__UncompressedBuffer.bin
-rw-rw-r-- 1 ff ff 2854 Sep 30 21:13 1028_svchost.exe_0x81009000_0x4dc_lznt1__CompressedBuffer.bin
-rw-rw-r-- 1 ff ff 8192 Sep 30 21:13 1028_svchost.exe_0x81009000_0x4dc_lznt1__UncompressedBuffer.bin
-rw-rw-r-- 1 ff ff 3590 Sep 30 21:13 1028_svchost.exe_0x8100a000_0x1fd_lznt1__CompressedBuffer.bin
-rw-rw-r-- 1 ff ff 12288 Sep 30 21:13 1028_svchost.exe_0x8100a000_0x1fd_lznt1__UncompressedBuffer.bin
-rw-rw-r-- 1 ff ff 2917 Sep 30 21:13 1028_svchost.exe_0x8100b000_0x49e_lznt1__CompressedBuffer.bin
-rw-rw-r-- 1 ff ff 12288 Sep 30 21:13 1028_svchost.exe_0x8100b000_0x49e_lznt1__UncompressedBuffer.bin
-rw-rw-r-- 1 ff ff 2304 Sep 30 21:13 1028_svchost.exe_0x8100c000_0x702_lznt1__CompressedBuffer.bin
-rw-rw-r-- 1 ff ff 8192 Sep 30 21:13 1028_svchost.exe_0x8100c000_0x702_lznt1__UncompressedBuffer.bin
-rw-rw-r-- 1 ff ff 2778 Sep 30 21:13 1028_svchost.exe_0x8100d000_0x528_lznt1__CompressedBuffer.bin
-rw-rw-r-- 1 ff ff 8192 Sep 30 21:13 1028_svchost.exe_0x8100d000_0x528_lznt1__UncompressedBuffer.bin
-rw-rw-r-- 1 ff ff 1690 Sep 30 21:13 1028_svchost.exe_0x8100e000_0x321_lznt1__CompressedBuffer.bin
-rw-rw-r-- 1 ff ff 4096 Sep 30 21:13 1028_svchost.exe_0x8100e000_0x321_lznt1__UncompressedBuffer.bin
-rw-rw-r-- 1 ff ff 2949 Sep 30 21:13 1028_svchost.exe_0x81011000_0x36c_lznt1__CompressedBuffer.bin
-rw-rw-r-- 1 ff ff 4096 Sep 30 21:13 1028_svchost.exe_0x81011000_0x36c_lznt1__UncompressedBuffer.bin
-rw-rw-r-- 1 ff ff 2941 Sep 30 21:13 1028_svchost.exe_0x81013000_0x1b3_lznt1__CompressedBuffer.bin
-rw-rw-r-- 1 ff ff 4096 Sep 30 21:13 1028_svchost.exe_0x81013000_0x1b3_lznt1__UncompressedBuffer.bin
-rw-rw-r-- 1 ff ff 4099 Sep 30 21:19 1084_TPAutoConnect.e_0x81008000_0x0_lznt1__CompressedBuffer.bin
-rw-rw-r-- 1 ff ff 12288 Sep 30 21:19 1084_TPAutoConnect.e_0x81008000_0x0_lznt1__UncompressedBuffer.bin
-rw-rw-r-- 1 ff ff 2854 Sep 30 21:19 1084_TPAutoConnect.e_0x81009000_0x4dc_lznt1__CompressedBuffer.bin
-rw-rw-r-- 1 ff ff 8192 Sep 30 21:19 1084_TPAutoConnect.e_0x81009000_0x4dc_lznt1__UncompressedBuffer.bin
-rw-rw-r-- 1 ff ff 3590 Sep 30 21:19 1084_TPAutoConnect.e_0x8100a000_0x1fd_lznt1__CompressedBuffer.bin
-rw-rw-r-- 1 ff ff 12288 Sep 30 21:19 1084_TPAutoConnect.e_0x8100a000_0x1fd_lznt1__UncompressedBuffer.bin
-rw-rw-r-- 1 ff ff 2917 Sep 30 21:19 1084_TPAutoConnect.e_0x8100b000_0x49e_lznt1__CompressedBuffer.bin
-rw-rw-r-- 1 ff ff 12288 Sep 30 21:19 1084_TPAutoConnect.e_0x8100b000_0x49e_lznt1__UncompressedBuffer.bin
-rw-rw-r-- 1 ff ff 2304 Sep 30 21:19 1084_TPAutoConnect.e_0x8100c000_0x702_lznt1__CompressedBuffer.bin
-rw-rw-r-- 1 ff ff 8192 Sep 30 21:19 1084_TPAutoConnect.e_0x8100c000_0x702_lznt1__UncompressedBuffer.bin
-rw-rw-r-- 1 ff ff 2778 Sep 30 21:19 1084_TPAutoConnect.e_0x8100d000_0x528_lznt1__CompressedBuffer.bin
-rw-rw-r-- 1 ff ff 8192 Sep 30 21:19 1084_TPAutoConnect.e_0x8100d000_0x528_lznt1__UncompressedBuffer.bin
-rw-rw-r-- 1 ff ff 1690 Sep 30 21:19 1084_TPAutoConnect.e_0x8100e000_0x321_lznt1__CompressedBuffer.bin
-rw-rw-r-- 1 ff ff 4096 Sep 30 21:19 1084_TPAutoConnect.e_0x8100e000_0x321_lznt1__UncompressedBuffer.bin
-rw-rw-r-- 1 ff ff 2949 Sep 30 21:19 1084_TPAutoConnect.e_0x81011000_0x36c_lznt1__CompressedBuffer.bin
-rw-rw-r-- 1 ff ff 4096 Sep 30 21:19 1084_TPAutoConnect.e_0x81011000_0x36c_lznt1__UncompressedBuffer.bin
-rw-rw-r-- 1 ff ff 2941 Sep 30 21:19 1084_TPAutoConnect.e_0x81013000_0x1b3_lznt1__CompressedBuffer.bin
-rw-rw-r-- 1 ff ff 4096 Sep 30 21:19 1084_TPAutoConnect.e_0x81013000_0x1b3_lznt1__UncompressedBuffer.bin
-rw-rw-r-- 1 ff ff 4099 Sep 30 21:14 1088_svchost.exe_0x81008000_0x0_lznt1__CompressedBuffer.bin
-rw-rw-r-- 1 ff ff 12288 Sep 30 21:14 1088_svchost.exe_0x81008000_0x0_lznt1__UncompressedBuffer.bin
-rw-rw-r-- 1 ff ff 2854 Sep 30 21:14 1088_svchost.exe_0x81009000_0x4dc_lznt1__CompressedBuffer.bin
-rw-rw-r-- 1 ff ff 8192 Sep 30 21:14 1088_svchost.exe_0x81009000_0x4dc_lznt1__UncompressedBuffer.bin
-rw-rw-r-- 1 ff ff 3590 Sep 30 21:14 1088_svchost.exe_0x8100a000_0x1fd_lznt1__CompressedBuffer.bin
-rw-rw-r-- 1 ff ff 12288 Sep 30 21:14 1088_svchost.exe_0x8100a000_0x1fd_lznt1__UncompressedBuffer.bin
-rw-rw-r-- 1 ff ff 2917 Sep 30 21:14 1088_svchost.exe_0x8100b000_0x49e_lznt1__CompressedBuffer.bin
-rw-rw-r-- 1 ff ff 12288 Sep 30 21:14 1088_svchost.exe_0x8100b000_0x49e_lznt1__UncompressedBuffer.bin
-rw-rw-r-- 1 ff ff 2304 Sep 30 21:14 1088_svchost.exe_0x8100c000_0x702_lznt1__CompressedBuffer.bin
-rw-rw-r-- 1 ff ff 8192 Sep 30 21:14 1088_svchost.exe_0x8100c000_0x702_lznt1__UncompressedBuffer.bin
-rw-rw-r-- 1 ff ff 2778 Sep 30 21:14 1088_svchost.exe_0x8100d000_0x528_lznt1__CompressedBuffer.bin
-rw-rw-r-- 1 ff ff 8192 Sep 30 21:14 1088_svchost.exe_0x8100d000_0x528_lznt1__UncompressedBuffer.bin
-rw-rw-r-- 1 ff ff 1690 Sep 30 21:14 1088_svchost.exe_0x8100e000_0x321_lznt1__CompressedBuffer.bin
-rw-rw-r-- 1 ff ff 4096 Sep 30 21:14 1088_svchost.exe_0x8100e000_0x321_lznt1__UncompressedBuffer.bin
-rw-rw-r-- 1 ff ff 2949 Sep 30 21:14 1088_svchost.exe_0x81011000_0x36c_lznt1__CompressedBuffer.bin
-rw-rw-r-- 1 ff ff 4096 Sep 30 21:14 1088_svchost.exe_0x81011000_0x36c_lznt1__UncompressedBuffer.bin
-rw-rw-r-- 1 ff ff 2941 Sep 30 21:14 1088_svchost.exe_0x81013000_0x1b3_lznt1__CompressedBuffer.bin
-rw-rw-r-- 1 ff ff 4096 Sep 30 21:14 1088_svchost.exe_0x81013000_0x1b3_lznt1__UncompressedBuffer.bin
-rw-rw-r-- 1 ff ff 4099 Sep 30 21:15 1148_svchost.exe_0x81008000_0x0_lznt1__CompressedBuffer.bin
-rw-rw-r-- 1 ff ff 12288 Sep 30 21:15 1148_svchost.exe_0x81008000_0x0_lznt1__UncompressedBuffer.bin
-rw-rw-r-- 1 ff ff 2854 Sep 30 21:15 1148_svchost.exe_0x81009000_0x4dc_lznt1__CompressedBuffer.bin
-rw-rw-r-- 1 ff ff 8192 Sep 30 21:15 1148_svchost.exe_0x81009000_0x4dc_lznt1__UncompressedBuffer.bin
--snip----
Example Data
Decompressed data recovered by the plugin can be found in the msdecompress_zeus.zip file located at: https://code.google.com/p/jamaal-re-tools/source/browse/volplugins/msdecompress/msdecompress_zeus.zip
$ hexdump -C 4_System_0x81008000_0x0_lznt1__CompressedBuffer.bin 
00000000 58 b7 00 69 63 61 74 6f 72 20 6f 00 66 20 74 68 |X..icator o.f th|
00000010 65 20 6b 69 08 6e 64 73 01 60 66 61 75 6c 04 74 |e ki.nds.`faul.t|
00000020 73 00 98 61 74 20 63 61 00 75 73 65 20 73 79 73 |s..at ca.use sys|
00000030 74 00 65 6d 2d 77 69 64 65 20 00 64 65 6c 61 79 |t.em-wide .delay|
00000040 73 2e 20 00 49 74 20 69 6e 63 6c 75 00 64 65 73 |s. .It inclu.des|
00000050 20 72 65 61 64 00 20 6f 70 65 72 61 74 69 44 6f | read. operatiDo|
00000060 6e 00 74 6f 20 73 00 14 73 48 66 79 20 04 98 69 |n.to s..sHfy ..i|
00000070 6e 02 c6 66 2c 69 6c 05 9a 00 b4 63 00 75 28 75 |n..f,il....c.u(u|
00000080 40 73 75 61 6c 6c 79 00 43 71 04 75 65 00 66 64 |@sually.Cq.ue.fd|
00000090 20 62 79 20 30 61 70 70 6c 01 9d 01 4d 29 20 08 | by 0appl...M) .|
000000a0 61 6e 64 00 6b 20 6e 6f 6e 88 2d 63 61 00 34 64 |and.k non.-ca.4d|
000000b0 20 6d 00 20 c2 65 00 06 65 6d 6f 72 00 63 00 55 | m. .e..emor.c.U|
000000c0 01 00 91 43 6f 6d 70 61 72 65 41 02 68 76 61 6c |...CompareA.hval|
000000d0 75 65 01 cc 4d 41 02 22 5c 5c 50 61 67 00 a8 52 |ue..MA."\\Pag..R|
000000e0 01 00 a8 73 2f 73 65 63 20 74 82 6f 19 27 49 6e |...s/sec t.o.'In|
000000f0 70 75 74 85 13 00 64 65 74 65 72 6d 69 6e 09 83 |put...determin..|
00000100 2c 61 76 00 73 67 65 20 6e d0 75 6d 62 65 02 9e |,av.sge n.umbe..|
00000110 70 02 2d 82 81 00 64 75 72 69 6e 67 20 65 63 80 |p.-...during ec.|
00000120 52 87 87 2e 00 00 82 3e 02 2a 50 04 65 72 00 3f |R......>.*P.er.?|
00000130 00 13 00 00 00 a8 15 00 88 80 02 03 80 01 2b 80 |..............+.|
00000140 01 40 0a 00 00 80 03 08 00 04 05 50 1f 00 00 0d |.@.........P....|
00000150 80 01 81 01 06 1a 15 00 04 2b 05 06 38 00 04 00 |.........+..8...|
00000160 75 69 00 6e 74 33 32 00 00 44 69 84 73 70 80 c4 |ui.nt32..Di.sp..|
00000170 4e 61 6d 65 84 2f 06 20 86 5a 80 0e 65 73 63 72 |Name./. .Z..escr|
00000180 69 26 70 01 c7 8e 0e 20 69 01 ee 65 20 09 80 d6 |i&p.... i..e ...|
00000190 65 20 c0 78 77 68 69 63 1c 68 20 43 31 c1 4f 42 |e .xwhic.h C1.OB|
000001a0 32 66 72 6f 40 6d 20 64 69 73 6b 41 4a 72 00 65 |2fro@m diskAJr.e|
000001b0 73 6f 6c 76 65 20 68 18 61 72 64 02 0a 84 75 2e |solve h.ard...u.|
000001c0 20 48 01 4c 04 20 6f 63 63 75 72 20 00 77 68 65 | H.L. occur .whe|
000001d0 6e 20 61 20 70 a0 72 6f 63 65 73 c1 87 66 80 3d |n a p.roces..f.=|
000001e0 0f 41 12 c0 04 40 4b 41 73 76 69 72 74 c9 40 7e |.A...@KAsvirt.@~|
000001f0 20 6d c3 70 74 68 40 22 00 26 24 6e 6f c1 95 20 | m.pth@".&$no.. |
00000200 69 00 a0 77 6f 02 72 c0 a4 67 20 73 65 74 20 e1 |i..wo.r..g set .|
00000210 80 a9 65 6c 73 65 c0 15 00 78 00 93 50 70 68 79 |..else...x..Pphy|
00000220 73 00 88 6c 84 0f 2c 81 c2 88 6d 75 73 74 20 62 |s..l..,...must b|
00000230 00 36 c0 65 74 72 69 65 76 00 88 46 31 58 2e 20 |.6.etriev..F1X. |
00000240 57 84 24 82 1f 73 03 30 65 ec 64 2c 42 81 c3 b7 |W.$..s.0e.d,B...|
00000250 20 41 0d 02 af 42 40 12 6d 40 c1 69 70 c0 ab 63 | A...B@.m@.ip..c|
00000260 6f 6e 80 74 69 67 75 6f 75 73 04 49 0d 40 60 6f |on.tiguous.I.@`o|
00000270 c6 2d 40 02 61 78 69 6d 04 69 7a 83 87 62 65 6e |.-@.axim.iz..ben|
00000280 65 66 9e 69 00 2c 03 d5 42 12 46 c6 2e 20 e0 a8 |ef.i.,..B.F.. ..|
00000290 af 46 71 c1 4b 01 e6 c6 b2 20 09 b3 20 ca b2 1f |.Fq.K.... .. ...|
000002a0 e8 a8 49 2f e9 55 4c 15 c4 46 4f 75 74 63 40 61 |..I/.UL..FOutc@a|
000002b0 a8 56 16 00 8c 80 54 ae 56 bb b0 21 00 00 c3 60 |.V....T.V..!...`|
000002c0 00 a2 56 d0 00 01 fa e2 85 01 ef 00 01 b9 56 23 |..V...........V#|
000002d0 0c 81 6d d2 56 83 c7 03 fd 56 77 72 69 74 74 e0 |..m.V....Vwritt.|
000002e0 4e 03 40 7b 05 57 66 72 65 65 20 75 80 70 20 73 |N.@{.Wfree u.p s|
000002f0 70 61 63 65 a1 4e 39 ec 46 2e 20 e3 82 c1 5e c5 |pace.N9.F. ...^.|
00000300 07 62 61 a6 63 82 5e 62 08 6f 6e 40 93 69 62 39 |.ba.c.^b.on@.ib9|
00000310 83 a0 92 60 4f 63 68 61 6e 67 20 4b 0d 71 50 73 |...`Ochang K.qPs|
00000320 62 8a e3 04 6c 69 6b 65 03 40 07 60 11 68 6f 6c |b...like.@.`.hol|
00000330 64 20 64 30 61 74 61 2c 20 98 20 aa 6f 64 00 65 |d d0ata, . .od.e|
--snip---
$ hexdump -C 4_System_0x81008000_0x0_lznt1__UncompressedBuffer.bin
00000000 69 63 61 74 6f 72 20 6f 66 20 74 68 65 20 6b 69 |icator of the ki|
00000010 6e 64 73 20 6f 66 20 66 61 75 6c 74 73 20 74 68 |nds of faults th|
00000020 61 74 20 63 61 75 73 65 20 73 79 73 74 65 6d 2d |at cause system-|
00000030 77 69 64 65 20 64 65 6c 61 79 73 2e 20 49 74 20 |wide delays. It |
00000040 69 6e 63 6c 75 64 65 73 20 72 65 61 64 20 6f 70 |includes read op|
00000050 65 72 61 74 69 6f 6e 73 20 74 6f 20 73 61 74 69 |erations to sati|
00000060 73 66 79 20 66 61 75 6c 74 73 20 69 6e 20 74 68 |sfy faults in th|
00000070 65 20 66 69 6c 65 20 73 79 73 74 65 6d 20 63 61 |e file system ca|
00000080 63 68 65 20 28 75 73 75 61 6c 6c 79 20 72 65 71 |che (usually req|
00000090 75 65 73 74 65 64 20 62 79 20 61 70 70 6c 69 63 |uested by applic|
000000a0 61 74 69 6f 6e 73 29 20 61 6e 64 20 69 6e 20 6e |ations) and in n|
000000b0 6f 6e 2d 63 61 63 68 65 64 20 6d 61 70 70 65 64 |on-cached mapped|
000000c0 20 6d 65 6d 6f 72 79 20 66 69 6c 65 73 2e 20 43 | memory files. C|
000000d0 6f 6d 70 61 72 65 20 74 68 65 20 76 61 6c 75 65 |ompare the value|
000000e0 20 6f 66 20 4d 65 6d 6f 72 79 5c 5c 50 61 67 65 | of Memory\\Page|
000000f0 73 20 52 65 61 64 73 2f 73 65 63 20 74 6f 20 74 |s Reads/sec to t|
00000100 68 65 20 76 61 6c 75 65 20 6f 66 20 4d 65 6d 6f |he value of Memo|
00000110 72 79 5c 5c 50 61 67 65 73 20 49 6e 70 75 74 2f |ry\\Pages Input/|
00000120 73 65 63 20 74 6f 20 64 65 74 65 72 6d 69 6e 65 |sec to determine|
00000130 20 74 68 65 20 61 76 65 72 61 67 65 20 6e 75 6d | the average num|
00000140 62 65 72 20 6f 66 20 70 61 67 65 73 20 72 65 61 |ber of pages rea|
00000150 64 20 64 75 72 69 6e 67 20 65 61 63 68 20 6f 70 |d during each op|
00000160 65 72 61 74 69 6f 6e 2e 00 00 50 61 67 65 73 49 |eration...PagesI|
00000170 6e 70 75 74 50 65 72 73 65 63 00 13 00 00 00 15 |nputPersec......|
00000180 00 88 00 00 00 03 00 00 00 2b 00 00 00 0a 00 00 |.........+......|
00000190 80 03 08 00 00 00 05 1f 00 00 0d 1f 00 00 81 08 |................|
000001a0 00 00 00 1a 1f 00 00 2b 1f 00 00 81 08 00 00 00 |.......+........|
000001b0 38 1f 00 00 00 75 69 6e 74 33 32 00 00 44 69 73 |8....uint32..Dis|
000001c0 70 6c 61 79 4e 61 6d 65 00 00 50 61 67 65 73 20 |playName..Pages |
000001d0 49 6e 70 75 74 2f 73 65 63 00 00 44 65 73 63 72 |Input/sec..Descr|
000001e0 69 70 74 69 6f 6e 00 00 50 61 67 65 73 20 49 6e |iption..Pages In|
000001f0 70 75 74 2f 73 65 63 20 69 73 20 74 68 65 20 72 |put/sec is the r|
00000200 61 74 65 20 61 74 20 77 68 69 63 68 20 70 61 67 |ate at which pag|
00000210 65 73 20 61 72 65 20 72 65 61 64 20 66 72 6f 6d |es are read from|
00000220 20 64 69 73 6b 20 74 6f 20 72 65 73 6f 6c 76 65 | disk to resolve|
00000230 20 68 61 72 64 20 70 61 67 65 20 66 61 75 6c 74 | hard page fault|
00000240 73 2e 20 48 61 72 64 20 70 61 67 65 20 66 61 75 |s. Hard page fau|
00000250 6c 74 73 20 6f 63 63 75 72 20 77 68 65 6e 20 61 |lts occur when a|
00000260 20 70 72 6f 63 65 73 73 20 72 65 66 65 72 73 20 | process refers |
00000270 74 6f 20 61 20 70 61 67 65 20 69 6e 20 76 69 72 |to a page in vir|
00000280 74 75 61 6c 20 6d 65 6d 6f 72 79 20 74 68 61 74 |tual memory that|
00000290 20 69 73 20 6e 6f 74 20 69 6e 20 69 74 73 20 77 | is not in its w|
000002a0 6f 72 6b 69 6e 67 20 73 65 74 20 6f 72 20 65 6c |orking set or el|
000002b0 73 65 77 68 65 72 65 20 69 6e 20 70 68 79 73 69 |sewhere in physi|
000002c0 63 61 6c 20 6d 65 6d 6f 72 79 2c 20 61 6e 64 20 |cal memory, and |
000002d0 6d 75 73 74 20 62 65 20 72 65 74 72 69 65 76 65 |must be retrieve|
000002e0 64 20 66 72 6f 6d 20 64 69 73 6b 2e 20 57 68 65 |d from disk. Whe|
000002f0 6e 20 61 20 70 61 67 65 20 69 73 20 66 61 75 6c |n a page is faul|
00000300 74 65 64 2c 20 74 68 65 20 73 79 73 74 65 6d 20 |ted, the system |
00000310 74 72 69 65 73 20 74 6f 20 72 65 61 64 20 6d 75 |tries to read mu|
00000320 6c 74 69 70 6c 65 20 63 6f 6e 74 69 67 75 6f 75 |ltiple contiguou|
00000330 73 20 70 61 67 65 73 20 69 6e 74 6f 20 6d 65 6d |s pages into mem|
--snip--

TODO: Xpress and XpressH decompression

EOF
-jamaal


