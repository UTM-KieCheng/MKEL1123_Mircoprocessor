# MKEL1123_Mircoprocessor_Milestone1

This program is developed for the **STM32 Nucleo-F446RE** development board and demonstrates how to blink the onboard LED (connected to **GPIO Port A Pin 5**) with a variable blinking speed by adjusting the delay between ON and OFF states.
  
The LED blinks a configurable number of times (default is **2**, controlled by `u8MaxBlink`) at a delay interval defined by the variable `u16Frequency`. After reaching the set number of blinks, the delay is adjusted based on the value of the `s8Direction` variable, which determines whether the delay is increased or decreased:

- A **positive direction** (`s8Direction = 1`) increases the delay (slows blinking).
- A **negative direction** (`s8Direction = -1`) decreases the delay (speeds up blinking).

The `u16Frequency` variable controls the delay between LED ON and OFF states. When this value reaches either the **minimum (200 ms)** or **maximum (800 ms)** limit, the direction is reversed, creating a continuous loop of increasing and decreasing blink speeds.

---

##  Global Variables

The following section defines the global variables used to control the LED blinking behavior:

```c
uint16_t u16Frequency = 200;  // Delay in milliseconds
uint8_t  u8NumOfBlink = 0;    // Blink counter
int8_t   s8Direction  = 1;    // Direction of frequency change: +1 or -1
uint8_t  u8MaxBlink   = 2;    // Number of blinks before frequency update
```
##  Main Loop 

The following section defines the LED blinking behavior:

```c
GPIOA->BSRR = GPIO_PIN_5;         // Turn ON the LED connected to GPIO Port APin 5  
HAL_Delay(u16Frequency);          // Wait for the current frequency duration  

GPIOA->BSRR = GPIO_PIN_5 << 16U;  // Turn OFF the LED  
HAL_Delay(u16Frequency);          // Wait again to complete one blink cycle  

u8NumOfBlink += 1;                 // Increase the blink counter  

// After the LED has blinked u8MaxBlink times (e.g., 5), update frequency  
if (u8NumOfBlink == u8MaxBlink)  
{  
  u8NumOfBlink = 0;              // Reset blink counter for the next cycle  
  
  u16Frequency = u16Frequency + (100 * s8Direction);  // Change the blinking frequency based on direction  
  
  if (u16Frequency >= 800)  // If frequency exceeds upper limit, reverse direction to start decreasing  
  {  
    u16Frequency = 800;    // Set the frequency to the upper limit = 800  
    s8Direction = -1;      // Set direction to decrease  
  }  
  else if (u16Frequency <= 200) // If frequency goes below lower limit, reverse direction to start increasing  
  {  
    u16Frequency = 200;    // Set the frequency to the lower limit = 200  
    s8Direction = 1;       // Set direction to increase  
  }  
}  
```

The demonstration video can be found here:   
