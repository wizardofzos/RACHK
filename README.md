# RACHK
An example implementation to do a RACOUTE REQUEST=AUTH from a COBOL Module.
Build with love. Big thanks to Louis and Anji for the tip&tricks to get this
working.

# Usage 
1. Amend .RACHK.JCL(COMPASM) to reflect correct datasetnames
2. Submit it
3. Amend .RACHK.JCL(COMPCOB) to reflect correct datasetnames
4. Submit it
5. Amend .RACHK.JCL(RUNCOB) to reflect correct datasetnames
6. Submit it

# Workings

```
 +---------------------------------------------------+                                                   
 |01  RACHK-COMMAREA.                                |                                                   
 |    05 RACHK-RETURNCODE  PIC S9(8) USAGE IS BINARY.<-----RACROUTE RC----+  
 |    05 RACHK-RACFPLEN.   PIC S9(8) USAGE IS BINARY.|                    |  
 |    05 RACHK-RACFPROF.   PIC X(255)                |                    |
 +---------------------------------------------------+                    |  
                                                                          |  
 +--------------------------------------------------+                     |  
 |move 'Test.Read.On.This.EJBROLE' to RACHK-RACFPROF|                     |  
 |move 25 to RACHK-RACFPLEN                         |                     |  
 |call 'RACHK' using RACHK-COMMAREA                 |                     |  
 +--------+-----------------------------------------+                     |  
          |                                                               |  
      +---v--++               +---------------------------------------------+
      | RACHK|+---------------->SAF/RACF                                    |
      +---+--++               | CHECK ACCESS FOR 'Test.Read.On.This.EJBROLE'|
          |                   +---------------------------------------------+
          |                                                                  
+---------v-------------------------------------+                            
| IF RACHK-RETURNCODE = 0 THEN                  |                            
| do-stuff-because-user-is-authorized           |                            
| ELSE                                          |                            
| do-other-stuff-because-user-is-not-authorized |                            
+-----------------------------------------------+
```
