# Random Project Code

## Building a PDF Data Form Extractor

```
import PyPDF2 as p2

filename = input("Enter the name of your WRP: ")

PDFfile = open(filename,"rb")
pdfread = p2.PdfFileReader(PDFfile)

fen = pdfread.getFormTextFields()
print(fen)
```

## Bulding a XXXXXX

