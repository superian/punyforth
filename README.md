# punyforth

Punyforth is a toy implementation of the FORTH programming language. Most parts of Punyforth is written in itself. Including the outer interpreter and the compiler (that compiles indirect-threaded code). The primitives are implemented in assembly language.

## Notes to myself:

### Demonstration of *create> does*

```forth
: constant ( n -- ) 
    word create , 
    does> @ ;
    
: does>
  lit (does) , lit exit ,
  104 c, dodoes , 195 c,                      ( complile embedded assembly: PUSH DODOES RETN )
; immediate

: (does)
    rdup r> 1 cells +                         ( address of the embedded assembly code )
    lastword 1 cells + dup @ + 2 cells + ! ;  ( change lastword codeword to point to the embedded assembly code )

-1 constant TRUE 
0 constant FALSE

( The DODOES is a similar codeword than DOCOLON /ENTERCOL in punyforth/. 
The main difference between DODOES and DOCOLON is that the former pushes 
the address of the parameter field of the word. )

DODOES:
    mov [ebp], esi          // save esi to return stack
    add ebp, CELLS
    mov esi, [eax]          // [eax] points to the embedded assembly that called DODOES
    add esi, 6              // length of the embedded assembly code is 6, after that there are the forth code
    add eax, CELLS          // eax points to the codeword of the defined word, after that there is the param. field
    push eax
    NEXT

```





