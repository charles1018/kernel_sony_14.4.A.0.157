如何重新編譯xperia裝置的核心(zImage)
=================================== 
為了解說和設定交叉編譯工具環境變數方便起見
-----------------------------------
下面範例中的交叉編譯工具目錄以及核心源碼工作目錄都是直接設在家目錄下
-----------------------------------
### 下載交叉編譯工具(cross compiler toolchain)
		git clone https://android.googlesource.com/platform/prebuilts/gcc/linux-x86/arm/arm-eabi-4.7/
### 下載核心源碼並建立工作目錄
		git clone https://github.com/sonyxperiadev/kernel-copyleft xperia_kernel
### 進入工作目錄
		cd xperia_kernel
### 檢查列出目前所有的核心分支版本
		git branch -r
### 切換到你要的核心分支版本
		git checkout origin/14.4.A.0.xxx
### 設定交叉編譯工具的環境變數
		export ARCH=arm
		export CROSS_COMPILE=/home/使用者名稱/arm-eabi-4.7/bin/arm-eabi-
### 設定你要編譯裝置的相關設定的預設值(只有第一次編譯核心時需要執行此動作)
##### 裝置的開發代號可以在檔案README_Xperia裡面找到相關資料
		make rhine_amami_row_defconfig
### 開始編譯核心檔案(zImage),可根據你的電腦cpu調整要用幾個執行緒下去編譯
		make -j4
### 如果編譯過程順利核心(zImage)檔案位在
		arch/arm/boot
____
如何製作boot.img開機檔案
=================================== 
這邊介紹的是比較簡單的方法，
利用高手做好的boot.img解包和打包工具，
將官方原本的boot.img解開來修改和置換我們前面編譯好的核心，
然後打包回成可以刷入手機的boot.img開機檔案。
##### 這邊提供兩個工具請依自己的需要決定要使用哪一種
=================================== 
1.Android Image Kitchen:
-----------------------------------
##### 標準android格式和高通晶片的裝置可以採用此工具
##### xda論壇討論區：
<http://forum.xda-developers.com/showthread.php?t=2073775>
##### 目前最新版本linux系統的Android Image Kitchen工具名稱為：
		AIK-Linux-v1.7-ALL.tar.gz
=================================== 
2.XZDualRecovery
-----------------------------------
##### sony xperia裝置可使用此工具：
##### 可至作者的github下載解壓縮後即可使用
<https://github.com/charles1018/XZDualRecovery>
此工具可將官方的核心檔案kernel.sin解包，修改完後在打包成boot.img開機檔案。
=================================== 
### 這邊以多數裝置都可以通用的Android Image Kitchen來做範:
##### 將工具壓縮檔解壓縮後進入工作目錄，將官方原本的boot.img複製過來，打開終端機輸入解包的指令
		./unpackimg.sh boot.img
##### 此時你可以看到官方的核心檔案位置就在
		AIK-Linux/split_img/boot.img-zImage
##### 將自己編譯出來的核心重新命名為boot.img-zImage置換過去即可
ramdisk目錄裡面的檔案其實就是暫時的根目錄檔案系統 (Root File System)，
要修改開機第一屏畫面，增加recovery功能，busybox等功能都在個目錄裡面。
##### 完成所有的修改後，輸入下列指令打包回boot.img
		./repackimg.sh boot.img
##### 修改好的boot.img會被命名為
		image-new.img
##### 要清除解包和打包過程中所產生的檔案可執行下列指令
		./cleanup.sh
