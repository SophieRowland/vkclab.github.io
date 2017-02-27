# VKC Lab Website

hi!

## Contents

#### HTML (.html) pages

- index
- people
- photos
- publications
- etc
- contact

#### CSS (.css) files

- style (shared across all pages)
- index
- people
- photos
- publications
- etc
- contact

#### JavaScript (.js) files

- publications -- script to list all papers from JSON object (mentioned below)

#### JSON (.json) files

- contains a JSON object of all published papers; exported from a Google Spreadsheet titled "VKC Lab Publications (Web)"

----------

## Instructions for updating the list of publications

_Each time you update the list of publications, you must also extract a JSON file and copy that into **publications.json**. You must then push the changes to GitHub. Instructions for how to do so are below._

1. Open the Google Spreadsheet titled "VKC Lab Publications (Web)".

2. At the bottom are several "sheets" labeled in decreasing order, from 2016 to 2004.

  * If the publication you want to add is from a year that does not already have a sheet (e.g. when adding the first publication from 2017)...

    1. Create a sheet with that year and drag it to the front of the queue.

    2. Copy the first line from another sheet (the line with "Author, Title, ..."). __It is crucial that the first line does not vary from sheet to sheet.__

    3. (Optional: Drag the thick line right below "A, B, C, ..." down by one cell, such that it's below the first line. The first line will now be fixed at the top as you scroll down the spreadsheet.)

    4. Enter the relevant information from the publication in the second line: Author, Title, Journal, Pages (if applicable), and links.

  * If the year already exists in the sheets, insert a blank row between the first and second row, and add the relevant information from the publication: Author, Title, Journal, Pages (if applicable), and links.

3. Towards the top of the spreadsheet is the menu ("File, Edit, View, ..."). The last option in the menu should be "Export JSON"; click on it, and then "Export JSON for all sheets."

  * Note that you may first need to give the script permission to run on Google Spreadsheets. Also note that clicking on "Export JSON for _one_ sheet" will only give you the data (publications) for one year, whereas we want the data for all years.

  * This step is necessary because instead of using the spreadsheet's data directly for the website, we export a JSON object, which is easily parsed with JavaScript, which is a language web browsers use.

  * In case of a bug in this step, the export-JSON script was copied from [this GitHub Gist](https://gist.github.com/pamelafox/1878143), which should contain instructions on how to debug.

4. A window of the exported JSON should pop up. (These are the cells of each sheet converted into a JSON object.) Copy and paste this newly-generated JSON into the __publications.json__ file, _making sure to erase over the previous content_. (Otherwise you'd have duplicates.)

5. Push the changes from __publications.json__ to GitHub, to the _main_ branch. This change should [be reflected online](http://vkclab.github.io/json/publications.json) (there might be a slight delay). (In short, the jQuery API that loads JSON-encoded data must "use a GET HTTP request", which means the jQuery uses [an absolute URL intead of a relative URL](https://kb.iu.edu/d/abwp), and so the JSON file must be online at the specified URL. It also means that if we rename __publications.json__ file or move it elsewhere, we'd need to specify this in the accompanying .js file so that it can still access the data.)

The JavaScript file _**publications.js**_ should then automatically format the JSON objects into user-friendly listings on the Publications page.

----------

## Instructions for changing the information listed in Publications

_e.g. We now no longer want to list the Journal for each publication, or we want to remove the links to Google Scholar. Or hypothetically, there is some new database called Apple Scholar and we want to add links to that for each publication._

### Removing a column of information:

Changes to this can be done in the _**publications.js**_ file alone. There is a middle section, approx. lines 20-60, where the JavaScript parses the JSON file and modifies the content of the HTML. To remove a column of information, simply comment out the relevant column. (This should be straightforward as the .js code is well-commented.)

For example, when removing the "journal" column, we simply comment out the line where it gets added to the HTML:
```javascript
// add journal
paper += "<i>" + jsonData[y][i]['journal'] + "</i> ";
```
then becomes
```javascript
// add journal
// paper += "<i>" + jsonData[y][i]['journal'] + "</i> ";
```

It is not necessary to modify the Google Spreadsheet nor the exported JSON because we simply do not display the information.

### Adding a column of information:

Changes to this must be done in both the _**publications.js**_ file as well as __publications.json__. (And to modify __publications.json__, you must of course update the Google Spreadsheet and reexport the JSON.)

1. Update the Google Spreadsheet with a new column of information (e.g. a link to the hypothetical Apple Scholar citation). Name the column something relevant, e.g. "applscholar". This will be a new key in your JSON object.) __Please make sure that the first column is consistent across all sheets in the spreadsheet, even if not every cell in that column is filled.__ 

2. Export the JSON from the Google Spreadsheet and update the __publications.json__ file. (Instructions above.)

3. Add relevant lines of code to _**publications.js**_ so as to print out information from the new column. The information can be accessed with `jsonData[y][i]['key']`, with "key" being a string of the previously-named column, e.g. 'applscholar'.
