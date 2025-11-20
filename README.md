# Skill-Assessment-2-MPMC
Question : 

Write an assembly language program in 8051 to generate a 50 ms delay using Timer 0 in Mode 2 (8-bit auto-reload mode) and blink an LED connected to Port 3.0.

AIM:

To write an 8051 assembly language program to generate a 50 ms delay using Timer-0 in Mode-2 (8-bit auto-reload mode) and blink an LED connected to Port 3.0.

APPARATUS REQUIRED:

8051 Microcontroller Board

Keil µVision 

LED & Resistor

Power Supply

Connecting wires

THEORY:

About 8051 Timer-0 Mode-2

Mode-2 is an 8-bit auto-reload mode.

TH0 holds the reload value.

TL0 counts from the reload value up to FFH, then overflows.

On overflow:
TF0 is set.
TL0 is automatically reloaded from TH0.
Delay Calculation
Assuming 12 MHz crystal:
Machine cycle = 1 µs
Timer increments every 1 µs
Required total delay = 50 ms
Because an 8-bit timer can only hold 0–255 counts, we generate:
250 µs delay per overflow
→ Reload value = 256 − 250 = 06H
To reach 50 ms:
250 µs × 200 = 50 ms
Thus, 200 timer overflows generate 50 ms.

ALGORITHM:

1.Start the program.

2.Jump to MAIN to begin execution.

3.Initialize Port 3.0 as output and turn OFF the LED initially.

4.Configure Timer-0 in Mode-2 (8-bit auto-reload):

   Load TMOD with 02H
   
   Load TH0 and TL0 with 00H (full 256-count overflow)
   
5.Enter the BLINK_LOOP:

   Toggle LED connected to P3.0 using CPL P3.0.
   
6.Call the delay subroutine DELAY_50MS to create approximately 50 ms delay using repeated timer overflows.

7.Inside DELAY_50MS subroutine:

  Load register R2 with 180, the required number of Timer-0 overflows to produce 50 ms.
  
  Start Timer-0 (set TR0 = 1).

  Wait for TF0 to become 1 (timer overflow).
  
  Clear TF0 (reset overflow flag).
  
  Decrement R2 and repeat until R2 = 0.
  
  Stop Timer-0 (clear TR0).

  Return to main program.
8.Return to BLINK_LOOP and continue blinking indefinitely.

9.End of program.

PROGRAM :

ORG 0000H
LJMP MAIN

MAIN:
    ; Initialize Port 3.0 as output (LED off initially)
    CLR P3.0  ; LED off
    

    ; Configure Timer 0 in Mode 2 (8-bit auto-reload)
    
    MOV TMOD, #02H  ; Timer 0, Mode 2
    
    MOV TH0, #00H   ; Reload value 0 (full 256 counts per overflow)
    
    MOV TL0, #00H   ; Start from 0

BLINK_LOOP:

    ; Toggle LED on P3.0
    
    CPL P3.0

    ; Generate 50 ms delay
    
    CALL DELAY_50MS

    ; Repeat forever
    
    SJMP BLINK_LOOP

; Subroutine for 50 ms delay using Timer 0

DELAY_50MS:

    MOV R2, #180    ; Counter for 180 overflows (180 * 277.76 µs ˜ 50 ms)
    
    SETB TR0        ; Start Timer 0

WAIT_LOOP:

    JNB TF0, WAIT_LOOP  ; Wait for overflow (TF0 set)
    
    CLR TF0             ; Clear overflow flag
    
    DJNZ R2, WAIT_LOOP  ; Decrement counter and repeat

    CLR TR0             ; Stop Timer 0
    
    RET

END

OUTPUT:

<img width="1162" height="756" alt="image" src="https://github.com/user-attachments/assets/de2401aa-c8a3-4e8a-b7f1-617dce142785" />
<img width="1280" height="363" alt="image" src="https://github.com/user-attachments/assets/7de63655-fd28-4478-9b55-f399d3860632" />



 RESULT:
 
The assembly program to generate a 50 ms delay using Timer-0 Mode-2 and blink an LED on P3.0 was successfully written, executed, and verified.
