# STM32F407VG Discovery Synth

Simple wavetable synthesizer for intro to audio on this kit using STM32CubeIDE.
I'm learning as I go.  Sharing in case this can also be useful for others.

Thanks to these YouTube videos for inspiration:
* https://www.youtube.com/watch?v=n9Cy1xkEf1E&list=PLTNEB0-EzPluXh0d_5zRprbgRfgkrYxfO
* https://www.youtube.com/watch?v=QIPQOnVablY
and https://github.com/MrBlueXav/Dekrispator_v2 for showing the usage of BSP_AUDIO

## Goals

- just a simple demo
- use STM32CubeIDE
- mainly code in a single file (main.c)
- wavetable synth
- using HAL & BSP_AUDIO Stm32 Firmware
- user button as input for notes & animates LEDs  

## Usage

- git clone this repo
- add repo to STM32CubeIDE
- compile & run
- press user button to play notes

## Steps

If you want to do this yourself, here is a digest of how I got to this point. 
Use https://github.com/rogerallen/stm32disc_synth1/commits/main to see how 
things changed over time.

- create new project in STM32CubeIDE
- select STM32F407VG-DISC1 board as starting point
- said "no" to initializing board connections to the default setup.
- create git repo to track
- add generated files
- setup Debug build & add more files
- add basic pushbutton & LED code
  - B1 & LD3-6 are enabled by default even though I said "no" above.
- add Audio I2C, I2S interfaces.
	Watch https://www.youtube.com/watch?v=QIPQOnVablY for a guide to how to use 
	the GUI to adjust the controls.  Note that we are using I2C and I2S.  We are 
	not using DAC or Timers, so skip those bits. Starting at 2:50 he goes over 
	the GUI and most things apply, but some things were a little different for me.
	- Making Audio_RST on PD4 as GPIO_Output was already done for me.
	- Adding the I2C Control Interface:
	  - For Audio_SDA,SCL on PB9, PB6 
	  - In Connectivity/I2C1 -> Select I2C mode
	- Adding I2S Sound Data Interface
	  - I2S3_MCK, CK, SD, WS on PC7, PC10, PC12 and PA4
	  - In Multimedia/I2S3 -> Select Half-Duplex Master Mode
	  - Select Master Clock Output
	  - DON'T follow the video where they modify the PA4 pin at 4:30
	  - DON'T follow the video to add the timer.
	- Add External Oscillator
	  - System Core/RCC High Speed Clock -> Crystal/Ceramic …
	  - Edit Clock configuration to enable HSE and CSS and adjust to 168 MHz max.
	- In Config window
	  - I2S3 Parameter settings.  Only had to change Audio Freq to 48KHz. Other settings were as in the video
	  - I2S3 DMA.  Add that as in the video, but change Threshold to Half Full.
	  - I2C1 as default
	  - DON'T use DAC or TIM2 settings
	Note that at 23:50 he re-adjusts the project for the non-DAC method that 
	we're going to use, putting the I2S3 back & disabling the timer.
- add BSP audio library code
  - follow instructions in https://community.st.com/s/question/0D50X0000AtidTb/how-to-add-bsp-library-cubemxide
  - copy Firmware BSP folder to Drivers and trim
  - copy Middlewares/.../PDM folder and trim
  - copy Firmware Drivers/STM32F4xx_HAL_Driver stm32f4xx_hal_spi.[ch]
  - uncomment HAL_SPI_MODULE_ENABLED in stm32f4xx_hal_conf.h 
- (finally) add BSP_AUDIO_OUT code.
- made the code a little more musical.  Calling this "done"

## To Do

- perhaps clean up any extra files I don't need.
- should I try a different library than BSP_AUDIO?

## License

The 3-Clause BSD License.  See LICENSE.txt

Copyright 2021 Roger Allen


  