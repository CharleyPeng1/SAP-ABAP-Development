## SAPscript

SAPscript is used to specify the look and feel of business documents on a piece of paper.  
SAPscript is language dependant. SAPscript is client dependant.  

### Print program

Print program is responsible for retrieving the data from the SAP system and for the control logic of the output.  

### Composer

Composer formats lines and executes script commands.  
The composer is sent data from the print program and the script.  

### User SAPscript settings
                       
SE71, Settings, Form Painter, Graphical Editor and Painter Option.  

### Create SAPscript form
                     
SAPscript forms are never changed on the original 000 client.  
Instead, copy the SAPscript from the client 000 and then change it.  

### Copy SAPscript from another client
                                  
SE71, Utilities, Copy from Client, select a SAPscript and choose a new Z target form name.  

### SAPscript elements
                  
SAPscript elements are header, layout, paragraph format and character format (there are others as well but they are not as important).  

### SAPscript header
                
Header holds administrative data and basic settings.  

### SAPscript layout
                
Layout defines the look of SAPscript.  
Layout defines what types of pages will be printed and what the pages contain.  
Windows define where the data will be printed.  

### SAPscript layout windows
                        
Windows have different types.  
The most important window type is MAIN.  
MAIN is filled with data after the composer has processed all static windows.  
The position of the window within the layout can be set and changed.  
Each window has static texts and variables that will be printed.  
The contents of each window can be seen with the Line Editor.  

### Window Line Editor
                  
Line editor has two columns.  
The first column defines the format and the second column defines the data.  
There are special format characters.  
Text can be structured into text elements.  
Variables within text are marked with &_var&.  
A valid variable name is a structure defined within the print program.  
Variables can be formatted.  
Some system symbols can also be embedded within the text.  
SAPscript commands also exist and can be written with the text.  

### Paragraph format
                
Paragraph format are elements which allow texts to be formatted at the level of text lines.  
The most important paragraph format elements are fonts and tabs. Tab is marked within text with ',,'.  

### Character format
                
Character format is used to change the way a line is printed.  
Character format's most important feature is the font type.  
Font is defined with <_char_format>_text</>.  

### Standard texts
              
Standard text is a text that is often used in multiple SAPscripts such as header or footer information.  
They are defined in SO10.  
Each text has a Z name, ID and language.  
Standard text is formatted in the same way as a SAPscript form.  

### SAPscript graphics
                  
SAPscript graphics should be stored on a document server.  
Graphics must be imported before they are used.  

### SAPscript styles
                
Styles define how a group of standard texts or SAPscript forms look.  
Style has a header, paragraph format and character format.  

### Symbols and control commands
                            
Symbols are variables that will be replaced upon execution with actual text.  
They are inserted in the Line Editor with Insert, Symbols.  
There are system, standard, program and text symbols.  

### Print program construction
                          
Print programs are made up of function module invocations.  
OPEN_FORM and CLOSE_FORM must be used as they open the form for printing.  
START_FORM and END_FORM are used when more forms will be combined into one spool request.  
WRITE_FORM controls text element printing.  
WRITE_FORM has the following parameters: element specifies which text element will be printed, function says how it will be printed (append, replace, delete), type says where in the window will it be printed and window says in which window it will be printed.  

### SAPscript customizing
                     
Customizing is done in transaction SPRO.  
Customizing SAPscript is done in many places, but it is done most commonly is SD, MM and FI (Sales and Distribution, Material Management and Finance).  
Options are located inside the Output Control node.  


### Special format characters

| Character | Description |
|---|---|
| # | predefine header |
| / | new line |
| = | continue previous line |
| /: | SAPscript command line |
| /# | comment |
| /E | text element |

### Variable format

| Variable | Description |
|---|---|
| &table field(n)& | write the first n characters |
| &table field(.1)& | write the number with 1 decimal place |
| &table field(Z)& | leave out leading zeroes |
| &table field(S)& | leave out the sign (+,  ) |
| &table field(<)& | write the sign to the left |
| &table field(>)& | write the sign to the right |

### System symbols

| Symbol | Description |
|---|---|
| &DATE& | system date |
| &TIME& | system time |
| &PAGE& | current page |
| &ULINE(X)& | write out a x character long horizontal line |
| &VLINE(Y)& | write out a x character long vertical line |

### SAPscript commands

| Command | Description |
|---|---|
| NEW PAGE | force a page break |
| PROTECT, END PROTECT | data shouldn't be split among two pages |
| INCLUDE | for including standard texts during printing |
| IF (ELSE) ENDIF | conditional block |
| BOX, POSITION, SIZE | frame printing |

### Control commands

| Command | Description |
|---|---|
| INCLUDE | include long text in the script |
| DEFINE | assign values to text symbols |
| ADDRESS, ENDADDRESS | format adresses |
| PROTECT, ENDPROTECT | keep data on a single page |
| NEW PAGE | force a page break |
| IF, ENDIF | conditional block |
| CASE, ENDCASE | conditional block |

### Including text and loading graphics
                                   
INCLUDE &ZTEXT& OBJECT TEXT ID ADRS LANGUAGE E.  
In Line Editor use Insert, Text, Standard Text.  

BITMAP 'Z_NAZIV' OBJECT GRAPHICS ID BMAP TYPE BMON ( BCOL)  
In Line Editor use Insert, Graphics.  

### PERFORM..ENDPERFORM

In SAPscript PERFORM is used to invoke a routine in a program.  

| Syntax |
|---|
| DEFINE &SIFRA_POR& = ' ' |
| PERFORM POREZ IN PROGRAM ZTEST_OBRASCI |
| USING &VBDKR KUNRG& |
| CHANGING &SIFRA_POR& |
| ENDPERFORM |

```abap
REPORT ZTEST_FORM.  

FORM TAXES  
      TABLES input_table STRUCTURE ITCSY  
             output_table STRUCTURE ITCSY.  

  TABLES: kna1.  
  DATA: vendor_code TYPE n.  

  REFRESH output_table.  
  READ TABLE input_table WITH KEY name = 'VBDRK KUNRG'.  
  MOVE input_table value TO vendor_code.   

  SELECT SINGLE # FROM kna1  
    WHERE kunnr = vendor_code. 

  IF sy subrc EQ 0.  
    MOVE 'TAX_CODE' TO output_table name.  
    MOVE kna1 stcd1 TO output_table value.  
    APPEND output_table.  
  ENDIF.  

ENDFORM.  
```
