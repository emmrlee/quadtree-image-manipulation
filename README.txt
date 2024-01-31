Spring 2023 - CS151
Names: Emma Lee and Rachel Nguyen
How to compile:
How to run it: Use the command java Main with additional flags listed below
	-i <filename> the input file to be processed (MUST BE OF .ppm TYPE)
	-o <filename> indicates the root name of the output file that your program should write to 
		For example, -o out would write to “out-1.ppm”, “out-2.ppm”, …, “out-8.ppm”
	-c indicates that you should perform image compression
	-e indicates that you should perform edge detection
	-x for running our own random neighbor filter
	-t indicates that output images should have the quadtree outlined
	
	We assume that only one of -c, -e or -x will be given. However, -t may or may not be present on any filter. 
	Please see the known bugs and limitations for further explanation on what -t is expected to show.

Known bugs and limitations:
	In the Image class, we have 2 constants that keep track of how many header lines the given ppm file has
	as well as the location of the line that gives us the image size information. We noticed some of the test
	ppms have 3 header lines with the image size info given on the 2nd line (index 1) while some have 4 header
	lines with the image size info given on the 3rd line (index 2). Please change the constants if you want to
	test using ppms of different formats.

	We have two QuadTree division methods; one with a compressionLvl parameter and one without. 
		The -c calls the one with the parameter on all 8 compression levels. 
		The -e calls the one without, meaning it will always divide as much as it can until our threshold (5).

	When given the -c flag, the program should always write 8 files no matter what. 
	This means that if the given image cannot achieve a certain compression level, some of the output images will look the same. 
		For example, if a given image can only achieve up to compression level 0.2, then the output files 7 and 8 (for levels 0.5 and 0.75) 
		will look the same. When an image cannot be compressed to a certain level, the QuadTree compression will subdivide into nodes as small 
		as possible according to our given threshold (5).

	So basically, our QuadTree division will stop dividing once it reaches a given compression level or once it can no longer divide based on our threshold.

	*Please note that this means when -e is called alongside -t, the outlines of the QuadTree (in red) will likely take up most of image 
	(especially if the image has a lot of contrast like glass.ppm or brick.ppm). This is the expected behavior.

	Runtime information:
		At least on our machines, we noticed that images of around 512 x 512 - 833 x 833 (like kira or tokyo images) take about 16 minutes to write all 8 outputs.
		We noticed that these images are not able to reach the last few compression levels, so towards the 7th - 8th output, the QuadTree division will stop only 
		after it has divided as much as possible according to our threshold. Each time the QuadTree divides all the way down it takes about 8 minutes.
			So for kira-bunny.ppm, the first 6 outputs go by quickly, but the last 2 take 8 minutes each to write (leading to a total of 16 mins).

Discussion:
	Extra credit things to note:
		Our own filter:
			We create a random neighbor filter. The algorithm of this function is taking one pixel, picking one of its 8 neighbors randomly, 
			and setting itself to the color of its randomly chosen neighbor. If it is an edge pixel, leave the color as itself.

		Resiz:
			Our program should still work when given a rectangular image or an image that is not a power of 2 size due to our resize method 
			(that should be applied to any given image regardless (even if it is a square of power of 2)).
