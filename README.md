# MI_XCAM_formalRelease

This is the user application project of using thermal sensor XCAM from Meridian Innovation Limited.
There are two series MCU: M480 & N329-. XCAM is running with USB Video Device Class(UVC), that capbale of streaming video like webcams without installing any specific drivers.

Please read XCAM_Documentation.docx for more details.

SDK Version: Release V3.08 181205

# API

ThermalSensor.h

		void			M480_InitSensor(void);

		void			M480_OpenSensor(void);

		void			M480_StartSensor(void);

===============================================================================

		void			N329_InitSensor(void);

		void			N329_OpenSensor(void);

		void			N329_VideoStreaming(void);

		int			N329_Interface_init (unsigned short interface);

		void			N329_UartDataValid_Handler (UINT8* buf, UINT32 u32Len);

MI_XCAM.h

		void 			InitI2C(unsigned char mode);

		void 			HighDensSequentialRead(unsigned short address,unsigned char* data, unsigned short numbytes);

		void 			HighDensPageWrite(unsigned short address,unsigned char* data, unsigned short numbytes);



		void 			ReadCalibDataN(void);

		void			InitSensorDev(unsigned short TN);

		void 			InitMBITTRIMN(unsigned char user);

		void 			InitBIASTRIMN(unsigned char user);

		void 			InitBPATRIMN(unsigned char user);

		void 			InitPUTRIMN(unsigned char user);

		void 			InitCLKTRIMN(unsigned char user);



		unsigned int 		CalcTO(unsigned int TAmb, signed int dig, signed long PiC, unsigned int dontCalcTA);

		void 			Create_color_table(RGB_COLOR_INFO_T RGB_ColorPalette[],YUV_COLOR_INFO_T YUV_ColorTable[]);

		void	 		DeadPixelMasking (DEADPIXEL pixel);

		FRAMEPOIS 		GetFramePOIs (void);

		void 			GetImageData (void);

		int			GetTargetPixelIndex(void);

		unsigned short		GetTemp(unsigned int x, unsigned int y);

		unsigned short		GetTempDisplay(void);

		void 			ResetFramePOIs(void);

		void			SetTargetPixelIndex(int index);

		void			SetTempDisplay(unsigned short flag);

		unsigned int		StartStreaming(int Mode, char Temps, char Stream);

		
# Known Issues

-	Could not open file with correct existing file path

.\MI_Thermal\MI_XCAM_N329_SW_I2C.axf: error: L6002U: Could not open file ..\..\..\..\ThermalAPI_inC\example\demo\KEIL\demo_Data\N32901_SingleTask\ThermalSensorAPI_N329.lib: No such file or directory

Soultion: Probably it is because the path is too long, which may cause problem. Relocate the file to somewhere with shorter path.

-	(N329) Can I output Uart/Huart data without opening XCAM?

Solution: Move "TempCal()" to outside the usb connected checking statement "if (usbdStatus.appConnected == 1)".

-	How can I remove temperature display?
	
Solution: Control the blackets and Temperature display On/Off inside if(GetTempDisplay() == 1) {...}

-	How to read temperature output from UART?
	
First send UART command VCMD=DAT/AMT/TMP. The output is shown in Hex format. E.g: C9(HEX) = 201(DEC) = 20.1 degree / FFFFC9(HEX) = (FFFFFF - FFFFC9)54(DEC) = -5.4 degree
Further details please refers to the documentation under UART/HUART Interface.

-	Temperature value format

Row * Col * 2 bytes

-	Temperature accuracy

The temperature is not fine tuned at prototyping stage. The accuracy is +-1 degree roughly.

-	How to change color palette?

The color palette is controlled by macros under COLOR_ADAPTIVE.  You can activate adaptive mode with "#define COLOR_ADAPTIVE" in MI_XCAM.h. You can define which color palette is using in ThermalSensor.h.

-	What is the frequency(fps) the XCAM can run?

On N329-
Single:	2-8 fps
Dual:	3-6 fps
