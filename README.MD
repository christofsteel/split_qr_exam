split_qr_exam
=============

This program splits a pdf composed of scanned exams. The input is a pdf file containing multiple
scanned exames. On each exam cover there should be a QR code identifying the student writing this
exam.
For each part of the exam a folder will be created and for each student a pdf will be created in
that folder to allow for parallel evaluation. To ensure pseudonymity the files are named with the
sha256 hash of the student id.

For this to work, you have to specify all the parts of the exam with the corresponding amount of
pages.

Install
=======

The program can be installed using pip

```
pip install split_qr_exam
```

This will also install all needed requirements, namely `pymupdf` and `python-opencv2-headless`.

Usage
=====

```
usage: split_qr_exam [-h] [-t THRESHOLD] [-s SHRINK] [-c CROP] [-T] [-S] [-C] -p PART [-d] [-b] pdf [dest]

Split a scanned examfile into folders for each student and tasks in seperate pdf files. The input PDF should have a QR code identifying the student on the cover of each exam.

positional arguments:
  pdf                   The pdf file to split
  dest                  Destination folder. Default is the current directory

optional arguments:
  -h, --help            show this help message and exit
  -t THRESHOLD, --threshold THRESHOLD
                        Set the threshold for the cleanup phase. Values range from 0 to 255. Lower value means more grey is detected as white, higher value means more grey is detected as black. Only impacts the QR detection phase. Default: 127
  -s SHRINK, --shrink SHRINK
                        Width of the image in pixel for the QR code detection phase. Lower value means more performance, higher value means more accuracy. Only impacts the QR detection phase, does not affect the output PDF. Default: 600
  -c CROP, --crop CROP  Fractional coordinates, that represent the area of the coverpage, where the QR code is located. The format is "x1,y1:x2,y2" where x1, x2, y1, y2 are values from 0 to 1. Default is "0,0.5:0.5,1" which corresponds to the top right
                        quartal of the image.
  -T, --no-cleanup      Skip cleanup phase
  -S, --no-shrink       Skip shrinking phase
  -C, --no-crop         Skip cropping phase
  -p PART, --part PART  Name and amount of pages for the parts of the exam, seperated by a colon. Can (and should) be issued multiple times (Example "-p Cover:1 -p Task1:2"). Should also include a part for the cover sheet, and empty bonus sheets. The
                        number of pages should not include the backsides, if the document is scanned in duplex. Use --is-double-sided instead
  -d, --is-double-sided
                        The exam is scanned in duplex. The split output pdf will also include the last backside of the previous part, since students tend to also write solutions there
  -b, --last-part-is-bonus
                        The last part are bonus sheets. There will be a seperate pdf for each bonus sheet.

Example of use:
        split_qr_exam -p cover:1 -p task1:2 -p task2:3 -p task3:1 -p bonus:3 -b -d scanned_exams.pdf ~/splitexams/
```
