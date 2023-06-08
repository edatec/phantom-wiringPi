WiringPi (Unofficial Mirror/Fork)
=================================

This is a GPIO access library for REIMEI1(Phantom). It is based on original WiringPi for Raspberry Pi.

**NOTE:** This branch implements the RaspberryPi BCM Pins mapping.

# Installation

## Install WiringPi

```sh
git clone -b bcm-pins-mapping https://github.com/edatec/phantom-wiringPi.git
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

 +------+-----+---------+------+---+--Phantom-+---+------+---------+-----+------+
 | GPIO | wPi |   Name  | Mode | V | Physical | V | Mode | Name    | wPi | GPIO |
 +------+-----+---------+------+---+----++----+---+------+---------+-----+------+
 |      |     |    3.3v |      |   |  1 || 2  |   |      | 5v      |     |      |
 |   70 |   2 |   SDA.1 |   IN | 0 |  3 || 4  |   |      | 5v      |     |      |
 |   71 |   3 |   SCL.1 |   IN | 0 |  5 || 6  |   |      | 0v      |     |      |
 |   83 |   4 |    PIN7 |   IN | 0 |  7 || 8  | 0 | IN   | TxD     | 14  | 37   |
 |      |     |      0v |      |   |  9 || 10 | 0 | IN   | RxD     | 15  | 38   |
 |   79 |  17 |   PIN11 |   IN | 0 | 11 || 12 | 0 | IN   | PIN12   | 18  | 33   |
 |   75 |  27 |   PIN13 |   IN | 0 | 13 || 14 |   |      | 0v      |     |      |
 |   76 |  22 |   PIN15 |   IN | 0 | 15 || 16 | 0 | IN   | PIN16   | 23  | 77   |
 |      |     |    3.3v |      |   | 17 || 18 | 0 | IN   | PIN18   | 24  | 72   |
 |   45 |  10 |    MOSI |   IN | 0 | 19 || 20 |   |      | 0v      |     |      |
 |   46 |   9 |    MISO |   IN | 0 | 21 || 22 | 0 | IN   | PIN22   | 25  | 73   |
 |   48 |  11 |    SCLK |   IN | 0 | 23 || 24 | 0 | IN   | CE0     | 8   | 47   |
 |      |     |      0v |      |   | 25 || 26 | 0 | IN   | CE1     | 7   | 78   |
 |    0 |   0 |   SDA.0 |   IN | 0 | 27 || 28 | 0 | IN   | SCL.0   | 1   | 1    |
 |   36 |   5 |   PIN29 |   IN | 0 | 29 || 30 |   |      | 0v      |     |      |
 |   32 |   6 |   PIN31 |   IN | 0 | 31 || 32 | 0 | IN   | PIN32   | 12  | 35   |
 |   82 |  13 |   PIN33 |   IN | 0 | 33 || 34 |   |      | 0v      |     |      |
 |   40 |  19 |   PIN35 |   IN | 0 | 35 || 36 | 0 | IN   | PIN36   | 16  | 80   |
 |   74 |  26 |   PIN37 |   IN | 0 | 37 || 38 | 0 | IN   | PIN38   | 20  | 81   |
 |      |     |      0v |      |   | 39 || 40 | 0 | IN   | PIN40   | 21  | 31   |
 +------+-----+---------+------+---+----++----+---+------+---------+-----+------+
 | GPIO | wPi |   Name  | Mode | V | Physical | V | Mode | Name    | wPi | GPIO |
 +------+-----+---------+------+---+--Phantom-+---+------+---------+-----+------+

```


### Configure wiringPi pin as output

Taking PIN15 as an example,its wiringpi pin is 22.

```sh
gpio export 22 out
#output high level
gpio write 22 1
#output low level
gpio write 22 0
```

### Configure wiring pin as input

Taking PIN15 as an example,its wiringpi pin is 22.
```sh
gpio export 22 in
#read PIN15 level
gpio read 22
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

  sprintf (command, "/usr/local/bin/gpio export %d out", 22) ;
  system (command) ;
  
  wiringPiSetupSys() ;
  for(;;)
  {
    digitalWrite(22, HIGH) ;
    delay (500) ;
    digitalWrite(22,  LOW) ;
    delay (500) ;
  }
}
```

Compile and run "test.c":

```sh
gcc -Wall -o test test.c -lwiringPi -lpthread -lm
sudo ./test
```