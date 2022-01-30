# XML Parser

This folder contains files used to test XML Parsing of the KindleSyncMetadataCache.xml file.

### Convert the XML to CSV
1.	Initialize the CSVHolder with Table Headers
2.	Create the XML Object, Open the File & Read the File.
3.	Set root to book elements
4.	Loop through XMLHolder til EOF
a.	Pull out ASIN, TITLE, AUTHORS & DatePurchased.
b.	Write each to new CSVHolder
5.	Save CSVHolder to File
6.	End

### My Untested Program
```javascript
Dim CSVHolder, XMLFile, objXMLDoc, CSVFileName
Dim objFSO, outFile 

' Initialize the CSVHolder with Table Headers
CSVHolder = "ASIN,Title,Author(s),Purchase Date/n"
CSVFileName = "D:\AmazonBooksList" & Format(Now(). "YYYY-MM-DD") & ".csv"
XMLFile = "D:\KindleSyncMetadataCache.xml" ' Set to proper Location!
' Create the XL Object and load the XML Data
Set objXMLDoc = CreateObject("msxml2.DOMDocument.6.0")
objXMLDoc.async = False
objXMLDoc.load XMLFile
MsgBox objXMLDoc.xml  ' Test Check

' Loop through XML Data to create CSVHolder
Dim Book, Books, ASIN, Title, Author, DatePurchased
' 
Set objRoot = objXMLDoc.documentElement
' Select all books in the Document
Set Books = objRoot.getElementsByTagName("book")
For Each Book in Books {
  ASIN          = Book.childNodes(0).text
  Title         = Book.childNodes(1).text
  Author        = Book.childNodes(2).text  ' Formatting Required
  DatePurchased = Book.childNodes(3).text
  CSVHolder = CSVHolder + ASIN + ','+ Title + ','+ Authors + ','+ _
              DatePurchased + vbCRLF
}
Set objXMLDoc = Nothing

' Save CSVHolder to file
Set objFSO = CreateObject("Scripting.FileSystemObject")
Set outFile = objFSO.CreateTextFile(CSVFileName, True) outFile.WriteLine(CSVHolder)
outFile.Close
Set objFSO = Nothing
' Done!
```

