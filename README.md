# sensor-imu

C library for interacting with various IMUs (MPU6000, MPU6050, MPU6500, ICM20600, ICM20601, ICM2062).

Features:
* works with Raspberry Pi / Linux and probably with almost every single-board computer equipped with I2C
* IMU model and address are autodetected during initialization
* sampling rate up to 1khz (when I2C speed is 400khz)
* orientation estimation algorithms are available in folder `/orientation`


## Installation

Copy all the files ending with `.c` and `.h` into your project folder.


## Usage

```
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <fcntl.h>

#include "imu.h"
#include "imu_auto.h"

int main() {
    int i2c_fd = open("/dev/i2c-1", O_RDWR);
    if(i2c_fd < 0) {
        return -1;
    }

    imu_autot* imu;
    error* err = imu_auto_init(&imu, i2c_fd);
    if(err != NULL) {
        return -1;
    }

    imu_output io;
    err = imu_auto_read(imu, &io);
    if(err != NULL) {
        return -1;
    }

    printf("gyro x,y,z: %f, %f, %f\n", io.gyro.x, io.gyro.y, io.gyro.z);
    printf("acc x,y,z: %f, %f, %f\n", io.acc.x, io.acc.y, io.acc.z);
    return 0;
}
```
