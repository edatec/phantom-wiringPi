WiringPi (Unofficial Mirror/Fork)
=================================

This is a GPIO access library for REIMEI1(Phantom). It is based on original WiringPi for Raspberry Pi.

# Installation

## Install WiringPi

```sh
git clone https://github.com/edatec/phantom-wiringPi.git
cd phantom-wiringPi
./build clean
./build
```

## Uninstall WiringPi
```sh
./build uninstall
```


# Usage

## gpio command

```sh
gpio readall

+------+-----+---------+------+---+--Phantom-+---+------+---------+-----+-----+
 | GPIO | wPi |   Name  | Mode | V | Physical | V | Mode | Name    | wPi | GPIO |
 +------+-----+---------+------+---+----++----+---+------+---------+-----+------+
 |      |     |    3.3v |      |   |  1 || 2  |   |      | 5v      |     |      |
 |   70 |   8 |   SDA.1 |   IN | 0 |  3 || 4  |   |      | 5v      |     |      |
 |   71 |   9 |   SCL.1 |   IN | 0 |  5 || 6  |   |      | 0v      |     |      |
 |   83 |   7 |    PIN7 |   IN | 1 |  7 || 8  | 0 | IN   | TxD     | 15  | 37   |
 |      |     |      0v |      |   |  9 || 10 | 0 | IN   | RxD     | 16  | 38   |
 |   79 |   0 |   PIN11 |   IN | 0 | 11 || 12 | 1 | IN   | PIN12   | 1   | 33   |
 |   75 |   2 |   PIN13 |   IN | 0 | 13 || 14 |   |      | 0v      |     |      |
 |   76 |   3 |   PIN15 |   IN | 0 | 15 || 16 | 1 | IN   | PIN16   | 4   | 77   |
 |      |     |    3.3v |      |   | 17 || 18 | 0 | IN   | PIN18   | 5   | 72   |
 |   45 |  12 |    MOSI |   IN | 0 | 19 || 20 |   |      | 0v      |     |      |
 |   46 |  13 |    MISO |   IN | 0 | 21 || 22 | 0 | IN   | PIN22   | 6   | 73   |
 |   48 |  14 |    SCLK |   IN | 0 | 23 || 24 | 0 | IN   | CE0     | 10  | 47   |
 |      |     |      0v |      |   | 25 || 26 | 0 | IN   | CE1     | 11  | 78   |
 |    0 |  30 |   SDA.0 |   IN | 0 | 27 || 28 | 0 | IN   | SCL.0   | 31  | 1    |
 |   36 |  21 |   PIN29 |   IN | 0 | 29 || 30 |   |      | 0v      |     |      |
 |   32 |  22 |   PIN31 |   IN | 0 | 31 || 32 | 0 | IN   | PIN32   | 26  | 35   |
 |   82 |  23 |   PIN33 |   IN | 0 | 33 || 34 |   |      | 0v      |     |      |
 |   40 |  24 |   PIN35 |   IN | 0 | 35 || 36 | 0 | IN   | PIN36   | 27  | 80   |
 |   74 |  25 |   PIN37 |   IN | 0 | 37 || 38 | 0 | IN   | PIN38   | 28  | 81   |
 |      |     |      0v |      |   | 39 || 40 | 0 | IN   | PIN40   | 29  | 31   |
 +------+-----+---------+------+---+----++----+---+------+---------+-----+------+
 | GPIO | wPi |   Name  | Mode | V | Physical | V | Mode | Name    | wPi | GPIO |
 +------+-----+---------+------+---+--Phantom-+---+------+---------+-----+------+

```

### Configure wiringPi pin as output

Taking PIN15 as an example

```sh
gpio export 3 out
#output high level
gpio write 3 1
#output low level
gpio write 3 0
```

### Configure wiring pin as input

Taking PIN15 as an example

```sh
gpio export 3 in
#read PIN15 level
gpio read 3
```


## Code Sample with wiringPi

Taking PIN15 as an example

test.c

```sh
#include <stdint.h>
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <wiringPi.h>

int main(void)
{
  char command [128] ;

  sprintf (command, "/usr/local/bin/gpio export %d out", 3) ;
  system (command) ;
  
  wiringPiSetupSys() ;
  for(;;)
  {
    digitalWrite(3, HIGH) ;
    delay (500) ;
    digitalWrite(3,  LOW) ;
    delay (500) ;
  }
}
```

Compile and run "test.c":

```sh
gcc -Wall -o test test.c -lwiringPi -lpthread -lm
sudo ./test
```