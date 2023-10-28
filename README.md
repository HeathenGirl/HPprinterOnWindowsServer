# HPprinterOnWindowsServer
## How to install drivers for HP inkjet printers on Windows Server 2022
Some printers refuse to work with Windows Server. If one tries to install printer driver, a message appears stating that this OS is not supported. For example, when you run the Setup.exe from the HP DeskJet 2320 printer driver, the following window appears:  
![alt text](https://i2.imageban.ru/out/2023/06/14/4fe4758ac06b718c47fcda288b318d2b.jpg)  
In this case, you will not be able to install the driver directly, through the installer. You can install it on Windows Server like this.  
First you need to install the driver through the official installer on another PC or inside a Windows 10/11 virtual machine. In this case, during the installation process, you need to skip the step of connecting the printer via a USB cable:  
![alt text](https://i5.imageban.ru/out/2023/06/14/a331dd76a0132248bfd8ebef9d9a30eb.jpg)  
After installing the driver and rebooting, open PowerShell and type:  
`Get-WindowsDriver –Online| select Driver, ClassName, BootCritical, ProviderName, Date, Version, OriginalFileName|Out-GridView`  
This will list all drivers installed by the "third party". The following window will appear:  
![alt text](https://i5.imageban.ru/out/2023/06/14/cd293e5f8bcec2733e12579718ebdbd2.jpg)  
As you can see, the installer installed four drivers for the HP DeskJet 2320:  
oem1.inf (original name hpygid31_v4.inf) - printer driver,  
oem2.inf (original name hpwia_dj2300.inf) - scanner driver,  
oem3.inf (original name hpreststub.inf) - USB driver,  
oem4.inf (original name hpwinusbstub.inf) - USB driver.  
All these four drivers need to be exported to pre-prepared folders. Open command prompt and type:  
`pnputil.exe /export-driver oem1.inf C:\HPDeskJet2320\1  
pnputil.exe /export-driver oem2.inf C:\HPDeskJet2320\2  
pnputil.exe /export-driver oem3.inf C:\HPDeskJet2320\3  
pnputil.exe /export-driver oem4.inf C:\HPDeskJet2320\4`  
Upon successful export, a message will appear:  
