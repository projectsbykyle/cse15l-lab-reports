# Lab Report - Week 3 - Options for the grep command

## Main Objectives
In this lab, we discuss the **grep** command's options and some examples of how we can use them in practice. 

## Intro
The **grep** command is a terminal command that we can use to find a given pattern in a specified file. It searches for the pattern and displays all ines that contain it. 

An example of using the **grep** command is shown here. We assume that there is a file called **abc.txt**. 
```
abc.txt:
abc hello
def
ghi
abc bye
xyz
php
```
Assume that we are attempting to find all lines that contain the string "abc." We will do so as follows: 

```
1: grep "abc" abc.txt
GREP OUTPUT:
abc hello
abc bye
2: 
```
This seems simple at first sight, but with command options, we can make **grep** much more applicable in practice. The general format for **grep** is as follows: 
```
grep [options] file-path
```
Following this intro, we will explore different options that can help us in practice to make our jobs easier. 

## The -l option
Sometimes, we do not need to know which specific lines contain a string pattern; we simply need to find which files contain it. For example, in programming project, we may need to get all of the **.java** files with that have **interface** declarations. 

**grep** allows us to do this with the **-l** option. Inputting this tells the command to find all files that contain the string pattern we are looking for and print it to the console. 

The format is as follows:
```
grep -l "insert string pattern here" <file(s)>
```
For example, in the given lab directory **./technical**, we can find all text files that contain the words **"nuclear membrane"**.

```
Laptop:technical user$ grep -l "nuclear membrane" */*.txt
biomed/1471-2121-3-25.txt
biomed/1471-213X-1-12.txt
biomed/1471-2156-3-17.txt
biomed/1471-2164-2-8.txt
biomed/1471-2350-4-4.txt
biomed/1471-2407-1-19.txt
biomed/1475-9268-1-2.txt
```
All of the files listed contain the words **"nuclear membrane"** somewhere in their text. The command expanded the wildcards in the user input, and it scanned the entirety of each file, flagging any **.txt** files that contain this pattern. It then returned the list to the user. This can be useful for navigating a large folder of many research papers, as we would not need to click and open every text file to find one related to what we are researching. 

Another example (in the same directory as above) is shown here as we search for all law articles containaing the phrase **"pro bono"**. 

```
Laptop:technical user$ grep -l "pro bono" */*/*.txt
government/About_LSC/CONFIG_STANDARDS.txt
government/About_LSC/ONTARIO_LEGAL_AID_SERIES.txt
government/About_LSC/Progress_report.txt
government/About_LSC/Special_report_to_congress.txt
government/About_LSC/State_Planning_Report.txt
government/About_LSC/State_Planning_Special_Report.txt
government/About_LSC/Strategic_report.txt
government/About_LSC/commission_report.txt
government/About_LSC/conference_highlights.txt
government/Media/A_Perk_of_Age.txt
government/Media/Assuring_Underprivileged.txt
government/Media/Attorney_gives_his_time.txt
government/Media/Barnes_new_job.txt
government/Media/Barnes_pro_bono.txt
government/Media/Butler_Co_attorneys.txt
government/Media/Civil_Matters.txt
government/Media/CommercialAppealMemphis2.txt
government/Media/Commercial_Appeal.txt
government/Media/Disaster_center.txt
government/Media/Few_who_need.txt
government/Media/Funding_May_Limit.txt
government/Media/Good_guys_reward.txt
government/Media/Greedy_Generous.txt
government/Media/GreensburgDailyNews.txt
government/Media/Helping_Hands.txt
government/Media/Helping_Out.txt
government/Media/IOLTA_INTEREST_RATE.txt
government/Media/Law_Schools.txt
government/Media/Local_Attorneys.txt
government/Media/Marylands_Legal_Aid.txt
government/Media/New_Online_Resources.txt
government/Media/Oregon_Poor.txt
government/Media/Philly_Lawyers.txt
government/Media/Politician_Practices.txt
government/Media/Poverty_Lawyers.txt
government/Media/Pro_Bono_Services.txt
government/Media/Program_Lodges.txt
government/Media/Providing_Legal_Aid.txt
government/Media/Raising_the_Bar.txt
government/Media/Retirement_Has_Its_Appeal.txt
government/Media/State_funding.txt
government/Media/Terrorist_Attack.txt
government/Media/Texas_Lawyer.txt
government/Media/The_Bend_Bulletin.txt
government/Media/The_State_of_Pro_Bono.txt
government/Media/Too_Crucial_to_Take_Cut.txt
government/Media/Using_Tech_Tools.txt
government/Media/Valley_Needing_Legal_Services.txt
government/Media/Volunteers_Step_Up.txt
government/Media/Wilmington_lawyer.txt
government/Media/Working_for_Free.txt
government/Media/balance_scales_of_justice.txt
government/Media/grants_fail_to_come.txt
government/Media/pro_bono_efforts.txt
```
Like before, the command expands the wildcards to all matching directories and files. It then searches throuh each file, flagging all that contain the phrase **pro bono**. It then prints all articles that have been flagged to the console. 

