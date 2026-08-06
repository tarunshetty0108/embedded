#include <xc.h>
#define _XTAL_FREQ 20000000

void main()
{
    unsigned char occupied, available, i;

    TRISB = 0xFF;   // IR Sensors
    TRISD = 0x00;   // LEDs
    TRISA = 0x00;   // Gate

    LCD_Init();

    while(1)
    {
        occupied = 0;

        // Count occupied slots
        for(i = 0; i < 4; i++)
        {
            if(PORTB & (1 << i))
                occupied++;
        }

        available = 4 - occupied;

        // Display
        LCD_Clear();
        LCD_Set_Cursor(1,1);
        LCD_String("Total:4");

        LCD_Set_Cursor(2,1);
        LCD_Char(available + '0');
        LCD_String(" Free");

        // Gate Control
        PORTAbits.RA0 = (available > 0);

        __delay_ms(500);
    }
}
