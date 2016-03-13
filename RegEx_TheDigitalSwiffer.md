# RegEX: The Quicker Parser Upper
### or "How I Went from [A]() to [B]()"
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
		&nbsp;
		REPLACE:
		*leave blank*

Once this was done there was still some CSS coding around the top of the file, which was easy to get rid of manually. There may be some HTML or CSS coding leftover near the top/beginning of the text file, this can easily be removed manually _(Lines 5-50)_.

For this particular set of data the webpage had already organized the data into rows and columns with HTML. So I just needed to make is CSV ready. _Gosh I love it when acronyms tell you exactly what you need to know (CSV stands for Comma Separated Values)._ The column names were located within the document, just below the line which reads "List of all Event assertions around a specific date." This would be line 64 for if the above step has not been taken yet or line 24 if it has.

Before organizing the data into _comma separated values_ I need to get rid of the commas which would cause problems for me. The first step I took was to change the format of publication information from `London: 1645, 7` to `London: 1645 p.7`.

	FIND: ([0-9]{4}), ([0-9]{1,})
	REPLACE: \1 p.\2

Then I removed all commas with FIND:`,` REPLACE: `*leave blank*`

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
		REPLACE: *leave blank*

Once the file was all prepped and ready to collected into _comma separated values_ I decided to focus first on the date and locations columns. This is another area where it wasn't until the second cleaning attempt that I realized I was making a few big mistakes. I had forgotten that some of the cells in the location columns were blank and I removed blank lines and whitespace much earlier on so that if I had kept going and opened it up in a spreadsheet there would have been several rows that were not properly organized. On my final attempt I made sure to refer back to the original html page that I downloaded, so that I could doublecheck where the blank cells were.

#### The joining begins!

I started with nation and worked backwards.
		FIND:
		(\s)+(England)
		REPLACE:
		,\2

There are severals rows that are missing both the city and the parish information. Using this expression will take into account those blank cells.
FIND:
(1645)\n\s\s\n
REPLACE:
\1, , ,

				#The result should look like:
					line 1314	1645, , ,Suffolk
					line 1315	Suffolke,England

This next expression takes into account any rows which had blank cells in the city column. By adding , , I am accounting for that blank cell and ensuring that our information remain in the correct columns. This needs to be done for each of the location columns.
FIND:
(1645)\n\s
REPLACE:
\1, ,
				#The result should look like:
					line 183  1645, , St. Osyth; St. Ofes; St. Oses
					line 184  Essex
					line 185  Essex,England

Lastly there are results that do not contain parish or old county.
FIND:
(1645)\n(\w.+)\n\s\n(\w.+)
REPLACE:
\1,\2,,\3

	#The result should look like:
			line 154  1645,Ramsey,,Essex
			line 155  Essex,England

The above results you'll want to use this expression to join them all onto one line.
FIND:
(1645,.+)\n(.+\w)
REPLACE:
\1,\2

	#If they still aren't all on one line, just repeat the exact same expression.

For results that have data in all 5 location columns, I have already begun to collect them together when I removed the extra space between the country and the old county columns. For example:
		line 1890 1645
		line 1891 Manningtree
		line 1892  Manningtree
		line 1893 Essex
		line 1894 Essex, England
	#Now I just need to continue to collect them together, continuing to use the date as an anchor of sorts. BUT! Before I do this there are a couple of things mistakes that need to be cleaned up. Some of the sources which I was formatting earlier did not have page numbers therefore the line ended with just a date and accidentally got mixed up with our regex patterns
FIND:
(1645)\n(.+\w)\n (.+\w)\n(.+\w)\n(.+\w)
REPLACE:
\1,\2,\3,\4,\5

Remove all blank lines
FIND:
^\n
REPLACE: *leave blank*

Join the date and location lines to the source line, separating it with a comma. I chose to get rid of some of the extra space there as well.
FIND:
\s\n(1645.+)
REPLACE:
,\1

	#The next step deals with the source line and the lines which contain "Appears in:"

Find each line that begins with "Appears in" followed by a line containing source information and format so that it joins the description line above it and the line with source information is joined to it.
FIND: \n(\sAppears in:)\n
REPLACE: \1

	#The final step is joining the Row ID numbers.
	#Now I am able to use a regular expression to locate the ID number without it also recognizing the date. {#} looks for a pattern containing a number with only a specific amount digits, but if I add a comma {#,} it tells it to look for either # or more.
Find each line that contains only digits, this are the ID numbers, and join the line with the description line separating them with a comma.
FIND: ^([0-9]{1,})\n
REPLACE: \1,


Join the date 1645, to the description description line with a comma separating the description value and the data value.
FIND:\n\s\n(1645,)
REPLACE:,\1