Lastly, in the same directory, we try to find all files containing the phrase **"Olfactory Perceptual Space"**. 

```
Laptoptechnical user$ grep -l "Olfactory Perceptual Space" */*.txt
plos/journal.pbio.0030137.txt
```
This time, after scanning all matching files, only one text file was flagged. Even though the directory contains thouands of files, scanning and returning this singular file only took a few seconds. This highlights another useful feature of **grep** and terminal commands in general: they finish a user-inputted job much more quickly than a mouse. 

## The -n option
After getting all files that contain a specified string pattern, we sometimes need to find the line in which they are contained. One option is to read the text files ourselves to find them, but **grep** provides a much faster option for us. 

The **-n** option searches all user-inputted files for a string pattern and returns the lines and line numbers on which they were found. 

The format is as follows:
```
grep -n "insert string pattern here" <files>
```
In this example, we use the command inside **./technical/biomed**, trying to find all lines that contain the string **cellular respiration**.

```
Laptop:biomed user$ grep -n "cellular respiration" *.txt
1471-2148-1-4.txt:236:        all six of the proteins involved in cellular respiration
1471-2148-2-14.txt:142:        highly reproducible; and 3) cellular respiration can occur
```
As shown in the console, **grep** not only identifies which files contain the string, it also prints the line and line number on which the pattern was found. This makes access of large folders much easier, as **grep** not only helps us find the files related to what we are looking for; it also helps us identify the lines that the information is located on. 

We can also do the same for a single file. Here, after using **grep -l** to find a text file containing the words **"nuclear membrane"**, we use **grep -n** to find the line containing it. 

```
Laptop:biomed user$ grep -l "nuclear membrane" *.txt
1471-2121-3-25.txt
1471-213X-1-12.txt
1471-2156-3-17.txt
1471-2164-2-8.txt
1471-2350-4-4.txt
1471-2407-1-19.txt
1475-9268-1-2.txt
Laptop:biomed user$ grep -n "nuclear membrane" 1471-213X-1-12.txt
29:        nuclear membrane of the donor cells remains intact due to
```
As shown in the output, **grep -n** prints the exact line and line number containing the phrase **"nuclear membrane"**. It is important to note that here, **grep** does not print the file name of the article containing the string and only the line number. Like before, this makes article access more efficient and quicker. 

Lastly, we make use of this in **./technical**, where we attempt to find all lines containing the word **"dragon"**.

```
Laptop:technical user$ grep -n "dragon" */*.txt
biomed/1471-2148-2-2.txt:39:        snapdragon [ 3 ] , rice [ 4 ] , maize [ 5 ] , pine [ 6 ]
biomed/1472-6831-3-1.txt:550:        Decim supplied us with the steel dies. Mr. E Mondragon
```
Here, we can see that **grep** ignores that there are not singular instances of **dragon**. It only searches for any instance of it, be it in a compound word or by itself. To use this **grep -l** to its fullest advantage, we need to be more precise in our input. For example, here, we could have inserted spaces at both ends of the input: **" dragon "**. 

## The -c option
Sometimes, we consider the amount of times an expression is repeated in an article to know how relevancy of our topic within the file. Like before, **grep** provides a quick and easy way to finding this. 

