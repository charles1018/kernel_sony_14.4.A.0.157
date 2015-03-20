如何重新編譯xperia裝置的核心(zImage)
=================================== 
為了解說和設定交叉編譯工具環境變數方便起見
下面範例中的交叉編譯工具目錄以及核心源碼工作目錄都是直接設在家目錄下

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
### 待續...
