### Event-Driven Programming example :
```
DEFINE VARIABLE iNumber AS INT64 NO-UNDO.
DEFINE BUTTON btn_Next LABEL "Next".
DEFINE BUTTON btn_Prev LABEL "Prev".

DEFINE FRAME fMain
    SKIP(2)
    customer.cust-num AT 10 SKIP
    customer.name AT 14 SKIP
    customer.phone AT 13 SKIP(2)
    customer.city AT 12 SKIP(2)
    btn_Next AT 10 SKIP(2)
    btn_Prev
    WITH SIDE-LABELS CENTERED WIDTH 50 TITLE "Update Customer Phone Number ".

ON CHOOSE OF btn_Next IN FRAME fMain
DO:
    FIND NEXT customer EXCLUSIVE-LOCK NO-ERROR.
    IF NOT AVAILABLE customer THEN
        FIND FIRST customer EXCLUSIVE-LOCK NO-ERROR.
    DISPLAY cust-num name phone WITH FRAME fMain.
END.

ON CHOOSE OF btn_Prev IN FRAME fMain
DO:
    FIND PREV customer EXCLUSIVE-LOCK NO-ERROR.
    IF NOT AVAILABLE customer THEN
        FIND LAST customer EXCLUSIVE-LOCK NO-ERROR.
    DISPLAY cust-num name phone WITH FRAME fMain.
END.

ON ENTER, RETURN, GO OF customer.phone IN FRAME fMain DO:
    IF TRIM(INPUT phone) = "" THEN DO:
        MESSAGE "Entered phone number is blank" VIEW-AS ALERT-BOX.
        APPLY "ENTRY" TO customer.phone IN FRAME fMain.
        RETURN NO-APPLY.
    END.
    ELSE DO:
        ASSIGN iNumber = INT64(INPUT phone) NO-ERROR.
        IF ERROR-STATUS:ERROR THEN DO:
            MESSAGE "Invalid Number : " ERROR-STATUS:GET-MESSAGE(1) VIEW-AS ALERT-BOX.
            APPLY "ENTRY" TO customer.phone IN FRAME fMain.
            RETURN NO-APPLY.
        END.
    END.
    ASSIGN customer.phone.
END.

main:
DO ON ERROR UNDO, LEAVE main:
    FIND FIRST customer EXCLUSIVE-LOCK NO-ERROR.
    IF AVAILABLE customer THEN DO:
        DISPLAY cust-num name phone city WITH FRAME fMain.
        ENABLE customer.phone city btn_Next btn_Prev WITH FRAME fMain.
    END.
    WAIT-FOR CLOSE OF THIS-PROCEDURE.
END.
```

### PROCEDURAL PROGRAMMING Example :
```
DEFINE VARIABLE iNumber as INT64 NO-UNDO.
main:
DO ON ERROR UNDO, LEAVE main:
    FIND FIRST customer EXCLUSIVE-LOCK NO-ERROR.
    IF AVAILABLE customer THEN DO:
        DISPLAY cust-num name phone.
        PROMPT-FOR phone.

        IF TRIM(INPUT phone) = "" THEN DO:
            MESSAGE "Entered phone number is blank" VIEW-AS ALERT-BOX.
            RETURN.
        END.
        ELSE DO:
            ASSIGN iNumber = INT64(INPUT phone) NO-ERROR.
            IF ERROR-STATUS:ERROR THEN DO:
                MESSAGE ERROR-STATUS:GET-MESSAGE(1) VIEW-AS ALERT-BOX.
                RETURN.
            END.
        END.
        ASSIGN phone.
        MESSAGE "Phone number updated" VIEW-AS ALERT-BOX.
    END.
END.
```

### Internal Procedure Example :

```
main:
DO WHILE TRUE ON ERROR UNDO, LEAVE main:
    PROMPT-FOR customer.cust-num WITH FRAME fMain. /* This by default defines a Frame with name "fMain" */
    RUN pDisplayCustomer(INPUT (INPUT customer.cust-num)).
END.

PROCEDURE pDisplayCustomer:
    DEFINE INPUT PARAMETER ipiCustNum AS INTEGER NO-UNDO.

    FIND FIRST customer NO-LOCK WHERE customer.cust-num = ipiCustNum NO-ERROR.
    IF NOT AVAILABLE customer THEN
    DO:
        CLEAR FRAME fMain.
        MESSAGE "Invalid Customer " VIEW-AS ALERT-BOX.
        RETURN.
    END.
    DISPLAY WITH 2 COLUMNS FRAME fMain.
END PROCEDURE.
```

