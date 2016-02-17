# How to Win at NER

## The Problem
A quick glance at the NER exercise in the [HIST3709o workbook](http://workbook.craftingdigitalhistory.ca/supporting%20materials/ner/#pc-users) will warn you that if you use Windows there will likely be some struggles ahead. However, my difficulties began far earlier than I had expected ~~likely due to the fact that Windows 10 is terrible~~. After unzipping the stanford-ner-2015-12-09.zip, there was no response when I double-clicked on _ner-gui.bat_. There was just a brief flicker of a command line window opening  and then disappearing. I read through the [PC installation instructions](http://nlp.stanford.edu/software/CRF-NER.shtml#Starting) on the SNER website and through the README file, this confirmed my suspicions that the problem had something to do with Java or the JAR file, so I checked to make sure my java was up to date, I checked to make sure that the correct path for Java was entered into my environmental variables.


## The Solution
**A quicker version of the solution can be found at the bottom of this note.**
When I confirmed all of that I turned to the internet and immediately found mentions of operating NER through Python and this lead me to [SNER_preparer](https://gist.github.com/troyane/c9355a3103ea08679baf). To be honest I wasn't sure what to do with this at first, it is referred to as a script but I didn't think it would run correctly if I attempted to use it as a .sh file ~~and I didn't even try.~~ but I decided to use Git Bash to carry out lines 11 to 19. Before doing this I deleted the unzipped folder and started from square one.

In Git BASH I entered (running from DH work folder):

```
$ mkdir NER
$ cd NER
```

I then copied the stanford-ner-2015-12-09 zip file into this new directory before returning to BASH

```
$ unzip stanford-ner-2015-12-09.zip
$ mkdir stanford-ner
$ cp stanford-ner-2015-12-09/stanford-ner.jar stanford-ner/stanford-ner.jar
$ cp stanford-ner-2015-12-09/classifiers/english.all.3class.distsim.crf.ser.gz stanford-ner/english.all.3class.distsim.crf.ser.gz
$ cp stanford-ner-2015-12-09/classifiers/english.all.3class.distsim.prop stanford-ner/english.all.3class.distsim.prop

$ echo " . . . Clearing all"
$ rm -rf stanford-ner-2014-08-27 stanford-ner-2014-08-27.zip
```

At this point I went to my new directory and double clicked on _stanford-ner.jar_ and voila! It opened! However, reviewing the NER exercise I realized that this process meant that I had deleted to folders that for my purposes  were necessary to hold on to. _stanford-ner/classifiers_ and _stanford-ner/lib_. So instead of following the above lines, follow these ones, this way you can skip some of the backtracking I had to do to recover the classifiers folder and the library folder. Please note that I typed out each line individually, and *do not* include *$*.

```
$ unzip stanford-ner-2015-12-09.zip
$ mkdir stanford-ner
$ cp stanford-ner-2015-12-09/stanford-ner.jar stanford-ner/stanford-ner.jar
$ cp stanford-ner-2015-12-09/classifiers stanford-ner/classifiers
$ cp stanford-ner-2015-12-09/lib stanford-ner/lib
```

## The Result
The final test for making sure that I had everything correctly setup was to run the line from HIST3709o exercise, `java -mx500m -cp stanford-ner.jar edu.stanford.nlp.ie.crf.CRFClassifier -loadClassifier classifiers/english.all.3class.distsim.crf.ser.gz -textFile texas-letters.txt -outputFormat inlineXML > “my-ner-output.txt”`, however, I made a few adjustments. instead of `texas-letters.txt` I used a file for my own project, `pamphlet_JS_aconfirmation.txt` and instead of `"my-ner-output.txt"` I inserted `ner_JSpamphlet.txt` and omitted the quotation marks. Make sure that the text file which you want to run through NER needs to be located in _stanford-ner_ directory.

The line I entered looked like this:
```
$ java -mx500m -cp stanford-ner.jar edu.stanford.nlp.ie.crf.CRFClassifier -loadClassifier classifiers/english.all.3class.distsim.crf.ser.gz -textFile pamphlet_JS_aconfirmation.txt -outputFormat inlineXML > ner_JSpamphlet.txt
```
A few seconds after hitting enter _ner_JSpamphlet.txt_ appeared in my new directory! You can find it [here](https://github.com/rhymeswithfloral/hist5702w_notebook/blob/master/ner_JSpamphlet.txt)

# You win!

I've add a very brief How-To in my HIST5702w Notebook with all the snippets of code that I have edited to help you *win at NER* and you can find that [here](https://github.com/rhymeswithfloral/hist5702w_notebook/blob/master/How-to-NER.txt).
