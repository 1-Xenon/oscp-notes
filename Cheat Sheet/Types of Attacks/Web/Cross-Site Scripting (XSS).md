https://book.hacktricks.xyz/pentesting-web/xss-cross-site-scripting
## Special Characters used in XSS
```
< > ' " { } ;
```
< > is used to denote elements in HTML
{ } is used to declare functions in JavaScript
' and " are used to denote strings
; is used to mark the end of a statement

DO XSS WHEN THE INPUT FIELD ACCEPTS UNSANTIZED INPUTS
Always read how the input is being processed