####Hokuyo Multiecho
* Hokuyo typically use [firmware version 1.1.5](https://www.hokuyo-aut.jp/02sensor/07scanner/download/products/utm-30lx-ew/data/change_log_en.txt)
* 1.1.5 supports:
  * significantly improved multiecho
  * but you cannot get 40Hz and 270degree 
  * 25% loss of data: 200 degree horizontal or 30Hz

* CRL use firmware 1.1.4 (which is an unofficial version)
  * this version bricks the newest devices as they recently changed the microcontroller on board

* Supporting multiecho (e.g. patching 1.1.4 or moving to 1.1.5):
  * would require a change the cpu code running on the CRL unit
  * changes in CRL ros drivers
  * significant engineering effort

####IR Windows
There are two windows on the device:
- Normal Internal IR-filtering gorilla windows were **not installed** during manufacture (between lens and imager)
- Normal front window on exterior of device was installed
- a replacement to the normal front window with IR coating was supposed to be provided to NASA
  - ***was not actually shipped*** (issue wasn't forgotten about at CRL)
  - CHRIS OSTERWOOD WILL LOOK INTO WHERE THESE IR LENSES ARE

- IR coated window is supposed to be user swapable for usual front window:
  - this front window which compresses against the O-rings of the pressurised internal chamber.

Effect of IR windows:
- its possible that there would be focus shift
- lens are tuned to visible light, when operate with alot of IR things can be out of focus.

Chris suggested reverting the whole idea would be possible 'gratis': i.e. put the IR window back in at CRL. He said they have had little progress in finding a supplier for the IR projector interested in what we can learn about IR windows

####30/15HZ IMAGE CROPPING
Currently the 4MP cameras are limited to 15Hz due to the amount of data. A firmware update would allow the device to achieve on-demand 30Hz by cropping the images to 2MP
https://bitbucket.org/crl/multisense_ros/issues/15/stereo-resolution-cannot-be-set-above
* Maurice: this is long promised (8 months)
* CHRIS: will look into how long the 30Hz firmware update will take.


Most of this content is from a phone call with Chris Osterwood in Dec 2015