# RegEX: The Quicker Parser Upper
### or "How I Went from [A](https://github.com/rhymeswithfloral/hist5702w_project/blob/master/Laurel/wget_weme/wget_brimstone/1645_brimstone.html) to [B](https://github.com/rhymeswithfloral/hist5702w_project/blob/master/Laurel/wget_weme/wget_brimstone/1645_brimstone_copy.csv)"
_March 12, 2016_

The regular expressions that I have included in this are actually pulled from the third worksheet I created to do the exact same job because as I was working I realized there were quicker and easier way to do earlier tasks. I was working on creating a tutorial for cleaning and transforming HTML files downloaded with Wget into CSV files, I wanted to make sure I was providing the best possible methods and that the order of steps made sense. So I would start over and create a new worksheet to track the expressions I used.

#### We must be clean if we want to clean!

First I needed to find all the HTML tags. The funny thing is I was attempting a whole bunch of real complex regular expressions before I realized I was just making things harder than they needed to be. Sometimes there are **too many** ways to skin a cat.

_Find all HTML tags_

	FIND:
		<.+?>
	REPLACE:
		*leave blank*

_Find all instances of `&nbsp;`_

	FIND:
		`&nbsp;`
	REPLACE:
		*leave blank*

Once this was done there was still some CSS coding around the top of the file, which was easy to get rid of manually. There may be some HTML or CSS coding leftover near the top/beginning of the text file, this can easily be removed manually _(Lines 5-50)_.

For this particular set of data the webpage had already organized the data into rows and columns with HTML. So I just needed to make is CSV ready. _Gosh I love it when acronyms tell you exactly what you need to know (CSV stands for Comma Separated Values)._ The column names were located within the document, just below the line which reads "List of all Event assertions around a specific date." This would be line 64 for if the above step has not been taken yet or line 24 if it has.

`ID,short description,date,city,parish,current county,old county,nation`

Before organizing the data into _comma separated values_ I need to get rid of the commas which would cause problems for me. The first step I took was to change the format of publication information from `London: 1645, 7` to `London: 1645 p.7`.

	FIND: 
		([0-9]{4}), ([0-9]{1,})
	REPLACE:
		\1 p.\2

