## MSMG options and procedure to build Windows 10 ISO

* Download [Media Creation Tool](https://www.microsoft.com/en-gb/software-download/windows10) and create Windows ISO
* Mount Windows ISO
* Copy contents to DVD Folder
* Start MSMG (right click Start "Run as Administrator")
* __[2]__ Convert 
  * __[2]__ Convert ESD Image to WIM Image
  * Select Architecture
* __[1]__ Source
  * __[1]__ Select source from DVD folder
  * Select image number
  * __[Y]__ to mount Window Setup Boot Image
  * __[Y]__ to mount Windows Recovery Image
* __[4]__ Remove 
  * __[1]__ Remove Default Metro Apps 
    * __[6]__ All apps except store app
  * __[3]__ Remove Windows Components
    * __[A]__   Microsoft Connect App
    * __[B]__   Microsoft OneDrive Desktop Client
    * __[C]__   Microsoft Skype ORTC
    * __[D]__   Windows Content Delivery Manager
    * __[F]__   Windows Embedded Features
    * __[H]__   Windows Mixed Reality
    * __[I]__   Windows Quick Assist App
    * __[J]__   Windows Take Test App
    * __[2]__
      * __[E]__   Windows Cortana & StartMenu Search App
      * __[G]__   Windows Media Player
      * __[L]__   Microsoft Telemetry
      * __[N]__   Windows People Experience Host
      * __[O]__   Windows SmartScreen
