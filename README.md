# Kindle-Book-List

This is a project to collect information from Amazon related to purchased books from the Kindle Cloud Library. All the books added to Kindle account are stored on Amazon servers, not on the particular device. Access your Kindle Cloud Library by loggin into your Amazon account then select Manage Your Content and Devices. 

Access the Library: https://read.amazon.com/kindle-library

Note:
- Kindle Cloud Reader offers very basic features: highlights, notes, or search within a book.  
- You can read your books in the browser, creating notes & highlights, just like using your Kindle Device.  
- You can copy your notes & highlights within the web browser for all synced books.
Data in the Kindle Cloud Library is stored in a JSON format.  

# Related GISTS
- https://gist.github.com/jkubecki  
- https://gist.github.com/usayamadx  

# Acronyms
| Acronym | Description |  
| ----------- | ----------- |
| ASIN    | Amazon Store Identification Number; Amazon Standard Identification Number |  
| csrs    | ??? |  
| CSV     | Comma Separated Values (a Database) |  
| HTML    | HyperText Markup Language |  
| JSON    | JavaScript Object Notation (a Database) |  
| K4PC    | Kindle for Personal Computers |  
| K4W     | Kindle for Windows |  
| XML     | eXtensible Markup Language (a Database) |   

# Build the Query
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

> Select one item within {} for your query. Not all elements are required in the query. I haven't tested all possible queries, but the first 5 in the list do work well.  

# Data Elements (Fields)
| Element | Description |  
| ----------- | ----------- |  
| authors[0] | List of Authors | 
| title | The Title of the Book | 
| asin | Amazon's Unique ID Number for the Book | 
| webReaderUrl |  bookmark to the web Reader | 
| productUrl | a link to cover images | 
| percentageRead | a guestimate of how much you've read of the book.  | 
| resourceType | (EBOOK or EBOOK_SAMPLE) | 
| originType | (PRIME and PURCHASE) | 
| orderDetailURL | URL To your Purchase Order for the Book | 
| productImage | URL to the Book cover | 
| acquiredDate | Purchase Date | 

# Additional Possible Fields from the product page:
| Element | Description |  
| ----------- | ----------- |  
| sales_rank Sales | ranking of the book (Date Specific) |  
| rating | Rating of the Book (Date Specific) |  
| number_of_ratings | Number of Ratings for the Book (Date Specific) |  

# Alternate Sources of Book List
* KindleSyncMetadataCache.xml (a Kindle for PC database)
* Kindle Cloud Reader HTML file.

# KindleSyncMetadataCache.xml File Sample Format:
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

> **Note**: This file has different information available as compared to the Kindle Cloud Library. a conversion from XML to CSV would be necessary.

# Kindle Cloud Reader HTML file format example.

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

> **Note:** Wait for all of your books to be loaded into the browser window, then view it's source and copy the DIV section that start with ID of titles_inner_wrapper.   
This isn't the best choice as clean up of the HTML will take so much more time, and doesn't lend itself to a simple parsing program.


# Using ExportKindle.js
First, modify the query to support your book list requirements based on the query options available above. 

    let url = domain + 'kindle-library/search?query=&libraryType=BOOKS' + ( paginationToken ? '&paginationToken=' + paginationToken : '' ) + '&sortType=recency&querySize=50'

Second Modify these two line to reflect your data form the data elements to be collected in the CSV creation (This example has ASIN, Title, & Authors):

    let csvData = "ASIN,Title,Author(s)\n"
    csvData += item.asin + '","' + item.title + '","' + item.authors + '\n'

Make sure you put the '\n' at the end or all items will be on one row following the header created in the first line.  
Go to your Kindle Cloud Reader in Chrome, or FireFox. Open the Developer Console. Paste the code block in ad then select RUN.  A dialog box will open when the download completes so you can save or open the new CSV file. Go check out your saved CSV file in the Downloads folder of the current user!  

I've included a test run file that I created for my 17000+ book Library.  Note, only the first 10000 books were gathered. This appears to be a limitation of the AMAZON data exchange, not the script.