Then I removed all commas with:

	FIND:
		,
	REPLACE: 
		*leave blank*`

Since there were instances where the source information didn't include page numbers I decided that putting the source publication information between brackets will keep them from getting mixed up in our later expressions. However, this was not as easy as I had thought it would be. Each time I put something in brackets, I realized my expression was catching everything. So I ended up using a few different ones. Most of these I had first attempted with `164\d` but then around the end of my second cleaning I noticed that I had missed a few from different decades and I switched it `16\d\d`

	FIND:
		(London: 16\d\d p\..+)\n
		(London: 16\d\d)\n
		(London: 16\d\d Cover .+)\n
		(London: 16\d\d Cover)\n
		(London: [0-9]{4} p\.[0-9]{1,}$)
	REPLACE:
		(\1)

That was one of the more difficult moments, though as I type this I am realizing that there was an easier way to do it. You see, I found it difficult because I thought I needed to be careful about the order in which I used these expressions -- so as not to have single brackets around a few, double brackets around some, and triple backets around others. What I just realized was that I could have just done them in whatever order and then gone back with something like `\(\(.+\)\)`

_Remove all doubles spaces at beginning of lines._

	FIND:
		^(  )
	REPLACE: 
		*leave blank*

Once the file was all prepped and ready to collected into _comma separated values_ I decided to focus first on the date and locations columns. This is another area where it wasn't until the second cleaning attempt that I realized I was making a few big mistakes. I had forgotten that some of the cells in the location columns were blank and I removed blank lines and whitespace much earlier on so that if I had kept going and opened it up in a spreadsheet there would have been several rows that were not properly organized. On my final attempt I made sure to refer back to the original html page that I downloaded, so that I could doublecheck where the blank cells were.

#### The joining begins!

I started with nation and worked backwards because I knew that nation would always be England and that there were now blank cells in the _current county_ or _old county_ columns for any of the rows.

	FIND:
		(\s)+(England)
	REPLACE:
		,\2

There were several rows that were missing both the city and the parish information. By adding , , I am accounting for that blank cell and ensuring that our information remain in the correct columns. This needs to be done for each of the location columns. I used these regular expressions in order to retain the empty cells because, as I mentioned, they function as a placeholder that maintains the organization of the other columns.

	FIND:
		(1645)\n\s\s\n
	REPLACE:
		\1, , ,

![](https://github.com/rhymeswithfloral/hist5702w_project/blob/master/Laurel/wget_weme/wget_brimstone/wget_regexclean19.png)

The result should look like:
					line 1314:	1645, , ,Suffolk
					line 1315:	Suffolke,England

This next expression takes into account any rows which had a blank cell in the city column.

	FIND:
		(1645)\n\s
	REPLACE:
		\1, ,

The result should look like:
					line 183:  1645, , St. Osyth; St. Ofes; St. Oses
					line 184:  Essex
					line 185:  Essex,England

Lastly there were results that did not contain a parish.

	FIND:
		(1645)\n(\w.+)\n\s\n(\w.+)
	REPLACE:
		\1,\2,,\3

![](https://github.com/rhymeswithfloral/hist5702w_project/blob/master/Laurel/wget_weme/wget_brimstone/wget_regexclean21.png)

The result should look like:
			line 154:  1645,Ramsey,,Essex
			line 155:  Essex,England

After completing the above steps, I had managed to join together nearly all the columns contain location information onto one line. I just needed to join or two more lines together before I had more than half the _*values*_ for each result _*separated*_ by _*commas*_. For the final step I managed to create an expression that could be used twice (this kind of blew my mind, I'm not sure why).

	FIND:
		(1645,.+)\n(.+\w)
	REPLACE:
		\1,\2

| Line | Before | After Once | After Twice|
|------|--------|-------|------------|
|line 54:| 1645,Ramsey,,Essex| 1645,Ramsey,,Essex,Essex,England| N/A|
|line 55:| Essex,England| ---| N/A|
|line 83:| 1645, , St. Osyth; St. Ofes; St. Oses| 1645, , St. Osyth; St. Ofes; St. Oses,Essex| 1645, , St. Osyth; St. Ofes; St. Oses,Essex,Essex,England|
|line 184:| Essex| Essex,England| ---|
|line 185:| Essex,England|  ---| ---|

_I was, still am, pretty sure that creating a regular expression that I could use twice without fail means that I won something._

From the get go I have been using 1645 as an anchor, of sorts, for all of my regular expression. Using 1645 and England as "anchors" has been really useful way of keeping track of which/what/where my regular expressions need focus on. This is something I have had a little bit of difficulty when it comes to using regular expressions, keeping track of what it is I am trying to clean, because every manipulation can change the location of a line or the format of the whole file. It can be easy to get lost. Now that I had combined those into one line, I needed to work on the rows which had data in all 5 location columns. Though I had already started to bring them together when I removed the extra row between old county and nation.

This is an example of a result with all 5 columns filled. I am proud to say that as you can see, my previous expression has succesffuly managed to leave it un-~~fucked~~-scathed.

		line 1890: 1645
		line 1891: Manningtree
		line 1892:  Manningtree
		line 1893: Essex
		line 1894: Essex, England

Now I just need to continue to collect them together, continuing to use the date as an anchor of sorts. ~~Some of the sources which I was formatting earlier did not have page numbers therefore the line ended with just a date and accidentally got mixed up with our regex patterns~~ This is where I realized that I needed to be more careful about formatting the source and publication information. However, it turned out that during my final attempt I still missed something when I was trying to make sure all the source/publication information were between brackets. I didn't discover it until I had opened up the CSV in a spreadsheet. But I digress...

I used this expression to bring together the date and location data for the results with 5 filled location columns.
	FIND:
		(1645)\n(.+\w)\n (.+\w)\n(.+\w)\n(.+\w)
	REPLACE:
		\1,\2,\3,\4,\5


_Remove all blank lines_
FIND:
`^\n`
REPLACE: `*leave blank*`. At this point I felt there were not problem were made from removing all the empty lines from the file. So my next step was to join the date and location lines to the end of the source and publication line, making sure that a _comma_ was _separating_ the source and date _values_.

	FIND:
		\s\n(1645.+)
	REPLACE:
		,\1

The next step deals with the source line and the lines which contain "Appears in:". This became my next anchor, or point of focus, for my regular expressions. I need to have the description line above it as well as the source and publication information line from below it, join "Appears in:", which _**appears in**_ its own line. This expression manage to _kill to birds with one stone_.

	FIND:
		\n(\sAppears in:)\n
	REPLACE:
		\1

![](https://github.com/rhymeswithfloral/hist5702w_project/blob/master/Laurel/wget_weme/wget_brimstone/wget_regexclean25.png)

The final step is joining the Row ID numbers. With everything else joined together I am able to use a regular expression to locate the ID number without it also recognizing the date. This is an issue I was having in my first attempt at cleaning the file.

_Find each line that contains only digits, the ID numbers, and join the line with the description line separating them with a comma._

	FIND:
		^([0-9]{1,})\n
	REPLACE:
		\1,


### ~~Almost~~ Squeaky clean!

![](https://github.com/rhymeswithfloral/hist5702w_project/blob/master/Laurel/wget_weme/wget_brimstone/wget_regexclean27.png)

Note the multiple copies that had been made. Well actually! Note the other CSV file in the folder. I had gone through, cleaned and parsed and turned this webpage into a CSV file before. I had not keep track of what I had done and I hadn't taken any screen caps of my process. Nor was the final product, the CSV file, as well put together as it was this time. There is value ~~comma separated value~~ in going back trying again! But maybe there is a need to be quite so neurotic about it as I was. But I digress!

![](https://github.com/rhymeswithfloral/hist5702w_project/blob/master/Laurel/wget_weme/wget_brimstone/wget_regexclean31.png)

When I opened up the CSV file I was fairly certain that I would find some mistakes. However, viewing the CSV as spreadsheet makes it much easier to locate mistakes and return to text editor to resolve them.

Now I've got a worksheet, a clear process and sequence, and a beginning and end result I can spend another 5 hours writing a tutorial.

~~This project has given me a stress rash that is slowly spreading up my arms.~~
