# Kindle-Book-List

> **BLUF:**  Use the Kindle for PC app to generate the KindleSyncMetadataCache.xml. Use that file and a parser program to extract the data fields into a CSV, HTML(Table), MD(Table) or TSV file for use. What follows is my train (wreck) of research to reach this decision.  

This is a research project to collect information from Amazon related to purchased books from the Kindle Cloud Library. All the books added to Kindle account are stored on Amazon servers, not on the particular device. Access your Kindle Cloud Library by logging into your Amazon account then select Manage Your Content and Devices. 

Access 
- Calibre 
- Kindle Library: https://read.amazon.com/kindle-library 
- Kindle for PC/Phone App.
- Google Library: https://play.google.com/books/  (an Alternate)  

Note:
- Kindle Cloud Reader offers very basic features: highlights, notes, or search within a book.  
- You can read your books in the browser, creating notes & highlights, just like using your Kindle Device.  
- You can copy your notes & highlights within the web browser for all synced books.
- Data in the Kindle Cloud Library is stored in a JSON format.  

#### Equipment
* Windows 10 PC
* Kindle Fire HD10

#### References
* [Amazon Product Advertising API (AWS Account)](https://affiliate-program.amazon.com/gp/advertising/api/detail/main.html) - An AWS account is required!  
* [GoodReads API](https://www.goodreads.com/api) - A GoodReads key is required!  
* [Karen's Print Directory](http://www.karenware.com/)  
* [Kovid Goyal's calibre](https://github.com/kovidgoyal/calibre)  
* [Markdown Cheat Sheet](https://github.com/lifeparticle/Markdown-Cheatsheet)  

#### Related GISTS
- https://gist.github.com/jkubecki  
- https://gist.github.com/usayamadx  

## Acronyms
| Acronym | Description |  
| ----------- | ----------- |
| ASIN    | Amazon Store Identification Number;<br> Amazon Standard Identification Number |  
| csrs    | ??? - If you know - let me know! |  
| CSV     | Comma Separated Values (a Database) |  
| CTRL    | Control (a keyboard key!) |  
| HTML    | HyperText Markup Language |  
| JSON    | JavaScript Object Notation (a Database) |  
| K4PC    | Kindle for Personal Computers |  
| K4W     | Kindle for Windows |  
| TSV     | Tab Separated Values |  
| XHR     | XML HTTP Request |  
| XML     | eXtensible Markup Language (a Database) |   

## Build the Query
| Query Type | Options Available |  
| ----------- | ----------- |
| sortOrder | { DESCENDING &#124; ASCENDING } |  
| sortIndex | { DATE &#124; TITLE &#124; AUTHOR } |  
| startIndex | <StartIndex> ( 0, then multiples of batchSize) |  
| batchSize | <bsize> ( Max 50 ) |  
| contentType | Ebook |  
| itemStatus | { Active &#124; Expired } |  
| excludeExpiredItemsFor | { KOLL &#124; Purchase &#124; Pottermore &#124; FreeTrial &#124; DeviceRegistration &#124; ku &#124; Sample } |  
| originType | { Purchase &#124; PublicLibraryLending &#124; PersonalLending &#124; KOLL &#124; RFFLending &#124; Pottermore &#124; Rental &#124; DeviceRegistration &#124; FreeTrial &#124; ku &#124; Sample &#124; Prime } |  
| isExtendedMYK | { True &#124; False } |  
| csrfToken | encodeURIComponent(csrf) |  

> **Note**: Select one item within {} for your query. Not all elements are required in the query. I haven't tested all possible queries, but the first 5 in the list do work well.  

## Data Elements (Fields)
| Element | Description | Tested - Problems | 
| ----------- | ----------- | ----------- |  
| authors[0] | List of Authors | Yes - colons & commas |  
| title | The Title of the Book | Yes - Series & commas |  
| asin | Amazon's Unique ID Number for the Book | Yes |  
| webReaderUrl |  bookmark to the web Reader | No |  
| productUrl | a link to cover images |  Yes | 
| percentageRead | a guestimate of how much you've read of the book.  | Yes |  
| resourceType | (EBOOK or EBOOK_SAMPLE) | No |   
| originType | (PRIME and PURCHASE) | No |   
| orderDetailURL | URL To your Purchase Order for the Book | No |   
| productImage | URL to the Book cover | Yes - Undefined |   
| acquiredDate | Purchase Date | Yes - Undefined |   

## Additional Possible Fields from the product page:
| Element | Description |  
| ----------- | ----------- |  
| sales_rank Sales | ranking of the book (Date Specific) |  
| rating | Rating of the Book (Date Specific) |  
| number_of_ratings | Number of Ratings for the Book (Date Specific) |  

## Alternate Sources of Book List
* [KindleSyncMetadataCache.xml](https://github.com/MrMikey59/Kindle-Book-List/blob/main/KindleSyncMetadataCache.xml) (a Kindle for PC database normally found at **C:\Users\%USERNAME%\AppData\Local\Amazon\Kindle\Cache**)
  - Books are in Root **<add_update_list>** in **<meta_data>**...**</meta_data>** blocks.  
  - Fields include: *ASIN*, *title*, *authors*, *publishers*, *publication_date*, *textbook_type*, *cde_contenttype*, and *content_type*.
* [Kindle Cloud Reader](https://read.amazon.com/kindle-library) HTML file.
* [GoodReads](https://www.goodreads.com/) or [GoodReads Mobile](https://www.goodreads.com/toggle_mobile) Shelfari or My Books HTML file.
* You can copy/print the file names (mostly ASINs) from your Kindle while in MPT Mode attached to your computer (using Karen's Print Directory) from either of the following directories that must be copied to a local drive temporarily:
  - Internal **storage\Android\data\com.amazon.kindle\files\medium** 
  - Internal **storage\Android\data\com.amazon.kindle\files\small** 
* You could parse the JSON file metadata.calibre created by [calibre](https://github.com/kovidgoyal/calibre) for the device. You may have two of these files with an external storage installed!  

## KindleSyncMetadataCache.xml File Sample Format:
```xml
    <add_update_list>  
      <meta_data>  
        <ASIN></ASIN>  
        <title></title>  
        <authors>  
          <author></author>  
        </authors>  
        <publishers>  
          <publisher></publisher>  
        </publishers>  
        <is_multimedia_enabled></is_multimedia_enabled>  
        <cde_contenttype></cde_contenttype>  
        <content_type></content_type>  
        <publication_date></publication_date>  
      </meta_data>  
    </add_update_list>
```

> **Note**: This file has different information available as compared to the Kindle Cloud Library. a conversion from XML to CSV would be necessary.

## Kindle Cloud Reader HTML file format example.
```html
    <DIV id="titles_inner_wrapper" style="font-size: 191.25px;">  
      <DIV id="B00DJI3HWS" class="book_container">  
        <DIV class="book_cover">  
          <IMG class="book_image book_click_area" src="https://images-na.ssl-images-amazon.com/images/P/B00DJI3HWS.01._SX255_SY255_TTXW_SCLZZZZZZZ_.jpg" title="I Bastardi di Pizzofalcone (Italian Edition)">  
          <DIV class="alt_title book_click_area"></DIV>  
        </DIV>  
        <DIV class="book_details">  
          <DIV class="book_title book_click_area">I Bastardi di Pizzofalcone (Italian Edition)</DIV>  
          <DIV class="book_author book_click_area">Maurizio de Giovanni</div>  
        </DIV>  
      </DIV>  
    </DIV>    
```

> **Note:** Wait for all of your books to be loaded into the browser window, then view it's source and copy the DIV section that start with ID of titles_inner_wrapper.   
This isn't the best choice as clean up of the HTML will take so much more time, and doesn't lend itself to a simple parsing program.

# Using ExportKindleCSV.js for CSV File or ExportKindleTSV.js for TSV File
First, modify the query to support your book list requirements based on the query options available above. 

    let url = domain + 'kindle-library/search?query=&libraryType=BOOKS' + ( paginationToken ? '&paginationToken=' + paginationToken : '' ) + '&sortType=recency&querySize=50'

Second Modify these two line to reflect your data form the data elements to be collected in the CSV creation (This example has ASIN, Title, & Authors):

    let csvData = "ASIN,Title,Author(s)\n"
    csvData += item.asin + '","' + item.title + '","' + item.authors + '\n'

Make sure you put the '\n' at the end or all items will be on one row following the header created in the first line.  

Go to your [Kindle Cloud Reader](https://read.amazon.com/kindle-library) in Chrome, or FireFox. Open the Developer Console.  
* In FireFox useTools > Web Developer (Ctrl+Shift+C)  
Paste the code block in and then select RUN.  A dialog box will open when the download completes so you can save or open the new CSV file. Go check out your saved CSV file in the Downloads folder of the current user!  

> **Note**: Run this script in FireFox’s Multiline mode. 

Here's a screenshot of the console as it's running:

[![Script Running](https://github.com/MrMikey59/Kindle-Book-List/blob/main/ScriptRunning.png)](https://github.com/MrMikey59/Kindle-Book-List/blob/main/ScriptRunning.png) 

Here's a screenshot of the Save File dialog box:

[![Save the File](https://github.com/MrMikey59/Kindle-Book-List/blob/main/SaveFileDialog.png)](https://github.com/MrMikey59/Kindle-Book-List/blob/main/SaveFileDialog.png) 

Screenshot in Windows File Explorer showing the Saved file (Your name will vary!):

[![Saved File](https://github.com/MrMikey59/Kindle-Book-List/blob/main/FileExplorerScreenShot.png)](https://github.com/MrMikey59/Kindle-Book-List/blob/main/FileExplorerScreenShot.png) 

I've included a test run file that I created for my 17000+ book Library.  Note, only the first 10000 books were gathered. This appears to be a limitation of the AMAZON data exchange, not the script.  I've included a Raw Object file that I copied from the console for those that like to look under the hood!

Screenshot of FireFox's Console where I collected the [Raw Object data](https://github.com/MrMikey59/Kindle-Book-List/blob/main/RawObject.txt)  (Pink):

[![Raw Object Data Collection](https://github.com/MrMikey59/Kindle-Book-List/blob/main/FFBrowserConsole.png)](https://github.com/MrMikey59/Kindle-Book-List/blob/main/FFBrowserConsole.png) 

> **Note**: You can use multiple sort options to get a broader listing and then combine the files and remove duplicates.

### Second Running of Script?

Script cannot be run consecutively.  Refresh your browser page and then rerun. If you do run it again without the refresh, here’s the clue:

[![Script Run Error](https://github.com/MrMikey59/Kindle-Book-List/blob/main/SecondRunError.png)](https://github.com/MrMikey59/Kindle-Book-List/blob/main/SecondRunError.png) 

> **Note**: You will not lose the console or the script when you refresh.

> **Note**: Using the TSV version solves the problem with title or authors using commas, but adds steps to convert it to a proper spreadsheet by importing it into Excel or Google Sheets.

### Change App Default for TSV File

Using Windows Settings, I tried to change the Default App to Excel, but it's not possible:

[![Change TSV Default App](https://github.com/MrMikey59/Kindle-Book-List/blob/main/TSVFileDefaultApp.png)](https://github.com/MrMikey59/Kindle-Book-List/blob/main/TSVFileDefaultApp.png)

And the Windows Store doesn't recognize the file type!  So I went to the trouble to "Open With" then selecting more apps till I seek out the Excel.exe file.  I selected that and the change was made to my "Open With" selections.  And now I see these: 

From File Explorer Context Menu:  
[![From File Explorer Context Menu](https://github.com/MrMikey59/Kindle-Book-List/blob/main/TSVOpenWith1.png)](https://github.com/MrMikey59/Kindle-Book-List/blob/main/TSVOpenWith1.png)

From Choose Another App:  
[![From Choose Another App](https://github.com/MrMikey59/Kindle-Book-List/blob/main/TSVOpenWith2.png)](https://github.com/MrMikey59/Kindle-Book-List/blob/main/TSVOpenWith2.png)

Note: I recommend not making it a permanent change because you can review the file in NotePad the way it is...and import to a spreadsheet when you are ready.

# Using the Kindle Directory Small or Medium for ASIN's 

After copying the small directory to my desktop, I used [Karen' Print Directory](http://www.karenware.com/) to create a text file of the file names. I deleted most of the header created by Karen's PD but left the odd names for you to review. The file [DirPrntInfo.txt](https://github.com/MrMikey59/Kindle-Book-List/blob/main/DirPrnInfo.txt) is available for your underhood review.   
Now, with a list of ASINs, you can use some of the other GIT projects to acquire further details about the books.  With >10000 books, this may be an easier way to get the data than the previously tested idea above.

After completing the header removal, the extraneous non-ASIN file listings and the "FILE      " lead in to each ASIN, I now have the functional [ASINList.txt](https://github.com/MrMikey59/Kindle-Book-List/blob/main/ASINList.txt).


# Using the Kindle Sync Metadata Cache XML File from Kindle for PC

I copied the file into a Word temperary document. I Prettied with an edit replace to see all the lines in a more readable format using :
    Search Field: *><*  
    Replace Field: *>^p  <*  (Note I put two spaces there to add an indent.)  
This process took >45 minutes with my ~17,600 records - causing some ~270,000 replacements and that was just the first pretty step, now with ~7800 pages!

Due to size, I deleted many <meta_data>...</meta_data> entries to make the file usable for testing - also a just little time consuming! Using *[CTRL] + [SHIFT] + END* from a high point at at <meta_data> markermade selection and cursor back up the an end tag made the mass deletion a lot easier in WORD!

*NOTE*: This is a research project, so I did all of the above in WORD to see how long it would take. I know it could be programatically done much faster.  ;)  

The prettier [KindleSyncMetadataCache.xml](https://github.com/MrMikey59/Kindle-Book-List/blob/main/KindleSyncMetadataCache.xml) is now ready for further XML to CSV or TSV processing.
      
### KindleSyncMetadataCache.xml

Basic details about the [file](https://github.com/MrMikey59/Kindle-Book-List/blob/main/README.md#kindlesyncmetadatacachexml-file-sample-format) needed for parsing
| Node | Element Used|  
| ---- | ---- |  
| Root Node | <add_update_list> |  
| Book Nodes | <meta_data>…</meta_data> |  
      
Using the add_update_list section with an XML DOCID:
|Child Node Index|Node ID|Notes|
| :---: | --- | --- |  
|0|ASIN|Amazon’s Standard Identification Number (Primary Key)|  
|1|title|May contain a series, commas, and colons| 
|2|authors|May contain multiple entries (<author pronunciation="">…</author>), commas and colons| 
|3|publishers|May contain multiple entries (<publisher>…</publisher>), commas and colons| 
|4|publication_date|Left(<Node>,10) trimming required.| 
|5|purchase_date|Left(<Node>,10) trimming required.| 
|6|textbook_type|| 
|7|cde_contenttype|EBOK| 
|8|content_type|MIME: `application/x-mobipocket-ebook`| 

Use:
```vbscript
  Set xmlDocRoot = xmlDoc.childNodes(1)
' Get to the add_update_list
  Set xmlDocRecord = xmlDocRoot.childNodes(1)
' Set the Root for the Record Set
  Set xmlDocList = xmlDoc.getElementsByTagName("meta_data
```
With the full XML package as saved by Kindle for PC:

|Child Node Index|Node ID|Notes|
| :---: | --- | --- |
|0|?xml|<?xml version="1.0"?>|
|1|response||
|3|cache_metadata||
|4|add_update_list|Broken down above|

Use:
```vbscript
  Set xmlDocRoot = xmlDoc.childNodes(1)
' Get to the add_update_list
  Set xmlDocRecord = xmlDocRoot.childNodes(4) 
' Set the Root for the Record Set
  Set xmlDocList = xmlDoc.getElementsByTagName("meta_data")
```
      
## Using Excel 2017/O365

After updating the KindleSyncMetaqDataCache.xml, I used Excel 2016/O365 to import the data into the [spreadsheet My Kindle Books](https://github.com/MrMikey59/Kindle-Book-List/blob/main/My%20Kindle%20Books.xlsx) in the following process:
>1. File > Open, Choose the file location.
>2. Since no schema is provided, select to open the XML as a spreadsheet (first option)
>3. Review the file!

Problem I noticed:
- Sync Date & Time was added to every book. Deleted the column.  
- Columns headers were exactly the XML Element name. Attributes were also expanded into their own column, so I deleted them.  
- Mulitiple Authors/Publishers were treated to duplicate entries with repeated information from the book's other information.  
- Dates & Time are large details that will need trimming.  

My Conclusion - this is a viable option, Excel converts the file (17700+ Unique Books) quickly, but some serious massaging of the data is required to make this a **GOOD NORMAILIZED** database.

# Alternatives to Consider  

## Amazon.js
```javascript
/* 
 * Extract kindle book list from the amazon web site
 * Code is written in phantomjs
 *
 * Usage: phantomjs --ssl-protocol=any [-latest] amazon.js e-mail password
 * Outputs two files:
 * 1. booklist.txt - a JSON stringified representation of the items
 * 2. booklist.csv - a CSV version of the relevant fields from the page
 *
 * Note this code is quite fragile. It relies on an internal query from 
 * the 'Manage Devices ...' page the api only returns the first 1000 
 * entries regardless of the batch size used.
 * Because the API only returns 1000 entries, it reads the list 6 ways. 
 * Note if you have over 2000 books you will not get a full listing - has 
 * been tested on a library with 3500+ books. The book list is queried by 
 * decending then ascending date, then title, then author. Duplicates are 
 * removed.
 * The -latest option only extracts up to 1000 latest books
 *
 * The Phantomjs browser needs to pretend to be an interactive browser
 * (the userAgent string is from Chrome and may get out of date). If the 
 * user agent is not faked then amazon does not generate the relevant 
 * cookies.
 * The Devices page can hang the browser. Need to use ssl-protocol flag on
 * phantomjs as the default is not accepted by amazon at this time.
 *
 * For diagnosis - A number of screen shots are created.
 * Calls the UK site - not sure if this works for non-UK owners.
 *
 * @author Justin Saunders
 *
 */

var page = require('webpage').create(),
    system = require('system'),
    fs = require('fs'),
    email, pass, loadInProgress = false, multiLoad = false, stepindex = 0, steps = [], latest = false, csrf='';

page.settings.userAgent = 'Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Ubuntu Chromium/55.0.2883.59 Chrome/55.0.2883.59 Safari/537.36';
console.log('Using user agent of ' + page.settings.userAgent);

if ((system.args.length !== 3)&&((system.args.length != 4)||(system.args[1]!= '-latest'))) {
    console.log('Usage: amazon.js [-latest] <user> <pass>');
    phantom.exit();
} else {
  if(system.args[1] == '-latest') {
      email = system.args[2];
      pass = system.args[3];
    latest = true;
  } else {
      email = system.args[1];
      pass = system.args[2];
  }
    console.log("User: " + email);
}

// Simple heart beat for every page loaded.
page.onLoadStarted = function() {
    loadInProgress = true;
    console.log("load started");
};

// Page loaded now.
page.onLoadFinished = function() {
    loadInProgress = false;
    console.log("load finished");
};

/**
* steps contains all of the pages to be visited.
 * A series of functions that make one page call or examine a page
 * unless multiload is true
*/
// Initial landing page - we will be redirected to login as part of this
// Yes this is the UK site
steps[0] = function() {
// Initial page load
    console.log('RUNNING INITIAL PAGE');
    page.open('https://www.amazon.co.uk/gp/yourstore/home/ref=nav_cs_ys',
        function(status) {
        if(status !== 'success') {
            console.log('Failed to load the page: ' + status);
            phantom.exit();
        }
    });
};
// Login page - will be redirected here after going to the home page
// fill in the fields - now split into two stages
steps[1] = function() {
    console.log('RUNNING LOGIN PAGE');
    console.log('Rendering page to amazon_login.png');
    page.render('amazon_login.png');
    page.evaluate(function(e, p) {
        document.getElementById('ap_email').value = e;
// there are two continue id items - the span and the button
// select by class
        document.getElementsByClassName('a-button-input')[0].click()
    }, email, pass);
};
// Login page - will be redirected here after going to the home page
// fill in the fields - part 2
steps[2] = function() {
    console.log('RUNNING LOGIN PAGE 2');
    console.log('Rendering page to amazon_login2.png');
    page.render('amazon_login2.png');
    page.evaluate(function(e, p) {
        document.getElementById('ap_password').value = p;
        document.getElementById('signInSubmit').click();
    }, email, pass);
};
//Home page - will be redirected here after entering the login details
// But not all the time sometimes we get a captcha at this point
steps[3] = function () {
    console.log('Running step 3');
    console.log('Rendering page to amazon.png');
    page.render('amazon.png');
    if(page.title == "Amazon Sign In")
    {
        console.log("captcha found please look at amazon.png and enter the characters at the prompt");
        system.stdout.writeLine('Enter Captcha: ');
        var cp = system.stdin.readLine();
        console.log('Captcha: ' + cp)
        // console.log(page.evaluate(function() { return document.body.innerHTML}));
        page.evaluate(function(e, p, c) {
            document.getElementById('ap_password').value = p;
            document.getElementById('auth-captcha-guess').value = c;
            document.getElementById('signInSubmit').click();
          }, email, pass, cp);
    }
    console.log('Rendering page to amazon_next.png');
    page.render('amazon_next.png');
};
// Load the Manage Devices page which will initialize the cookies to call
// the web service. This visit sometimes hangs.
steps[4] = function() {
    // Load the devices page
    console.log('RUNNING DEVICES PAGE');
page.open('https://www.amazon.co.uk/mn/dcw/myx.html/ref=kinw_myk_redirect#/home/content/booksAll/dateDsc/',
        function(status) {
        if(status !== 'success') {
            console.log('Failed to load the page: ' + status);
            phantom.exit();
        }
        /* note that this consistently renders blank as a result of recent changes */
        console.log('Rendering page to amazon_devices.png');
        page.render('amazon_devices.png');
        csrf = page.evaluate(function() {
            return csrfToken;
        });
        /* console.log("CSRF Token is " + csrf); */
      });
    /*page.onLoadFinished = function(status) {
      console.log('Rendering page to amazon_devices.png');
      page.render('amazon_devices.png');
      steps[4]();
    } */
};
// Make the calls to the book list web service - returns JSON
steps[5] = function() {
    // get the book list
    var bs = 50;
    var methods = [["DESCENDING", "DATE"]];
    console.log('Starting step 5')
    if(!latest) {
    	methods = [["DESCENDING", "DATE"], ["ASCENDING", "DATE"],
    	            ["DESCENDING", "TITLE"], ["ASCENDING", "TITLE"],
    	            ["DESCENDING", "AUTHOR"], ["ASCENDING", "AUTHOR"]];
    }
                   ;
    console.log('GETTING THE BOOK LIST');
    getbookbatch('booklist', 0, bs, [], methods);
};
// Last step exit phantom
steps[6] = function() {
    phantom.exit();
};

function uniq(a) {
// uniq - a function from stackoverflow uses orderDetailURL and the
// Amazon Stock Number as a unique identifier to dedup the book list when
// multiple calls are made.
  var seen = {};
  return a.filter(function(item) {
    return seen.hasOwnProperty(item.orderDetailURL+item.asin) ? false : (seen[item.orderDetailURL+item.asin] = true);
  });
}

function writecsv(fn, items) {
// writecsv - a function that will write out the relevant fields into a 
// csv. Takes the root of the filename and the book list array
  var out = '"Authors", "Title", "ASIN", "Order URL", "Product Image", "Acquired Date"\n';
  var val = [];
  var notfirst = false;
  for (it in items) {
    val = [];
    val[0] = items[it].authors;
    val[1] = items[it].title;
    val[2] = items[it].asin;
    val[3] = items[it].orderDetailURL;
    val[4] = items[it].productImage;
    val[5] = items[it].acquiredDate;
    notfirst = false;
    for(v in val) {
      val[v] = val[v].replace(/"/g, '""');
      if(notfirst) {
        out += ',';
      } else {
        notfirst = true;
      }
      out += '"' + val[v] + '"';
    }
    out += '\n';
  }
  fs.write(fn + '.csv', out, 'w');
}

/**
 * getbookbatch - The main routine to interact with the ajax-activity 
 * Amazon page. To deal with the asynchronous nature of the return 
 * function, this calls itself recursively to process the list in various 
 * orders and in suitable size batches
*
 * Arguments are:
* The file name root,
 * The offset of the batch - initial call should be 0,
 * the batch size something sensible - up to 100 seems to work
 * steps an embedded array of sort orders
 *
 * The routine will recurse until the ajax call returns no more items 
 * and the array of sort orders has been traversed. The list of items is 
 * deduped.
 * Once the recursion is complete, the contents of the JSON is written to 
 * a txt file and to the csv file.
*/

function getbookbatch(fn, start, bsize, books, steps) {
/* Recent changes to this web service call include cross site forgery 
   protection so code changed to fetch the token off of the devices page
   and add it to the call */
    var jss, js;
    var order = steps[0][0];
    var ind = steps[0][1];
    var settings = "data=" + encodeURIComponent(JSON.stringify({
          "param":{"OwnershipData":{"sortOrder": order,"sortIndex": ind,"startIndex": start, "batchSize":bsize,
          "contentType":"Ebook","itemStatus":["Active","Expired"],
          "excludeExpiredItemsFor":["KOLL","Purchase","Pottermore","FreeTrial","DeviceRegistration","ku","Sample"],
          "originType":["Purchase","PublicLibraryLending","PersonalLending","KOLL","RFFLending","Pottermore",
          "Rental","DeviceRegistration","FreeTrial","ku","Sample"],"isExtendedMYK":true}}}
        )) + "&csrfToken=" + encodeURIComponent(csrf);
    console.log("Fetching book list in " + order.toLowerCase() +
    		" " + ind.toLowerCase() + " order with batch start of " + start);
    page.customHeaders = {
        "Accept" : "application/json, text/plain, */*" };

    multiLoad = true;
    page.open('https://www.amazon.co.uk/mn/dcw/myx/ajax-activity', 'POST',
        settings, function(status) {
        if(status !== 'success') {
            console.log('Failed to load the web service: ' + status);
            phantom.exit();
        }

        jss = page.evaluate(function() {
            return document.body.innerHTML;
        });

        /* console.log(jss) */
        js = JSON.parse(jss);

        if((typeof(js.OwnershipData.success) !== 'undefined')&&
        		(js.OwnershipData.success === true)) {
            books.push.apply(books, js.OwnershipData.items);

// change to the web service call from amazon now fails if
// you try and move beyond 1000 rather than just returning
// no more data
// added explicit limit check
            if((js.OwnershipData.hasMoreItems)&&(start + bsize < 1000)) {
                getbookbatch(fn, start + bsize, bsize, books, steps);
            }
            else
            {
            	steps.shift();
            	if(steps.length > 0) {
            		books = uniq(books);
                    getbookbatch(fn, 0, bsize, books, steps);
            	}
            	else
            	{
            		books = uniq(books);
                    fs.write(fn + ".txt", JSON.stringify(books), 'w');
                    writecsv(fn, books);
                    console.log("Got book list successfully - look in " + fn + ".csv");
                    multiLoad = false;
            	}
            }
        } else {
        	console.log("Failed to get list of books from the web service");
        	phantom.exit();
        }
    });
}

function steprunner() {
// steprunner - the main loop that runs the functions in the steps array
// moves on when the page is loaded and multiload is not set.
    if((!loadInProgress) && (!multiLoad)) {
        steps[stepindex]();
        stepindex++;
    }
}

function runit() {
// runit - sets up steprunner to be called every 2 seconds
    setInterval(steprunner, 8000);
}

runit();
```

## API.JS
The javascript source for the Kindle API
```javascript
var KindleBookMetadata = function(title, authors, asin) {
    return {
        title: title,
        authors: authors,
        asin: asin
    };
};

var KindleBookProgress = function(positions, locs, page_nums) {
    return {
        positions: positions,
        locs: locs,
        page_nums: page_nums
    };
};

var KindleAPI = (function() {
    function _get_new_kmm(modules) {
        modules = modules || [];
        var kmm = KindleModuleManagerFactory();
// This attr makes sense seeing as BOOK_METADATA and BOOK_FRAGMAP made
// it into the KindleModuleManager.
        kmm.BOOK_CONTEXT = "book_context";
        function copy_registration(to, from, modules) {
            modules.forEach(function(a) {
                if (!to.isModuleRegistered(a) && from.isModuleInitialized(a)) {
                    to.registerModule(a, from.getModuleSync(a));
                }
            });
        }
        copy_registration(kmm, KindleModuleManager, modules);
        return kmm;
    }

    function _load_book_modules(asin, kmm) {
        kmm = kmm || _get_new_kmm();
        kmm.BOOK_CONTEXT = "book_context";
        // Detach any attached Book data leftover
        for (var module in [kmm.BOOK_FRAGMAP, kmm.BOOK_METADATA, kmm.BOOK_CONTEXT]) {
            if (kmm.isModuleRegistered(module)) {
                kmm.detachModule(module);
            }
        }

        var modules_ready = $.Deferred();
        kmm.getModuleSync(kmm.SERVICE_CLIENT)
            .startReading({asin: asin})  // Submit ajax request for book context
            .done(function(context) {  // Register modules using this context
                kmm.registerModule(kmm.BOOK_CONTEXT, context);
                var info = KindleReaderBookInfoProvider
                                        .BookInfo({asin: null}, kmm);

                var ncp = NetworkContentProvider.create({
                    context: context,
                    bookInfo: info,
                    asin: asin
                });
                // Register metadata and fragmap
                kmm.registerModuleWithDeferred(kmm.BOOK_METADATA, ncp.getMetadata());
                kmm.registerModuleWithDeferred(kmm.BOOK_FRAGMAP, ncp.getFragmap());
                // Return the three modules
                kmm.getModuleList([kmm.BOOK_CONTEXT, kmm.BOOK_METADATA, kmm.BOOK_FRAGMAP])
                    .done(function(mods) {
                        modules_ready.resolve(mods[kmm.BOOK_CONTEXT],
                                mods[kmm.BOOK_METADATA],
                                mods[kmm.BOOK_FRAGMAP]);
                    });
            });
        return modules_ready.promise();
    }

/*
 * Convert the larger metadata object returned by the AppDb to a
 * KindleBookMetadata object (just title, author list, and ASIN).
 */
function _from_db_book(db_book) {
  return KindleBookMetadata(db_book.title, db_book.authors, db_book.asin);
}

/*
 * ASYNC
 * Return an Array of KindleBookMetadata objects representing the 
 */
    function get_library_metadata() {
        var kmm = _get_new_kmm([Kindle.MODULE.DB_CLIENT]);
        var books_ready = $.Deferred();
        kmm.getModuleSync(Kindle.MODULE.DB_CLIENT)
            .getAppDb()
            .getAllBooks()
            .done(function(books) {
                books_ready.resolve($.map(books, _from_db_book));
            });
        return books_ready.promise();
    }

/*
 * ASYNC
 * Return the KindleBookMetadata object for the book associated with asin
 */
    function get_book_metadata(asin) {
        var kmm = _get_new_kmm([Kindle.MODULE.DB_CLIENT]);
        var book_ready = $.Deferred();
        kmm.getModuleSync(Kindle.MODULE.DB_CLIENT)
            .getAppDb()
            .getBook(asin, function(db_book) {
                book_ready.resolve(_from_db_book(db_book));
            });
        return book_ready;
    }

/*
 * ASYNC
 * Return the KindleBookProgress object for the book associated with asin
 */
    function get_book_progress(asin) {
        // A new ModuleManager is constructed for each call to this function
        // so that calls can be made concurrently (e.g. in
        // get_library_progress())
        var kmm = _get_new_kmm([Kindle.MODULE.DB_CLIENT,
                                Kindle.MODULE.SERVICE_CLIENT,
                                Kindle.MODULE.METRICS_MANAGER,
                                Kindle.MODULE.PageNumberManager]);
        var book_ready = $.Deferred();
        _load_book_modules(asin, kmm)
            .done(function(context, metadata) {
                var info = KindleReaderBookInfoProvider
                                        .BookInfo({asin: metadata.asin}, kmm);
                var current = info.getFurthestPositionReadData().position,
                    start = metadata.startPosition,
                    end = metadata.endPosition;
                var positions = [start, current, end];

                // Convert positions to Location and Page Number (if available)
                var locs_dfd = $.map(positions, info.getLocationConverter().locationFromPosition);
                var loc_conversions = $.when.apply($, locs_dfd);

                var page_conversions = $.Deferred();
                if (context.pageNumberUrl) {
                    info.getContext = function() { return context; };
                    var pgnum_dfd = kmm
                                .getModuleSync(Kindle.MODULE.PageNumberManager)
                                .getPageNumbers(info);
                    pgnum_dfd.done(function(pageConverter) {
                        var range_obj = pageConverter.getPageNumberRanges().arabic;
                        var range = [range_obj.minPage, range_obj.maxPage];
                        // Ensure position is within the valid page range
                        var position_range = $.map(range, pageConverter.positionFromPageNumber);
                        if (position_range[0] == -1 || position_range[1] == -1) {
                            // Page conversion error
                            page_conversions.resolve(void 0);
                        } else {
                            var corrected_position = (current < position_range[0]) ? position_range[0]
                                                    : (current > position_range[1]) ? position_range[1]
                                                    : current;
                            var curr_page = pageConverter.pageNumberFromPosition(corrected_position);
                            page_conversions.resolve([range_obj.minPage,
                                                        parseInt(curr_page),
                                                        range_obj.maxPage]);
                        }
                    });
                } else {
                    page_conversions.resolve(void 0);
                }
                $.when(loc_conversions, page_conversions)
                    .done(function (locs, page_nums) {
                        book_ready.resolve(KindleBookProgress(positions, locs, page_nums));
                    });
            });
        return book_ready.promise();
    }

/*
 * ASYNC
 * Return an object where each attribute is an ASIN from the user's
 * library and the value is the associated KindleBookProgress object.
 */
    function get_library_progress() {
        var books_ready = $.Deferred();
        get_library_metadata().done(function(books) {
            var asins = $.map(books, function(book) { return book.asin; });
            $.when.apply($, $.map(asins, get_book_progress)).done(function() {
                var progress_list = arguments;
                var ret = {};
                asins.map(function(asin, i) {
                    ret[asin] = progress_list[i];
                });
                books_ready.resolve(ret);
            });
        });
        return books_ready.promise();
    }
    return {
        get_book_progress: get_book_progress,
        get_library_progress: get_library_progress,
        get_book_metadata: get_book_metadata,
        get_library_metadata: get_library_metadata
    };
})();
```

## kindle.py
Source: my_kindle_stats/kindle.py at main · yihong0618/my_kindle_stats · GitHub
Usage: python3 kindle.py $'{amazon.com cookie}'
```python
from http.cookies import SimpleCookie
from datetime import datetime
import re

import requests
import argparse

KINDLE_BASE_URL = "https://www.amazon.com/kindle/reading/insights/"
KINDLE_HISTORY_URL = KINDLE_BASE_URL + "data"
KINDLE_SINGLE_BOOK_URL = KINDLE_BASE_URL + "titlesCompleted/{book_id}?isPDoc=true"
AMAZON_BOOK_URL = "https://www.amazon.com/dp/{book_id}"
KINDLE_HEADER = {
  "User-Agent": "Mozilla/5.0 (iPhone; CPU iPhone OS 14_6 like Mac OS X) "
  "AppleWebKit/605.1.15 (KHTML, like Gecko) Mobile/1AE148",
}
GITHUB_README_COMMENTS = (
  "(<!--START_SECTION:{name}-->\n)(.*)(<!--END_SECTION:{name}-->\n)"
)
KINDLE_HEAD_INFO = "## I have read {books_count} books this year\n\n"
KINDLE_TABLE_HEAD = "| ID | Title | Authors | Date | \n | ---- | ---- | ---- | ---- |\n"
KINDLE_STAT_TEMPLATE = "| {id} | {title} | {authors} | {date} |\n"

def replace_readme_comments(file_name, comment_str, comments_name):
    with open(file_name, "r+") as f:
        text = f.read()
        # regrex sub from github readme comments
        text = re.sub(
            GITHUB_README_COMMENTS.format(name=comments_name),
            r"\1{}\n\3".format(comment_str),
            text,
            flags=re.DOTALL,
        )
        f.seek(0)
        f.write(text)
        f.truncate()

class Kindle:
    def __init__(self, cookie, is_cn=True):
        self.kindle_cookie = cookie
        self.session = requests.Session()
        self.header = KINDLE_HEADER
        self.is_cn = is_cn
        self.KINDLE_URL = KINDLE_CN_HISTORY_URL if self.is_cn else KINDLE_HISTORY_URL
        self.KINDLE_BOOK_URL = (
            KINDLE_CN_SINGLE_BOOK_URL if self.is_cn else KINDLE_SINGLE_BOOK_URL
        )
        self.AMAZON_URL = AMAZON_CN_BOOK_URL if self.is_cn else AMAZON_BOOK_URL
        self.has_session = False

    def _parse_kindle_cookie(self):
        cookie = SimpleCookie()
        cookie.load(self.kindle_cookie)
        cookies_dict = {}
        cookiejar = None
        for key, morsel in cookie.items():
            cookies_dict[key] = morsel.value
            cookiejar = requests.utils.cookiejar_from_dict(
                cookies_dict, cookiejar=None, overwrite=True
            )
        return cookiejar

    def make_session(self):
        cookies = self._parse_kindle_cookie()
        if not cookies:
            raise Exception("Please make sure your amazon cookie is right")
        self.session.cookies = cookies
        self.has_session = True

    def get_kindle_read_data(self):
        if not self.has_session:
            self.make_session()
        r = self.session.get(self.KINDLE_URL, headers=self.header)
        return r.json()

    def get_single_read_book_info(self, book_id, is_doc):
        # format True -> true False -> false
        is_doc = ["false", "true"][is_doc]
        url = self.KINDLE_BOOK_URL.format(book_id=book_id, is_doc=is_doc)
        book_info = self.session.get(url, headers=self.header).json()
        if not book_info:
            print(f"There's no book info if id {book_id}")
        book_title = book_info["title"]
        slice_index = book_title.find("(")
        if slice_index == -1:
            slice_index = book_title.find("（")
        if slice_index != -1:
            book_title = book_title[:slice_index]
        book_title = book_title.replace(" ", "")
        if is_doc == "false":
            book_url = self.AMAZON_URL.format(book_id=book_id)
            book_title = f"[{book_title}]({book_url})"
        book_authors = book_info.get("authors")
        if len(book_authors) > 2:
            book_authors = ",".join(book_authors[:2]) + "..."
        else:
            book_authors = ",".join(book_authors) if book_authors else ""
        return book_title, book_authors

    def make_all_books_list(self):
        year = datetime.now().year
        self.make_session()
        year_books_info = self.get_kindle_read_data()
        titles_read = year_books_info.get("goal_info", {}).get("titles_read")
        if not titles_read:
            return
        result = []
        for title in titles_read:
            if int(title.get("date_read", "1926-08-17")[:4]) < year:
                break
            is_doc = title.get("content_type", "") == "PDOC"
            book_title, authors = self.get_single_read_book_info(
                title.get("asin"), is_doc
            )
            title["book_title"] = book_title
            title["authors"] = authors
            result.append(title)
        return result

    def make_kindle_string(self, book_list):
        books_count = len(book_list)
        s = KINDLE_HEAD_INFO.format(books_count=books_count)
        s += KINDLE_TABLE_HEAD
        index = 1
        for book in book_list:
            year = datetime.now().year
            s += KINDLE_STAT_TEMPLATE.format(
                id=str(index),
                title=book.get("book_title"),
                authors=book.get("authors"),
                date=str(book.get("date_read"))[:10],  # only keep date
            )
            index += 1
        return s

if __name__ == "__main__":
    parser = argparse.ArgumentParser()
    parser.add_argument("cookie", help="amazon or amazon cn cookie")
    parser.add_argument(
        "--is-cn",
        dest="is_cn",
        action="store_true",
        help="if amazon accout is cn",
    )
    options = parser.parse_args()
    kindle = Kindle(options.cookie, options.is_cn)
    book_list = kindle.make_all_books_list()
    s = kindle.make_kindle_string(book_list)
    replace_readme_comments("README.md", s, "my_kindle")
```

## [Lector](https://github.com/msuozzo/Lector.git)
Python bindings are provided in any language that can run the javascript found in api.py from within a Kindle Cloud Reader session may easily access this data.
Usage
```javascript
import lector
api = lector.KindleCloudReaderAPI('my_amazon_username', 'my_amazon_password')
my_library = api.get_library_metadata()
book = my_library[0]
book_progress = api.get_book_progress(book.asin)
_, current_page, last_page = book_progress.page_nums
print 'Currently reading %s (Page %d of %d)' % (book.title, current_page, last_page)
```

## PY4U.NET Kindle Cloud Reader Book List
**Source**: [Get Kindle Library Book List (py4u.net)]( https://www.py4u.net/discuss/1739096)  
FireFox has a built-in Inspector tool that can be used to rip the complete book list from the Amazon cloud reader Library page:  
-	go to https://read.amazon.com/ and display the Library page
-	call up the Inspector tool under Tools > Web Developer (Ctrl+Shift+C)
-	select the actual list part (div #titles_inner_wrapper), which is the immediate parent of all the book entries
-	in the HTML pane of the Inspector dashboard, copy the HTML for the selected part

Selecting the list part is most easily done by activating the element picker (Ctrl+Shift+C or left-most icon in the Inspector dashboard), hovering the mouse over the top left book icon and then moving it slowly up or left until the selection expands to the whole inner part of the list (at which point the selection caption will say div #titles_inner_wrapper). A left-click at this point selects the corresponding node in the HTML pane of the Inspector dashboard so that Ctrl+C will copy the HTML to the clipboard. This is the same as Copy > Outer HTML in the right-click menu for the node.  
This gives the full book list as an HTML fragment with an easily parsed structure, including ASIN:  
```html
<div id="titles_inner_wrapper" style="font-size: 191.25px;">
  <div id="B00DJI3HWS" class="book_container">
    <div class="book_cover">
      <img class="book_image book_click_area" src="https://images-na.ssl-images-amazon.com/images/P/B00DJI3HWS.01._SX255_SY255_TTXW_SCLZZZZZZZ_.jpg" title="I Bastardi di Pizzofalcone (Italian Edition)">
      <div class="alt_title book_click_area"></div>
    </div>
    <div class="book_details">
      <div class="book_title book_click_area">I Bastardi di Pizzofalcone (Italian Edition)</div>
      <div class="book_author book_click_area">Maurizio de Giovanni</div>
    </div>
  </div>
  ...
</div>
```
This manual procedure takes just a small handful of mouse clicks and key strokes.
Downloading the list programmatically is a little more involved than issuing an HTTP GET and dissecting the result, since the cloud reader requires authentication and uses a lot of JavaScript. Here's some proof-of-concept code for downloading + saving the list using the .NET [WebBrowser]( https://docs.microsoft.com/en-us/dotnet/api/system.windows.forms.webbrowser?view=netcore-3.1) control. The code can be compiled as a .cs file but it can also be pasted into LINQPad and run as is (see #ifdefs). It uses a visible browser control on a form because it may be necessary to log in to the cloud reader.
You should review/modify the filename template before running this script.
```javascript
class KindleBookListProgram 
{
    const string FILENAME_TEMPLATE = "D:\kindle_library_{0:yyyyMMdd}.lst";  // gets DateTime.Now as parameter
    const string READ_AMAZON_COM = "https://read.amazon.com/";
    const string USERAGENT = "Mozilla/5.0 (Windows NT 6.3; WOW64; Trident/7.0; rv:11.0) like Gecko";
    const int URLMON_OPTION_USERAGENT = 0x10000001;

    static void Main ()
    {
// setting the user agent in the Navigate() call works only once;
// this works for the whole session
        UrlMkSetSessionOption(URLMON_OPTION_USERAGENT, USERAGENT, USERAGENT.Length, 0);
        using (var form = new BrowserForm())
        {
            form.ShowDialog();
        }
    }
    [DllImport("urlmon.dll", CharSet = CharSet.Ansi)]
    private static extern int UrlMkSetSessionOption (
        int dwOption, string pBuffer, int dwBufferLength, int dwReserved );

    class BrowserForm: Form
    {
        WebBrowser m_browser;
        public BrowserForm ()
        {   
            Width = 800;
            Height = 600;
            m_browser = new WebBrowser();
            m_browser.DocumentCompleted += handle_browser_DocumentCompleted;
            m_browser.Dock = DockStyle.Fill;
            Controls.Add(m_browser);
    
            KeyPreview = true;
            KeyDown += handle_KeyDown;
    
            m_browser.Navigate(READ_AMAZON_COM);
        }

        void find_and_save_book_list_frame (WebBrowser browser)
        {
            foreach (HtmlWindow frame in browser.Document.Window.Frames)
            {
                var elt = frame.Document.GetElementById("titles_inner_wrapper");
                if (elt != null)
                {
                    var text = elt.InnerHtml;
                    if (string.IsNullOrEmpty(text))
                    {
                        this.Text = "Book list is empty!";
#if LINQPAD
                        Console.WriteLine("{0} book list empty!
", DateTime.Now);
#endif
                    }
                    else
                    {
                        var filename = string.Format(FILENAME_TEMPLATE, DateTime.Now);
#if LINQPAD
                        Console.WriteLine("##### {0} ######

{1}

", filename, text);
#endif
                        File.WriteAllText(filename, text, Encoding.UTF8);
                        this.Text = filename + " saved!";
                    }
                }
            }
        }

        void handle_browser_DocumentCompleted (object sender, WebBrowserDocumentCompletedEventArgs e)
        {
            find_and_save_book_list_frame(sender as WebBrowser);
        }

        void handle_KeyDown (object sender, KeyEventArgs e)
        {
            if (e.Control && e.KeyValue == 17)  // ^S
            {
                e.SuppressKeyPress = true;
                find_and_save_book_list_frame(m_browser);
            }   
        }
    }
}
```
This little script loads the cloud reader and saves the book list if it finds it when the DocumentCompleted event fires (i.e. when the browser thinks it is done loading). The list save code can be invoked manually via the ^S hotkey (a.k.a. Ctrl+S), in case the DocumentCompleted event fires before the book list is actually loaded by the JavaScript.  
Note: the automatic event-based saving is likely to result in incomplete lists, so it is better to always save manually once the dust has settled. Or set a generous timer in the DocumentCompleted event so that automatic saving is only tried after the dust has settled somewhat, and do the actual saving only if the result has proved stable for several seconds. I've posted a [version of the code]( https://pastebin.com/htmP7q57) that does this at PasteBin.  
      
