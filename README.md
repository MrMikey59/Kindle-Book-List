# Kindle-Book-List

This is a project to collect information from Amazon related to purchased books from the Kindle Cloud Library.

Access the Library: https://read.amazon.com/kindle-library

Note:

- Kindle Cloud Reader offers very basic features: highlights, notes, or search within a book.

- You can read your books in the browser, creating notes & highlights, just like using your Kindle Device.

- You can copy your notes & highlights within the web browser for all synced books.
Data in the Kindle Cloud Library is stored in a JSON format.

# Acronyms

ASIN    Amazon Store Identification Number

CSV     Comma Separated Values (a Database)

JSON    JavaScript Object Notation (a Database)

K4PC    Kindle for Personal Computers

K4W     Kindle for Windows

XML     eXtensible Markup Language (a Database)

# Build the Query

sortOrder { DESCENDING | ASCENDING }

sortIndex { DATE | TITLE | AUTHOR }

startIndex <StartIndex> ( 0, then multiples of batchSize)

batchSize <bsize> ( Max 50 )

contentType Ebook

itemStatus { Active | Expired }

excludeExpiredItemsFor { KOLL | Purchase | Pottermore | FreeTrial | DeviceRegistration | ku | Sample }

originType { Purchase | PublicLibraryLending | PersonalLending | KOLL | RFFLending | Pottermore | Rental | DeviceRegistration | FreeTrial | ku | Sample | 
isExtendedMYK { True | False }

csrfToken encodeURIComponent(csrf)

Select one item within {} for your query. Not all elements are required in the query.


# Data Elements (Fields)

authors List of Authors

title The Title of the Book

asin Amazon's Unique ID Number for the Book

orderDetailURL URL To your Purchase Order for the Book

productImage URL to the Book cover

acquiredDate Purchase Date

# Additional Possible Fields from the product page:

sales_rank Sales ranking of the book (Date Specific)

rating Rating of the Book (Date Specific)

number_of_ratings Number of Ratings for the Book (Date Specific)

# Alternate Sources of Book List
KindleSyncMetadataCache.xml (a Kindle for PC database)
Kindle Cloud Reader HTML file.

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

Note: This file has different information available as compared to the Kindle Cloud Library. a conversion from XML to CSV would be necessary.

# Kindle Cloud Reader HTML file format example.

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

</div>

Note: Wait for all of your books to be loaded into the browser window, then view it's source and copy the section that start twith ID of titles_inner_wrapper.


