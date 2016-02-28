# Tutorial Reflections
_February 26, 2016_

## OpenRefine

Last year when I used OpenRefine I was flying by the seat of my pants, so to speak. I didn't refer to the [Programming Historian tutorial ](http://programminghistorian.org/lessons/cleaning-data-with-openrefine) at all and was just picking up things based on trial and error and from what I was reading in forums like [Stack Overflow](http://stackoverflow.com/). So this time around I decided to, as best as I could, pretend as though OpenRefine and I weren't as intimately familiar as we are with one another. Because of this decision I am going to jump straight into my experience with the tutorial for OpenRefine, followed by my own opinions on the wonders of OpenRefine and how great of a tool it was when I used it last year from [a project about antiquities dealers.](https://hist4805digitaldealer.files.wordpress.com)

Before I say anything there is one thing I need to get off my chest... What is with the lack of screen shots?! The amount of text these tutorials could cut down on would be pretty substantial if they only included screen shots to follow along with the text. It would also help to simplify the tutorial for those who may not be well versed in the compu-lingo. Mind you, I have to hand it to the authors of this tutorial, they did a pretty good job of explaining the initial steps in writing.

![Spelling out the not-obvious](https://github.com/rhymeswithfloral/hist5702w_notebook/blob/master/OpenRefine/OR_createnew2.png?raw=true)

After you've created the new project, the tutorial's next section is called **Get to know your data**, while this a really important step before you start using OpenRefine, I don't think OpenRefine is the best place to "get to know your data," you're limited to the numbers of rows visible on the screen at a time and you can't adjust the way the information is displayed in the rows and columns. If I were working on a project of my own, I would have a much easier "getting to know my data" in a spreadsheet. Spreadsheet allows you to review all of your data on one page and provides you an opportunity to eliminate unnecessary rows or columns before you even start cleaning it.

![But they are there! RIGHT IN FRONT OF YOU](https://github.com/rhymeswithfloral/hist5702w_notebook/blob/master/OpenRefine/OR_facets.png?raw=true)

The next step in the tutorial is **Remove blank rows**, now at this point myself as well as many of my peers hit the exact same obstacle. The tutorial tells us that we need to apply a facet to the Record ID column, however, when several of us did this our facet returned with "No numeric value present." This would have been a great opportunity for a picture. While the tutorial implies that after doing this step a subset of data for this column should have been created, however, this is a situation where the authors of the tutorial have clearly left out a step or forgotten to state that each of columns are currently read as text and before we are able to apply a numeric facet the column encoding needs to be edited so that OpenRefine will read it as a numeric value.

![Unnecessary stress](https://github.com/rhymeswithfloral/hist5702w_notebook/blob/master/OpenRefine/OR_facets2.png?raw=true)

**Removing duplicates**, another section where screen shots would do a much better job of explaining things. "Click the Sort menu that just appeared at the top," you mean the same sort menu I just used? No! They don't! A new drop down menu suddenly appears on the menu bar above the column fields. It is very sneaky and it took me a while before I realized that it was even there.



I should point out that I really appreciate the fact that this tutorial includes warnings and tips and bits of information that are useful to know and understand for any person who decides to use OpenRefine beyond this tutorial. Understanding how these steps may produce different results outside the context of these tutorials is the most important thing,  what's the point of following at tutorial if you can't apply any of what it teaches you to your work.

![Death Match: Rows vs. Records](https://github.com/rhymeswithfloral/hist5702w_notebook/blob/master/OpenRefine/OR_recordsvsrows.png?raw=true)

Under the **Atomization** heading, the tutorial discusses the different ways of viewing this data. Switching from _rows_ to _records_ better incorporates the parsing of the categories column. The additional rows that were created by this parsing no longer appear segmented from the record which they were parsed from. I was not aware of how valuable this option to switch between rows and records could be.

![Let us cluster so that we can merge!](https://github.com/rhymeswithfloral/hist5702w_notebook/blob/master/OpenRefine/OR_mergecluster.png?raw=true)

When it comes to the merge and cluster section, ~~finally a screenshot,~~ I think the tutorial does a pretty excellent job of explaining how the default works. However, when they refer to the other methods of clustering I'm not sure they stress enough how careful you need to be. Using the _nearest neighbor_ method several possible cluster options are returned, most of which it is pretty clear that the values aren't the same however there are some that, if you weren't entirely familiar with your dataset, that you could be more difficult to determine whether or not they should be clustered. I ended up only choosing to cluster 3 out of the 54 clusters that this method found.

|A|B|
|----------|---------|
|Air bricks| Airbricks|
|Bonbon dishes| Bon bon dishes|
|Swatchbooks| Swatch books|

Beyond those three, I didn't feel comfortable to make assumptions about any of the other clusters (some which were just a difference between plural and singular). This hesitation is from my experience working with OpenRefine last year.

When I got to the section that involved using GREL to clean up rows and identify unique categories, that is when things got a little hairy again. The tutorial so far has been extremely exact with the events and numbers that will occur through the process, so when I got 33006 cells instead of the 32559 cells after applying `value.split('|').uniques().join('|')` to the categories column I was a little confused. I would have liked some more information about GREL within the tutorial itself and some description of how the string worked. I think this would have helped me figure out why the number of affected cells was different than the number specified in the tutorial.

![Huh?](https://github.com/rhymeswithfloral/hist5702w_notebook/blob/master/OpenRefine/OR_grelunique.png?raw=true)

Based on the [link](https://github.com/OpenRefine/OpenRefine/wiki/GREL-Functions) that is provided in the tutorial, `unqiues()` will remove duplicates within the split values removed. `join('|')` will rejoin the cells which were split with `value.split('|')` and reinsert a `'|'` between each value.

For me to have had a higher number of cells affected by this action implies that there were a larger number of duplicates found than when this tutorial was made. I'm not sure if that makes much sense, since it would suggest that the file which I downloaded for this tutorial, phm-collection.tsv, is different from the version that was used in the tutorial?
This mystery was left unsolved.

Overall I think, despite some issues, this is a fairly functional and straightforward tutorial. Though there are areas which lack detail and explanation, it does provide a good sense of what OpenRefine can do.

---
