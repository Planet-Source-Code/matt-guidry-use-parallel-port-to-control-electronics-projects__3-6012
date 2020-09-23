<div align="center">

## Use parallel port to control electronics projects


</div>

### Description

If you hook up 8 LED lights to the parallel port, you can use this program to make them dance in cool patterns, or you can output a number to the port, and the lights will light up that number in binary. Requires basic electronics skills to hook up, but you basically connect 8 leds to pins 2 through 9 on the port, and ground them to pin 25. If you don't know how to do that, then just read the code to find out how to use the port. This will NOT work on NT, 2000, or XP without some workarounds since those operating systems don't allow direct access to the port. In order to use this code in Windows NT, 2000, or XP, which does not normally let you directly access the computer's ports, please visit the following link to download PortTalk. There is an explanation there on why it is needed and what it does. The downloaded file will contain a readme.txt file which contains instructions on how to use it.

The link: http://www.beyondlogic.org/porttalk/porttalk.htm
 
### More Info
 
**UPDATE**

To use this code in Windows NT, 2000, or XP, which does not normally let you directly access the computer's ports, please visit the following link to download PortTalk. There is an explanation there on why it is needed and what it does. The downloaded file will contain a readme.txt file which contains instructions on how to use it.

The link: http://www.beyondlogic.org/porttalk/porttalk.htm


<span>             |<span>
---                |---
**Submitted On**   |
**By**             |[Matt Guidry](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByAuthor/matt-guidry.md)
**Level**          |Advanced
**User Rating**    |4.5 (27 globes from 6 users)
**Compatibility**  |C\+\+ \(general\), Microsoft Visual C\+\+
**Category**       |[Miscellaneous](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByCategory/miscellaneous__3-1.md)
**World**          |[C / C\+\+](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByWorld/c-c.md)
**Archive File**   |[](https://github.com/Planet-Source-Code/matt-guidry-use-parallel-port-to-control-electronics-projects__3-6012/archive/master.zip)





### Source Code

```
//Do various things with leds hooked up to the parallel port from C++
//Or just learn how to use the parallel port
//Does not work with NT, 2000, or XP due to operation in protected mode. There are workarounds, but I'm not gonna go into them, since I'm lazy and they're complicated. Just find a computer with 95, 98, or ME on it.
/*
How to attach leds to the parallel port
---------------------------------------
Get a parallel port plug from radioshack, 8 leds, 8 330ohm resistors, some wire, a soldering iron, solder, and prototyping board
Solder 8 wires to the parallel plug on pins two through nine, and solder another on pin 25
solder the first 8 wires to the proto board, connect them to the resistors, then connect the resistors to the leds (check polarity, the parallel port puts out +5v) and then hook up the last pin on all the leds to the wire on pin 25
If none of this made any sense, then bug me about it until I write a better guide, maybe even get pictures and a video, or you could ask a friend that knows something more about electornics to do it for you
Note: 330 ohm resistors should be fine for this, but some leds should use smaller or larger resistors than this.
	If you want to learn more about hooking leds up, go to metku.net (not my site), and read the basic electornics tutorial, and maybe read up on some of the projects
*/
#include <conio.h>
#include <stdio.h>
#include <windows.h>
void Menu();
void UserDef();
void Walk();
void Bounce();
int LED[8]={1,2,4,8,16,32,64,128};
int main()
{
	system("cls");
	Menu();
	return 0;
}
//---------------------------------------------------------------
void Menu()
{
	int c=1;
	while(c != 0)
	{
		printf("Choose the mode\n");
		printf("(1) User defined values\n");
		printf("(2) Walk\n");
		printf("(3) Bounce\n");
		printf("(0) Quit\n");
		printf("------------------------\n");
		scanf("%d", &c);
		switch (c) {
			case 1: UserDef(); break;
			case 2: Walk(); break;
			case 3: Bounce(); break;
			case 0: break;
			default: system("cls"); printf("That is not a valid choice\n\n"); break;
		} //switch
	} //while loop
} //function
//---------------------------------------------------------------
void UserDef()
{
	int i=0;
	do
	{
		printf("Enter value (integer 0-256) (257 to quit): ");
		scanf("%d", &i);
		if (i != 257)
			_outp(0x378, i);
	} while(i != 257);
	system("cls");
}
//---------------------------------------------------------------
void Walk()
{
	int t;
	printf("Enter interval (ms): ");
	scanf("%d", &t);
	printf("Press any key to stop\n");
	int i=0;
	while(!kbhit())
	{
		_outp(0x378, LED[i]);
		i++;
		_sleep(t);
		if (i > 7)
			i = 0;
	}
	system("cls");
}
//---------------------------------------------------------------
void Bounce()
{
	int t;
	int i=1;
	bool fwd=true;
	printf("Enter interval (ms): ");
	scanf("%d", &t);
	printf("Press any key to stop\n");
	while(!kbhit())
	{
		do{
			_outp(0x378, LED[i]);
			i++;
			_sleep(t);
		}while( i<=7 && !kbhit());
		i-=2;
		do{
			_outp(0x378, LED[i]);
			i--;
			_sleep(t);
		} while(i >= 0 && !kbhit());
		i+=2;
	}
	system("cls");
}
```

