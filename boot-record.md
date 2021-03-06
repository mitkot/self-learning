# boot record analysis tips
```
< rhel6-cust.img: customer's boot-failed instance
> rhel6-150.img: rhel6 RHEL-6.9_HVM-20171024-x86_64-2-Hourly2-GP2 (ami-4ca80e2a)
```

```
$ hexdump -C ./rhel6-cust.img|head -n 128 > rhel6-cust-2048.img
$ hexdump -C ./rhel6-150.img |head -n 128 > rhel6-150-2048.img

$ diff rhel6-cust-2048.img rhel6-150-2048.img

46,49c46,49
< 00100400 00 00 96 00 fb fe 57 02 bc fd 1d 00 4f 7a 21 02 |......W.....Oz!.|
< 00100410 1e af 94 00 00 00 00 00 02 00 00 00 02 00 00 00 |................|
< 00100420 00 80 00 00 00 80 00 00 00 20 00 00 5a 95 cd 5b |......... ..Z..[|
< 00100430 18 9f cd 5b a6 00 ff ff 53 ef 01 00 01 00 00 00 |...[....S.......|
---
> 00100400 00 00 96 00 fb fe 57 02 26 fe 1d 00 fb 64 47 02 |......W.&....dG.|
> 00100410 09 23 95 00 00 00 00 00 02 00 00 00 02 00 00 00 |.#..............|
> 00100420 00 80 00 00 00 80 00 00 00 20 00 00 37 84 d1 5b |......... ..7..[|
> 00100430 67 8e ee 59 02 00 ff ff 53 ef 01 00 01 00 00 00 |g..Y....S.......|
54,58c54,58
< 00100480 00 00 00 00 00 00 00 00 2f 6d 6e 74 00 77 32 32 |......../mnt.w22|
< 00100490 32 65 30 5f 73 79 73 00 78 3c 56 e2 3f 88 ff ff |2e0_sys.x<V.?...|
< 001004a0 d5 59 b8 23 00 00 00 00 18 3c 56 e2 3f 88 ff ff |.Y.#.....<V.?...|
< 001004b0 38 3c 56 e2 3f 88 ff ff 88 d7 d8 bc 1f 88 ff ff |8<V.?...........|
< 001004c0 80 04 5a e0 1f 88 ff ff 00 00 00 00 00 00 76 01 |..Z...........v.|
---
> 00100480 00 00 00 00 00 00 00 00 2f 00 68 fc 21 04 04 88 |......../.h.!...|
> 00100490 ff ff 36 78 7c 98 00 00 00 00 c0 76 74 03 04 88 |..6x|......vt...|
> 001004a0 ff ff c0 76 74 03 04 88 ff ff 80 fe e3 02 04 88 |...vt...........|
> 001004b0 ff ff 80 b6 80 06 04 88 ff ff 00 5c 09 a0 ff ff |...........\....|
> 001004c0 ff ff e8 34 aa 06 04 88 00 00 00 00 00 00 76 01 |...4..........v.|
69c69
< 00100570 00 00 00 00 04 00 00 00 e5 5a 30 09 00 00 00 00 |.........Z0.....|
---
> 00100570 00 00 00 00 04 00 00 00 35 62 52 00 00 00 00 00 |........5bR.....|
72,128c72,128
```


# Write binary with dd
## 00100400
```
$ printf "%d\n" 0x100400
1049600

$ echo -en '\x00\x00\x96\x00\xfb\xfe\x57\x02\x26\xfe\x1d\x00\xfb\x64\x47\x02\x09\x23\x95\x00\x00\x00\x00\x00\x02\x00\x00\x00\x02\x00\x00\x00\x00\x80\x00\x00\x00\x80\x00\x00\x00\x20\x00\x00\x37\x84\xd1\x5b\x67\x8e\xee\x59\x02\x00\xff\xff\x53\xef\x01\x00\x01\x00\x00\x00' |dd of=/dev/xvdf bs=1 seek=1049600 conv=notrunc
```

## 00100480
```
$ printf "%d\n" 0x100480
1049728

$ echo -en '\x00\x00\x00\x00\x00\x00\x00\x00\x2f\x00\x68\xfc\x21\x04\x04\x88\xff\xff\x36\x78\x7c\x98\x00\x00\x00\x00\xc0\x76\x74\x03\x04\x88\xff\xff\xc0\x76\x74\x03\x04\x88\xff\xff\x80\xfe\xe3\x02\x04\x88\xff\xff\x80\xb6\x80\x06\x04\x88\xff\xff\x00\x5c\x09\xa0\xff\xff\xff\xff\xe8\x34\xaa\x06\x04\x88\x00\x00\x00\x00\x00\x00\x76\x01'|dd of=/dev/xvdf bs=1 seek=1049728 conv=notrunc
```

## 00100570
```
$ printf "%d\n" 0x00100570
1049968

$ echo -en '\x00\x00\x00\x00\x04\x00\x00\x00\x35\x62\x52\x00\x00\x00\x00\x00'|dd of=/dev/xvdf bs=1 seek=1049968 conv=notrunc
```