The **-c** option outputs text files that contain a particular string expression along with the number of times it was found. This will make it much easier to gauge how relevant our search term is within the text file. 

The format is as follows:
```
grep -c "insert string pattern here" <files>
```
In this example, we attempt to find text files containing the term **"cellular respiration"** within **./technical** and the number of occurences of that expression within them. 

```
Laptop:technical user$ grep -c "cellular respiration" biomed/*.txt
biomed/1471-213X-1-13.txt:0
biomed/1471-213X-1-15.txt:0
biomed/1471-213X-1-2.txt:0
biomed/1471-213X-1-3.txt:0
biomed/1471-213X-1-4.txt:0
biomed/1471-213X-1-6.txt:0
biomed/1471-213X-1-9.txt:0
biomed/1471-213X-2-1.txt:0
biomed/1471-213X-2-7.txt:0
biomed/1471-213X-2-8.txt:0
biomed/1471-213X-3-2.txt:0
biomed/1471-213X-3-3.txt:0
biomed/1471-213X-3-4.txt:0
biomed/1471-213X-3-7.txt:0
biomed/1471-2148-1-1.txt:0
biomed/1471-2148-1-14.txt:0
biomed/1471-2148-1-4.txt:1
biomed/1471-2148-1-6.txt:0
biomed/1471-2148-1-8.txt:0
biomed/1471-2148-2-12.txt:0
biomed/1471-2148-2-14.txt:1
biomed/1471-2148-2-15.txt:0
(removed portion of output for clarity)
```
As we can see, one obvious downside of this is that it prints the occurrence of the string in all files regardless if it was found or not. Navigating this may be somewhat cumbersome. However, the output does its job: it flagged the files containing **"cellular respiration"** and printed the amount of lines that contains it. 

A way that will make it easier for the user to identify the files is to first use **grep -l** to identify files that contain a search term, then use **grep -c** to find the number of occurences.

For example, here, we continue the search for **"cellular respiration"** inside **./technical**: 
```
Laptop:technical user$ grep -l "cellular respiration"  */*.txt
biomed/1471-2148-1-4.txt
biomed/1471-2148-2-14.txt
Laptop:technical user$ grep -c "cellular respiration" biomed/1471-2148-1-4.txt
1
```
As we can see, within the file **1471-2148-1-4.txt**, there is only one line that contains "cellular respiration."

To make it easier overall, we can pair this with **grep -l** using the **pipeline** operator and **xargs**. Combining all of these simplifies this process to one line for us.

For example, here, starting at the **./technical**, we try to find all files that contain the phrase **"photosynthesis""** and the number of lines that contain it. 

```
Laptop:technical user$ grep -l "photosynthesis" */*.txt | xargs grep -c "photosynthesis"
biomed/1471-2148-1-4.txt:5
biomed/1471-2164-4-23.txt:1
biomed/1471-2164-4-26.txt:1
biomed/1471-2229-1-2.txt:2
biomed/1471-2229-2-3.txt:1
biomed/1471-2229-2-4.txt:3
biomed/1471-2229-2-8.txt:2
biomed/gb-2001-3-1-research0001.txt:1
biomed/gb-2002-3-11-research0061.txt:5
biomed/gb-2002-3-5-research0025.txt:1
biomed/gb-2002-3-9-research0045.txt:4
biomed/gb-2003-4-3-r20.txt:1
plos/journal.pbio.0020224.txt:1
plos/journal.pbio.0020306.txt:1
plos/journal.pbio.0020439.txt:2
plos/journ
```
As we can see, unlike the first example for **grep -l**, this only outputs files that actually contain the expression, making it easier for us to read and understand. 

## Conclusion
In this lab report, we discussed some options that we can use with **grep** to make our workflow more efficient.

The first option is **-l**. This identifies all files that contain a given string and prints their file paths. 

Another option is **-n**. This identifies files that contain a given string and prints the lines on which it is located. 

Lastly, the **-c** identifies all files that contain a given string and prints its amount of occurences.

All three of these options will help us quickly find and search large amount of files with precise search terms and judge their relevancy to our task at hand. 