### Super Procedure Example :
```
/* Super-Procedure-Main.p */
DEFINE VARIABLE hHandle AS HANDLE.
DEFINE VARIABLE cVal AS CHARACTER.

FUNCTION fnFunction RETURNS CHARACTER (INPUT-OUTPUT cVal AS CHARACTER) IN hHandle.

RUN Super-Procedure-Driver.p PERSISTENT SET hHandle.
RUN pProcedure IN hHandle(INPUT-OUTPUT cVal).
MESSAGE cVal VIEW-AS ALERT-BOX.
ASSIGN cVal = "".
MESSAGE fnFunction(cVal) VIEW-AS ALERT-BOX.

/* Super-Procedure-SuperProc.p */ 
DEFINE VARIABLE hHandle AS HANDLE.

FUNCTION GetPartName RETURNS CHARACTER () IN hHandle.

PROCEDURE pProcedure:
    DEFINE INPUT-OUTPUT PARAMETER cVal AS CHARACTER.

    hHandle = TARGET-PROCEDURE.
    cVal = cVal + GetPartName().

    MESSAGE "TARGET-PROCEDURE is:" TARGET-PROCEDURE:FILE-NAME VIEW-AS ALERT-BOX.
    MESSAGE "SOURCE-PROCEDURE is:" SOURCE-PROCEDURE:FILE-NAME VIEW-AS ALERT-BOX.
END PROCEDURE.

FUNCTION fnFunction RETURNS CHARACTER (INPUT-OUTPUT cVal AS CHARACTER):
   hHandle = TARGET-PROCEDURE.
   cVal = cVal + GetPartName().
   RETURN cVal.
END.

/* Super-Procedure-Driver.p */ 
FUNCTION SetPartName RETURNS INTEGER (INPUT cVal AS CHARACTER) FORWARD.
DEFINE VARIABLE hHandle AS HANDLE.
DEFINE VARIABLE localPartName AS CHARACTER.

RUN Super-Procedure-SuperProc.p PERSISTENT SET hHandle.
THIS-PROCEDURE:ADD-SUPER-PROCEDURE (hHandle).
SetPartName("Hello").

PROCEDURE pProcedure:
    DEFINE INPUT-OUTPUT PARAMETER cVal AS CHARACTER.
    cVal = cVal + "proc: Part name is: ".
    RUN SUPER (INPUT-OUTPUT cVal).
END PROCEDURE.

FUNCTION fnFunction RETURNS CHARACTER (INPUT-OUTPUT cVal AS CHARACTER):
    cVal = cVal + "func: Part name is: ".
    SUPER (INPUT-OUTPUT cVal).
    RETURN cVal.
END FUNCTION.

FUNCTION GetPartName RETURNS CHARACTER ():
    RETURN localPartName.
END FUNCTION.

FUNCTION SetPartName RETURNS INTEGER (INPUT partname AS CHARACTER):
    localPartName = partname.
END FUNCTION.

```

### Functions
- Function is a sub-program same like procedure but it can return only a single value.
- we cannot use any prompt-for or update statements. Rather we use them for any input validations or other logic.
- FILL("r",20) -> RETURNS CHARACTER WHICH REPEATS r 20 TIMES.
- LEFT-TRIM("    HL") -> HL RETURNS CHARACTERS WITHOUT LEADING SPACES.
- R-INDEX("HELLO WORLD!", "!") -> SEARCHES FOR ! FROM RIGHT TO LEFT BUT RETURNS INDEX FROM THE START. (12)
- RIGHT-TRIM("HEL    ") -> REMOVES TAILING SPACES AND RETURNS CHARACTERS. (HEL)
- TRIM("   HEL   ") ->  REMOVES TAILING AND LEADING SPACES AND RETURNS CHARACTERS. (HEL)
- INDEX - RETURNS INTEGER
- LENGTH - RETURNS INTEGER
- REPLACE - RETURNS CHARACTER.
- SUBSTRING - RETURNS CHARACTER
- ENTRY -  ENTRY(4, "A B C D", " ") -> 4 -> INDEX ,  "A B C D" -> THAT INDEX VALUE IS FETCHED FROM THIS STRING AND BY DEFAULT THE DELIMITER WILL BE ',' BUT IF WE WANT OTHER DELIMETERS THEN SPECIFY THEM IN THIRD PARAMETER. RETURNS CHARACTER.
- LOOKUP - RETURNS AN INTEGER : INDEX OF THE SPECIFIED CHAR -> LOOKUP("A", "A:B:C:D",":"). -> 1 (OUTPUT VALUE).
#### ARITHMETIC FUNCTIONS 
- ABSOLUTE
- MAXIMUM
- MINIMUM
- EXP
- LOG
- MODULO
- RANDOM
- ROUND
- SQRT
- TRUNCATE
