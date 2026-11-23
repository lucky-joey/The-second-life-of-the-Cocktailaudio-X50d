# The Second Life of the CocktailAudio X50D

The high-end **CocktailAudio X50D** is to receive a second life following the insolvency of Novatron.

The hardware will first be comprehensively analyzed. Based on these findings, a dedicated Linux system will be developed for the device. The system is intended to be maintained and supported in the long term.

## Front View

[![CocktailAudio X50D - Front View](https://www.hifistatement.net/images/content/tests/18-10-22_cocktailaudio/intro.jpg)](https://www.hifistatement.net/tests/item/2380-cocktailaudio-x50d)

## Rear View

The rear view showing the connections is documented on page 28 of the [official German user manual](https://cocktailaudio.de/files/cocktailaudio/bedienungsanleitung/X50D_Handbuch_Deutsch_ver1.0.pdf).

### Image and Source Information

- **Product image:** [HIFISTATEMENT review, “CocktailAudio X50D”](https://www.hifistatement.net/tests/item/2380-cocktailaudio-x50d), author: Wolfgang Kemper.
- **Image license status:** The source does not specify a free license for this image. It is therefore not presented as freely reusable; use outside this project should require the rights holder’s permission.
- **User manual:** [CocktailAudio X50D manual, German edition](https://cocktailaudio.de/files/cocktailaudio/bedienungsanleitung/X50D_Handbuch_Deutsch_ver1.0.pdf). Rights to the product images and trademarks belong to their respective owners.

## Analysis of the Existing Update File

The existing update package can be extracted for analysis with:

```text
binwalk -e X50D-CA-1.0.0.r1721.pkg

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
224           0xE0            PNG image, 1024 x 8, 8-bit/color RGBA, non-interlaced
309           0x135           Zlib compressed data, best compression
2981          0xBA5           Zlib compressed data, best compression
3136          0xC40           PNG image, 1024 x 70, 8-bit/color RGBA, non-interlaced
3221          0xC95           Zlib compressed data, best compression
5893          0x1705          Zlib compressed data, best compression
6480          0x1950          PNG image, 1024 x 8, 8-bit/color RGBA, non-interlaced
6565          0x19A5          Zlib compressed data, best compression
9237          0x2415          Zlib compressed data, best compression
9440          0x24E0          PNG image, 46 x 46, 8-bit/color RGBA, non-interlaced
9525          0x2535          Zlib compressed data, best compression
12197         0x2FA5          Zlib compressed data, best compression
15600         0x3CF0          gzip compressed data, from Unix, last modified: 2021-11-09 10:22:42
7128096       0x6CC420        ELF, 32-bit LSB executable, ARM, version 1 (SYSV)
7251708       0x6EA6FC        Unix path: /home/root/bin/images5
7251738       0x6EA71A        Unix path: /home/root/bin/images6
7251920       0x6EA7D0        gzip compressed data, from Unix, last modified: 2021-11-09 10:20:29
66094678      0x3F08656       MySQL MISAM compressed data file Version 8
75308115      0x47D1C53       MySQL MISAM index file Version 10
203565011     0xC2227D3       PNG image, 512 x 512, 8-bit/color RGB, non-interlaced
203565105     0xC222831       Zlib compressed data, best compression
203972291     0xC285EC3       PNG image, 512 x 512, 8-bit/color RGB, non-interlaced
203972385     0xC285F21       Zlib compressed data, best compression
204303962     0xC2D6E5A       PNG image, 512 x 512, 8-bit/color RGB, non-interlaced
204304074     0xC2D6ECA       Zlib compressed data, best compression
204431379     0xC2F6013       PNG image, 512 x 512, 8-bit/color RGB, non-interlaced
204431473     0xC2F6071       Zlib compressed data, best compression
204793494     0xC34E696       PNG image, 512 x 512, 8-bit/color RGBA, non-interlaced
205160835     0xC3A8183       PNG image, 512 x 512, 8-bit/color RGBA, non-interlaced
205410169     0xC3E4F79       PNG image, 512 x 512, 8-bit/color RGBA, non-interlaced
205410327     0xC3E5017       Zlib compressed data, best compression
211245593     0xC975A19       JPEG image data, JFIF standard 1.01
211245623     0xC975A37       TIFF image data, big-endian, offset of first image directory: 8
211708648     0xC9E6AE8       PNG image, 206 x 206, 8-bit/color RGBA, non-interlaced
211708760     0xC9E6B58       Zlib compressed data, best compression
211793282     0xC9FB582       PNG image, 206 x 206, 8-bit/color RGBA, non-interlaced
211793394     0xC9FB5F2       Zlib compressed data, best compression
212029341     0xCA34F9D       PNG image, 118 x 104, 8-bit/color RGBA, non-interlaced
233017975     0xDE39277       IMG0 (VxWorks) header, size: 1314133057
284018626     0x10EDC7C2      MySQL ISAM compressed data file Version 6
296903319     0x11B26297      MySQL MISAM index file Version 7
351455108     0x14F2C784      device tree image (dtb)
```
