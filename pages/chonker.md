# Chonker x86 Game 🐹

<p align="center">
  <img src="/img/chonker.jpeg"/>
</p>

# What is it?

This game was created using Assembly code and DOS. Hardware/software interrupts were used to respond to button presses. In turn those interrupts were used to create an Escape game in Microsoft Macro Assembler (MASM). This was done to understand interrupts and improve my ability to program in assembly. Probably one of the hardest things I have had to conceptualize and code!

# Interrupts

<p align="center">
  <img src="/img/flowchart.jpeg"/>
</p>

Interrupts offer an alternative way of responding to external events in a microprocessor system. The microprocessor can poll for changes in the environment by constantly looking for those changes, or it can wait until a device signals for a change.

#### INT 10H

BIOS interrupt used to set the video mode of the project.

#### INT 21H

Hardware interrupt for character presses.

# Frustration

Not all coding is fun and cool, here is a clip of this game failing 😂. The reason why this type of code is particularly difficult is because it is extremely low-level and typically what a compiler will spit out. Assembly follows a 1:1 ratio, between what the computer is told to do and what it does. This is how computers understand the code you type in your chosen program, and typically is not the easiest to read or understand. This type of code interacts directly with registers and other logic gates behind the scenes. Check out this video below see how frustrating this coding can be.

