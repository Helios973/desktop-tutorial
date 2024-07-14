##Note: instructions bellow is based on windows, if you are on OS X / Linux, you should change addresses according to your operating system
  be sure that the .jar file full path doesn't contain any spaces
  See FAQ bellow for more information
---------------------------------------------------

1) Copy FineAgent.jar file to root of drive C
	(So path of FineAgent.jar should be C:\FineAgent.jar)

2) Locate .vmoptions file of desired JetBrains product:
	This is located in the bin directory of your JetBrains IDE. For example:
	C:\Program Files\JetBrains\IDEA\bin\idea64.exe.vmoptions

	Or, if you use Custom VM Options, the path would be, for example:

	C:\Users\%username%\AppData\Roaming\JetBrains\IntelliJIdea2021.3\idea64.exe.vmoptions

	So if something doesn't work, make sure you change the right file (custom takes precedence over the one in the bin directory)

3) add following line to the end of the .vmoptions file:

-javaagent:C:\FineAgent.jar

4) Run JetBrains product, Select "Activation Code" as licensing method & use Content of ActivationCode.txt file.

Done!

---------------------------------------------------
FAQ:
Q: This license YM4BWY5IKL has been cancelled.
A: 
Edit the correct .vmoptions. Edit the file for the platform you are using (x86, x64). Remember that .vmoptions located in C:\Users\Name\AppData\Roaming\JetBrains\ProductNameXXXX.X\product64.exe.vmoptions has priority over the one located in C:\Program Files\JetBrains\ProductNameXXXX.X\bin\product64.exe.vmoptions. 
Check that the javaagent .jar path does not contain spaces. The line starts with a "-" and it is the only one in the file, the others are commented out with a "#" at the beginning of the line.
Make sure there is a file janf_config.txt with a filter rule in the directory near the .jar. If you are using a system other than Windows, check that the line ends match your system.


Q: Where is the .vmoptions file on OS X / Linux?
A: Use the menu item in the IDE: Gear Icon -> Edit custom VM options
	Note: to access this, you have to activate jetbrains product in trial by creating a free account on JetBrains website & the Activate it fully.
