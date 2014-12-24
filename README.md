## Dump Wechat Messages from Android

### How to use:

#### Install Dependencies:
+ python-PIL
+ [PyQuery](https://pypi.python.org/pypi/pyquery/1.2.1)
+ [eyed3](http://eyed3.nicfit.net/)
+ sox
+ python-csscompressor(optional)

#### Get Necessary Data:
+ Get /data/data/com.tencent.mm/MicroMsg/long-long-name/EnMicroMsg.db from root filesystem, possible ways are:
	+ `adb root`; `adb pull /data/data/com.tencent.mm/MicroMsg/long-long-name/EnMicroMsg.db`
	+ Use your rooted file system manager app
+ Get WeChat user resource directory from user filesystem, possible ways:
	+ `adb pull /mnt/sdcard/tencent/MicroMsg/long-long-name` # location could be different
+ Get Wechat uin, possible ways are:
	+ Login to [web-based wechat](https://wx.qq.com); get wxuin=1234567 from `document.cookie`
	+ `adb pull /data/data/com.tencent.mm/shared_prefs/system_config_prefs.xml`;
	`grep 'default_uin' system_config_prefs.xml | grep -o 'value="[0-9]*' | cut -c 8-`


+ Get phone IMEI, possible ways are:
	+ Call `*#06#` on your phone
	+ Find IMEI in system settings
	+ `adb shell dumpsys iphonesubinfo | grep 'Device ID' | grep -o '[0-9]*'`

#### Run:
+ Decrypt database, will produce decrypted_db.db (for now, Linux x64 only):
```
./decrypt_db.sh <path to EnMicroMsg.db> <imei> <uin>
```
+ Parse and dump text messages of every contact:
```
./dump_msg.py decrypted_db.db output_dir
```
+ Dump messages of one contact to single-file html, containing voice messages and images:
```
./dump_html.py decrypted_db.db <resource directory> <contact name> output.html
```

### TODO
+ Group message
+ Change max size for custom emoji
+ Fix smiley spacing in html
+ Show name of emoji in text output
+ Search by uid/username..