[![Frustration](http://img.youtube.com/vi/21oYjKJl1RE/0.jpg)](http://www.youtube.com/watch?v=21oYjKJl1RE)

> _Click to Proceed to the Video_

# Full Code

```asm
.MODEL small
.STACK 100h
.386

.data
	msg DB "CHONKER TERMINATED", 0	; first msg
	nSize DW ($ - msg)-1

;used to generate random rock positions on screen
	
	randnum	DB 2, 60, 40, 41, 47, 70, 49, 48, 5, 72, 67, 36, 50, 11, 1, 64, 30, 53, 34, 4
			DB 21, 14, 26, 67, 54, 77, 18, 5, 35, 7, 55, 31, 43, 3, 42, 47, 32, 39, 16, 30
			DB 77, 43, 8, 21, 64, 23, 60, 47, 56, 32, 20, 70, 41, 27, 5, 24, 65, 18, 67, 44
			DB 18, 46, 57, 61, 54, 53, 19, 17, 29, 64, 52, 2, 51, 5, 45, 65, 78, 31, 28, 4
			DB 3, 11, 21, 45, 31, 7, 57, 59, 10, 50, 32, 24, 37, 6, 36, 48, 34, 43, 29, 74
			DB 31, 43, 52, 24, 21, 79, 44, 47, 2, 66, 75, 56, 53, 77, 71, 76, 38, 15, 65, 74
			DB 50, 59, 35, 20, 19, 75, 64, 27, 37, 54, 56, 61, 45, 76, 2, 1, 16, 4, 7, 79
			DB 52, 20, 42, 40, 29, 41, 25, 79, 49, 27, 75, 56, 36, 19, 2, 18, 15, 65, 11, 73
			DB 55, 70, 26, 35, 40, 12, 49, 22, 18, 44, 16, 58, 60, 52, 4, 2, 25, 28, 64, 71
			DB 67, 49, 30, 14, 24, 23, 63, 27, 43, 6, 64, 39, 72, 28, 61, 16, 4, 2, 32, 18
			DB 9, 16, 79, 46, 33, 78, 8, 49, 62, 26, 27, 48, 52, 38, 11, 35, 63, 42, 74, 47
			DB 48, 25, 79, 76, 63, 78, 10, 34, 26, 38, 2, 15, 70, 49, 44, 41, 23, 6, 68, 17
			DB 36, 55, 41, 42, 71, 65, 51, 34, 47, 10, 25, 29, 37, 64, 77, 40, 49, 6, 61, 72
			DB 33, 30, 19, 10, 14, 57, 75, 79, 8, 6, 29, 77, 52, 18, 4, 23, 58, 72, 62, 47
			DB 37, 42, 62, 55, 45, 8, 48, 70, 19, 35, 7, 27, 76, 39, 61, 79, 65, 30, 52, 44
			DB 23, 38, 43, 77, 76, 37, 26, 31, 73, 36, 14, 25, 68, 39, 28, 55, 54, 11, 10, 16
			DB 71, 54, 72, 27, 75, 74, 64, 48, 37, 58, 11, 65, 31, 50, 18, 8, 60, 20, 59, 35
			DB 43, 48, 51, 79, 60, 59, 46, 3, 13, 64, 57, 7, 70, 74, 23, 69, 65, 34, 29, 22
			DB 27, 4, 58, 5, 67, 8, 62, 2, 78, 61, 34, 42, 9, 12, 50, 74, 54, 6, 53, 25
			DB 54, 61, 17, 16, 44, 21, 15, 10, 64, 33, 74, 2, 7, 52, 47, 22, 36, 29, 65, 24
			
	randSize DW ($-randnum)-1

; never found a use for this lol	
	randPtr DW 0

; tracking of chonker (only x-position needed)	
	xpos DB 20h
	ypos DB 8h	

; 0.1 delay used to draw chonker at cursor position 
.code
delay proc
	MOV CX, 01h
	MOV DX, 86A0h
	MOV AH, 86h
	INT 15h	; 1 seconds delay	
	RET
delay ENDP

; sets position of cursor in video
func1 proc
	MOV AH, 2
	MOV BH, 0
	INT 10h      
	RET
func1 ENDP

;
	PUSH DX
	PUSH BX
	XOR DX, DX
	MOV BX, 0Ah
	DIV BX
	INC DX
	MOV AX, DX
	POP BX
	POP DX
	RET

; code used to generate 20 random coordinates
	; gets the time from system
	; does clock count, al = 00h if clock was read or written otherwise al > 0
random proc ; REFERENCE [1] https://stackoverflow.com/questions/29666034/printing-coordinates-assembly-8086
	PUSH CX
	PUSH DX
	XOR AX, AX
	INT 1Ah             
	MOV DS:21h, DX
	POP DX
	POP CX
	RET
random ENDP

; function that can be used to restore the values of registers so the register can be used elsewhere
func2 proc
	PUSH DX
	MOV AX, 6255h
	MUL WORD PTR DS:21h
	ADD AX, 3619h
	MOV DS:21h, AX
	POP DX
	RETN
func2 ENDP

_main PROC

; generates the random positions for the rocks to be written to
call random

OuterLoop:	

	MOV DX, CS
	MOV DS, DX
	MOV CX, 0Ah
 

Loop1:

	PUSH CX
	CALL func2
	MOV DL, AL
	MOV DH, 18h
	CALL func1 
	MOV AL, 52h ; 'R'
	MOV BH, 0
	MOV BL, 3
	MOV CX, 1
	MOV AH, 9
	INT 10h
; drawing of rocks around the cursor position
	
	POP CX
	LOOP Loop1
	MOV CH, 0
	MOV CL, 0
	MOV DH, 19h
	MOV DL, 50h ; 'P'
	MOV AL, 1
	MOV AH, 6
	INT 10h    ;int to scroll page up         
; al = number of lines to scroll page up
; ch,cl = upper left corner of window to scroll

	MOV DL, xpos
	MOV DH, 11h
	CALL func1
	MOV BH, 0
	MOV AH, 8
	INT 10h             ; int to read cursor position to determine if chonket hit rock
                        ; al = chonker
	CMP AL, 52h ; 'R' 	; compare rock to chonker
	JZ Loop5
	MOV DL, xpos
	MOV DH, 11h
	CALL func1
	MOV AL, 23h ; '#'	; once chonker is safe draw the chonker to video screen
	MOV BH, 0
	MOV BL, 7
	MOV CX, 1
	MOV AH, 9
	INT 10h             ; another int call to draw the chonker
	
	CALL delay			; waits 0.1 to draw chonker
	MOV AH, 0Bh
	INT 21h             ; check q is pressed
      
	CMP AL, 0			; if no chonker start noKey
	JZ SHORT noKey
	MOV AH, 0
	INT 16h             ; keyboard check 
	
	CMP AL, 71h ; 'Q'
	JZ SHORT Loop5
	CMP AL, 61h ; 'A'
	JZ SHORT Loop3
	CMP AL, 73h ; 'S'
	JZ SHORT Loop4
	;checks if left or right is pressed
	
noKey:
	MOV DL, xpos
	MOV DH, 11h
	CALL func1
	MOV AL, 58h ; 'X'
	MOV BH, 0
	MOV BL, 7
	MOV CX, 1
	MOV AH, 9
	INT 10h             ;drawing of X and response no movement
	JMP OuterLoop
; ---------------------------------------------------------------------------

Loop3:                             
	MOV DL, xpos
	MOV DH, 11h
	CALL func1
	MOV AL, 58h ; 'X'
	MOV BH, 0
	MOV BL, 7
	MOV CX, 1
	MOV AH, 9
	INT 10h
	cmp xpos, 0
  	je  noKey	;drawing of X and response to left movement
	dec xpos
	JMP OuterLoop
; ---------------------------------------------------------------------------

Loop4:                              
	MOV DL, xpos
	MOV DH, 11h
	CALL func1
	MOV AL, 58h ; 'X'
	MOV BH, 0
	MOV BL, 7
	MOV CX, 1
	MOV AH, 9
	INT 10h  
	cmp xpos, 0
  	je  noKey	;drawing of X and response to right movement
	INC xpos
	JMP OuterLoop
; ---------------------------------------------------------------------------

Loop5:    
                          
	MOV AL, 1
	MOV BH, 0
	MOV BL, 6
	MOV CX, randSize
	MOV DX, CS
	MOV ES, DX

	MOV DH, 18h
	MOV DL, 20h ; ' '
	MOV BP, 0Ah
	MOV AH, 13h
;	INT 10h	;should print string about chonker termination but its not working
	call delay
	call delay
	call delay
	INT 10h
	jmp _main
	MOV AX, 4C00h
	INT 21h             ; quits the program

_main ENDP
END _main
```

...return to [`projects.ibrahimsaid.ca`](https://projects.ibrahimsaid.ca/) or [`ibrahimsaid.ca`](https://www.ibrahimsaid.ca/).