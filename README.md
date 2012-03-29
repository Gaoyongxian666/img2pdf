img2pdf
=======

Lossless conversion of images to PDF without unnecessarily re-encoding JPEG and
JPEG2000 files. Thus, no loss of quality and no unnecessary large output file.

background
----------

PDF is able to embed JPEG and JPEG2000 images as they are without re-encoding
them (and hence loosing quality) but I was missing a tool to do this
automatically, thus I wrote this piece of python code.

If you know how to embed JPEG and JPEG2000 images into a PDF container without
recompression, using existing tools, please contact me so that I can put this
code into the garbage bin :D

functionality
-------------

The program will take image filenames from commandline arguments and output a
PDF file with them embedded into it. If the input image is a JPEG or JPEG2000
file, it will be included as-is without any processing. If it is in any other
format, the image will be included as zip-encoded RGB. As a result, this tool
will be able to lossless wrap any image into a PDF container while performing
better (in terms of quality/filesize ratio) than existing tools in case the
input image is a JPEG or JPEG2000 file.

For the record, the imagemagick command to lossless convert any image to
PDF using zip-encoding, is:

	convert input.jpg -compress Zip output.pdf

The downside is, that using imagemagick like this will make the resulting PDF
files a few times bigger than the input JPEG or JPEG2000 file and can also not
output a multipage PDF.

img2pdf is able to output a PDF with multiple pages if more than one input
image is given, losslessly embed JPEG and JPEG2000 files into a PDF container
without adding more overhead than the PDF structure itself and will save all
other graphics formats using lossless zip-compression.

Another nifty advantage: Since no re-encoding is done in case of JPEG images,
the conversion is many (ten to hundred) times faster with img2pdf compared to
imagemagick. While a run of above convert command with a 2.8MB JPEG takes 27
seconds (on average) on my machine, conversion using img2pdf takes just a
fraction of a second.

commandline arguments
---------------------

At least one input file argument must be given as img2pdf needs to seek in the
file descriptor which would not be possible for stdin.

Specify the dpi with the -d or --dpi options instead of reading it from the
image or falling back to 96.0.

Specify the output file with -o or --output. By default output will be done to
stdout.

Specify metadata using the --title, --author, --creator, --producer,
--creationdate, --moddate, --subject and --keywords options (or their short
forms).

More help is available with the -h or --help option.

bugs
----

If you find a JPEG or JPEG2000 file that, when embedded can not be read by the
Adobe Acrobat Reader, please contact me.

For lossless conversion of other formats than JPEG or JPEG2000 files, zip/flate
encoding is used.  This choice is based on a number of tests I did on images.
I converted them into PDF using imagemagick and all compressions it has to
offer and then compared the output size of the lossless variants. In all my
tests, zip/flate encoding performed best. You can verify my findings using the
test_comp.sh script with any input image given as a commandline argument. If
you find an input file that is outperformed by another lossless compression,
contact me.
