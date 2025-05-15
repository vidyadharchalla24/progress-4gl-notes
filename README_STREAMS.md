### Input and Output Streams
- The purpose of this is to read data from a file or export data to a file.
- Syntax : 
```
DEFINE STREAM Stream_Name.

INPUT STREAM Stream_Name FROM FILE_NAME.
IMPORT STREAM Stream_Name delimiter "," Variable/Table.
INPUT STREAM Stream_Name CLOSE.

OUTPUT STREAM Stream_Name TO FILE_NAME.
EXPORT STREAM Stream_Name delimiter "," Variable/Table.
OUTPUT STREAM Stream_Name CLOSE.
```

- ExAMPLE : 
```
DEFINE STREAM stExport.
OUTPUT STREAM stExport TO "/stExport.csv".
EXPORT STREAM stExport delimiter "," "ABC" "123".
OUTPUT STREAM stExport CLOSE.
```
- here if we try to run this then it will generate stExport.csv and if run again either it is going to overwrite it or generate an new file. If we want to just append data to the existing file then use below command : 

```
DEFINE STREAM stExport.
OUTPUT STREAM stExport TO "/stExport.csv" APPEND.
EXPORT STREAM stExport delimiter "," "ABC" "123".
OUTPUT STREAM stExport CLOSE.
```
- HERE EXPORT: will add the content to new line but if we want in same row side by side without new line then we have to use PUT.
```
DEFINE STREAM stExport.
OUTPUT STREAM stExport TO "/stExport.csv" APPEND.
PUT STREAM stExport "ABC" "," "123".
PUT STREAM stExport "ABC" "," "123".
OUTPUT STREAM stExport CLOSE.
/* ABC,123ABC,123 */
```

- READING FROM FILE
```
DEFINE STREAM stExportRead.
INPUT STREAM stExportRead FROM "/stExport.csv".

DEFINE VARIABLE cchar1 as CHARACTER NO-UNDO.
DEFINE VARIABLE cchar2 as CHARACTER NO-UNDO.

REPEAT:
    IMPORT STREAM stExportRead DELIMETER "," cchar1 cchar2.
    DISPLAY cchar1 cchar2.
END.
INPUT STREAM stExportRead CLOSE.
```