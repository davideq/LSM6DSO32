# LSM6DSO32
Arduino library to support the LSM6DSO32 3D accelerometer and 3D gyroscope

## API

This sensor uses I2C, I3C or SPI to communicate.
For I2C it is then required to create a TwoWire interface before accessing to the sensors:  

    TwoWire dev_i2c(I2C_SDA, I2C_SCL);  
    dev_i2c.begin();

For SPI it is then required to create a SPI interface before accessing to the sensors:  

    SPIClass dev_spi(SPI_MOSI, SPI_MISO, SPI_SCK);  
    dev_spi.begin();

For I3C it is then required to create an I3C interface before accessing to the sensors:

    I3C.begin(I3C_SDA, I3C_SCL, 1000000U);

An instance can be created and enabled when the I2C bus is used following the procedure below:  

    LSM6DSO32Sensor AccGyr(&dev_i2c);
    AccGyr.begin();
    AccGyr.Enable_X();  
    AccGyr.Enable_G();

An instance can be created and enabled when the SPI bus is used following the procedure below:  

    LSM6DSO32Sensor AccGyr(&dev_spi, CS_PIN);
    AccGyr.begin();	
    AccGyr.Enable_X();  
    AccGyr.Enable_G();

An instance can be created and enabled when the I3C bus is used with SETDASA (static-to-dynamic address assignment):

    LSM6DSO32Sensor AccGyr(&I3C, LSM6DSO32_I3C_ADD_H);
    I3C.resetDynamicAddresses();
    I3C.assignDynamicAddress(AccGyr.getStaticAddress(), LSM6DSO32_DYNAMIC_ADDRESS);
    AccGyr.begin(LSM6DSO32_DYNAMIC_ADDRESS);
    I3C.setClock(12500000);
    AccGyr.Enable_X();
    AccGyr.Enable_G();

An instance can be created and enabled when the I3C bus is used with ENTDAA (dynamic address discovery):

    LSM6DSO32Sensor AccGyr(&I3C);
    I3C.begin(I3C_SDA, I3C_SCL, 1000000U);
    I3C.discover(devices, 8, &found);
    // find dynAddr by matching LSM6DSO32_I3C_PID in discovered devices
    AccGyr.begin(dynAddr);
    I3C.setClock(12500000);
    AccGyr.Enable_X();
    AccGyr.Enable_G();

The access to the sensor values is done as explained below:  

  Read accelerometer and gyroscope.

    int32_t accelerometer[3];
    int32_t gyroscope[3];
    AccGyr.Get_X_Axes(accelerometer);  
    AccGyr.Get_G_Axes(gyroscope);

# Examples

There are several examples with the LSM6DSO32 library.
* LSM6DSO32_HelloWorld_I2C: This application provides a simple example of usage of the LSM6DSO32 
IMU 6-axis. It shows how to display on a hyperterminal the values of the sensor.
* LSM6DSO32_6DOrientation_I2C: This application shows how to use the LSM6DSO32 accelerometer 
to find out the 6D orientation and display data on a hyperterminal.
* LSM6DSO32_FreeFallDetection_I2C: This application shows how to detect the free fall event using the 
LSM6DSO32 accelerometer.
* LSM6DSO32_Pedometer_I2C: This application shows how to use the LSM6DSO32 accelerometer 
to count steps.
* LSM6DSO32_SingleTap_I2C: This application shows how to detect the single tap event using the 
LSM6DSO32 accelerometer.
* LSM6DSO32_DoubleTap_I2C: This application shows how to detect the double tap event using the 
LSM6DSO32 accelerometer.
* LSM6DSO32_TiltDetection_I2C: This application shows how to detect the tilt event using the 
LSM6DSO32 accelerometer.
* LSM6DSO32_WakeUpDetection_I2C: This application shows how to detect the wake-up event using the 
LSM6DSO32 accelerometer.
* LSM6DSO32_Datalog_Terminal_I3C: This application shows how to use LSM6DSO32 accelerometer and gyroscope over I3C using SETDASA.
* LSM6DSO32_Datalog_Terminal_I3C_ENTDAA: This application shows how to discover and use LSM6DSO32 dynamic address over I3C.

## Documentation

You can find the source files at  
https://github.com/stm32duino/LSM6DSO32

The LSM6DSO32 datasheet is available at  
https://www.st.com/en/mems-and-sensors/lsm6dso32.html