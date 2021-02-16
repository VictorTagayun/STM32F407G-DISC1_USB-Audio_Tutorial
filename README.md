# STM32F407G-DISC1_USB-Audio

### CubeMX  
1. set all pins default
2. C0nnectivity >> USB_OTG_FS : **MODE** : _Device Only_  
3. Middleware >> USB_Device : **Class for FS IP** : _Audio Device Class_  
  **>>at this stage, you can try to flash and check if windows can recognize as USB sound device**
4. copy BSP Driver folder from _Repository_ to your project
5. delete other folders exept _STM32F4-Discovery_ and _Components_
6. inside project folder _Components_ folder, delete all except _Common_ and _cs43l22_
7. inside project folder_Common_ folder, delete all _audio.h_
8. copy _STM32_Audio_ from _Middlewares\ST_
9. inside project folder _Drivers\BSP\STM32F4-Discovery_, delete _stm32f4_discovery_accelerometer.c/h_
10. in Project "Paths & Symbols" add the ff:  
    Drivers\BSP\Components\Common  
    Drivers\BSP\Components\cs43l22  
    Drivers\BSP\STM32F4-Discovery
11. inside project folder _USB_DEVICE\App_ edit _usbd_audio_if.c_, add the ff in the coresponding functions:  
    a. **AUDIO_Init_FS()** add _BSP_AUDIO_OUT_Init(OUTPUT_DEVICE_AUTO, Volume, AudioFreq);_ and comment the rest  
    b. **AUDIO_DeInit_FS()** add _BSP_AUDIO_OUT_Stop(CODEC_PDWN_SW);_ and comment the rest     
    c. **AUDIO_AudioCmd_FS()** add  
    	switch (cmd) {  
		    case AUDIO_CMD_START:  
			    BSP_AUDIO_OUT_Play((uint16_t *)pbuf, size);  
			    break;  
		    case AUDIO_CMD_PLAY:  
			    BSP_AUDIO_OUT_ChangeBuffer((uint16_t *)pbuf, size);  
			    break;  
		    case AUDIO_CMD_STOP:  
			    BSP_AUDIO_OUT_Stop(CODEC_PDWN_SW);  
			    break;  
	    }  
  and comment the rest  
  d. **AUDIO_VolumeCtl_FS()** add _BSP_AUDIO_OUT_SetVolume(vol);_ and comment the rest  
  e. **AUDIO_MuteCtl_FS()** add _BSP_AUDIO_OUT_SetMute(cmd);_ and comment the rest  
  f. **AUDIO_MuteCtl_FS()** add _BSP_AUDIO_OUT_SetMute(cmd);_ and comment the rest  
  g. in **main.h** add the ff:  
      #include "stm32f4_discovery_audio.h"  
      
