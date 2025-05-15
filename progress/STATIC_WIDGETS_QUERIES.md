### Static Widgets and Static Query
#### Widgets 
- A Widget is a visual element of a user interface.
- for example, a widget might be an image, a menu, a frame, a window, or a fill-in field.
- Widgets are often called user interface components (UICs) or controls.

#### Types of Widgets 
- Windows
- Frames
- Dialog boxes
- Browse
- Fill-in fields, text, sliders, selection lists, combo boxes, toggle boxes, radio sets and editors for data representation.
- Buttons, images, rectangles, and literals
- menus, submenus, menu bars and menu items.

#### Widgets Hierarchy
- Widgets exist in a hierarchy consisting of 
- **Container Widgets**
- A Container widget can contain one or more other widgets. The basic container widget of a user interface is the window.
- **Atomic widgets**
- An atomic widget represents data or other visual information but does not contain any other widgetss.
- Most container and atomic widgets can have a menu widget (a type of container widget) associated with them, and menu atomic widgets can have an associated literal widget that represents a side label.

#### Representing Data in widgets
- Database fields and progress procedure variables can be represented on the display in a variety of ways as 
- Text
- Fill-ins
- Sliders
- Selection lists
- Combo boxes
- Toggle boxes
- Radio Sets
- Editors

- **Fill-ins**
```
DEFINE VARIABLE filename AS CHARACTER FORMAT "X(80)" VIEW-AS FILL-IN NATIVE SIZE 42 BY 1 TOOLTIP "Enter the number".
DEFINE VARIABLE no-scroll AS CHARACTER.
DEFINE FRAME f
    filename no-scroll.
UPDATE filename no-scroll WITH FRAME f.
/*
NATIVE - after enter the value then it will show in different color.
SIZE 42 BY 1 -> 42 - width and 1 height
TOOLTIP - same as title in html
*/
```
- **Text** 
- its mainly used for read-only purpose.
```
DEFINE VARIABLE test AS CHARACTER NO-UNDO INITIAL "Welcome to Progress KT" FORMAT "X(30)".
DISPLAY test VIEW-AS TEXT FORMAT "X(70)" LABEL "Overall Message:" WITH FRAME d.
```
- **Sliders**
- A slider is a rectangular scale that represents a range of INTEGER values.
- A pointer that resides on the track bar within the scale indicates the current value. The current value is also displayed in character format near the scale. Only values of type INTEGER can be viewed as sliders.
- Syntax :
```
VIEW-AS SLIDER
MAX-VALUE max-value MIN-VALUE min-value
[HORIZONTAL | VERTICAL]
[NO-CURRENT-VALUE]
[LARGE-TO-SMALL]
[TIC-MARKS{NONE | TOP | BOTTOM | LEFT | RIGHT | BOTH}]
[FREQUENCY n]
[TOOLTIP tooltip]
[size-phrase]
```
- Example
```
DEFINE VARIABLE num-rows AS INTEGER LABEL "Number of Rows" VIEW-AS SLIDER MIN-VALUE 1 MAX-VALUE 20 TIC-MARKS BOTTOM FREQUENCY 1.
DEFINE BUTTON show-custs LABEL "Show Customers".
FORM
num-rows HELP "Choose the number of customers per screen."
show-custs
WITH FRAME main-frame THREE-D.
DEFINE FRAME cust-frame customer.cust-num customer.name WITH DOWN THREE-D.
ON CHOOSE OF show-custs
DO:
CLEAR FRAME cust-frame ALL.
FRAME cust-frame:DOWN = INTEGER(num-rows:SCREEN-VALUE IN FRAME main-frame).
FOR EACH customer:
DISPLAY cust-name name WITH FRAME cust-frame.
DOWN WITH FRAME cust-frame.
END.
HIDE FRAME cust-frame.
END.
num-rows:MAX-VALUE IN FRAME main-frame = SCREEN-LINES - 2.
ENABLE ALL WITH FRAME main-frame.
WAIT-FOR WINDOW-CLOSE OF CURRENT-WINDOW.
```
- **Toggle Boxes**
- A Toggle box, Sometimes called a check box, represents a logical value. The Presence (true) or absence (false) of filling in the toggle box indicates the current value.
- In a graphical interface, a toggle box and its filling style take on the native look of the windowing system.
- we can change a toggle value by positioning to it and pressing SPACEBAR. In a graphical interface, we can point to the box and click the mouse SELECT button.
- Syntax :
```
VIEW-AS TOGGLE-BOX [size-phrase] [TOOLTIP tooltip]
```

- **Radio Sets**
- A radio set is a series of buttons (radio items) that represent a single field or variable. When one button is passed, any previously selected button pops out.
- Each of the buttons represents a differnt value for the field or variable. At any one time, exactly one of the buttons is chosen. If the user chooses a different button, the previously chosen button is automatically deselected.
- A radio set can represent a LOGICAL, CHARACTER, INTEGER, DECIMAL, or DATE value. Radio sets are appropriate for fields that can have only one of a limited number of values.
- Syntax :
```
VIEW-AS RADIO-SET
[HORIZONTAL [EXPAND] | VERTICAL]
[size-phrase] RADIO-BUTTONS label, value
[,label, value] ...
[TOOLTIP tooltip]
```
- Example : 
```
DEFINE VARIABLE pay-stat AS INTEGER INITIAL 1 LABEL "Pay Status" VIEW-AS RADIO-SET RADIO-BUTTONS "Unpaid", 1, "Partially Paid", 2, "Paid in full", 3.
FORM
pay-stat VIEW-AS TEXT WITH FRAME out-frame.
ON GO OF FRAME in-frame
DO:
ASSIGN pay-stat.
DISPLAY pay-stat WITH FRAME out-frame.
END.
DISPLAY pay-stat WITH FRAME in-frame.
ENABLE pay-stat WITH FRAME in-frame.
STATUS INPUT "Select a pay status and GO".
WAIT-FOR WINDOW-CLOSE OF CURRENT-WINDOW.
```

- **Editors**
- An editor allows full editing of large characters fields. The editor widget supports features such as cut, copy, paste, and word-wrap.
- Syntax :
```
VIEW-AS EDITOR
{
    size-phrase | INNER-CHARS char INNER-LINES lines
}
[BUFFER-CHARS chars]
[BUFFER-LINES lines]
[LARGE]
[MAX-CHARS characters]
[NO-WORD-WRAP]
[SCROLLBAR-VERTICAL]
[TOOLTIP tooltip]
```
- Example 
```
FORM
item.item-num item-name item.price on-hand allocated re-order on-order cat-page item.cat-description VIEW-AS EDITOR SIZE-CHARS 30 BY 5 WITH FRAME x SIDE-LABELS.
FOR EACH item:
UPDATE item WITH FRAME x.
END.
```
