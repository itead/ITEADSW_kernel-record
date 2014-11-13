# linux-sunxi/sunxi-3.4 ITEAD Developing Record

    Author: Wu Pengfei<pengfei.wu@itead.cc>
    Date: 9/25/2014 10:56:59 AM 

--------------------------------------------------------------------------------

# Assumption

- `tree`: the source directory of branch sunxi-3.4 of linux-sunxi at 
          <https://github.com/linux-sunxi/linux-sunxi.git>

--------------------------------------------------------------------------------

# Record Start {

--------------------------------------------------------------------------------

# First commit version `3.4.79+` for ITEAD: 

## `git log`

Last commit:

    commit b6eb2b9b770537ff320c52342174d2bed56b574d
    Author: Luc Verhaegen <libv@skynet.be>
    Date:   Thu Jan 9 22:05:38 2014 +0100
    
        sunxi:nand: add support for samsung K9GBG08U0M
        
        This data was taken from a newer version of libnand, which is only
        available as a binary blob. The flags gave me a bit of trouble, but
        i verified that all of the ones in the current libnand are still
        being used the same way in the new libnand.
        
        Works as well as could be expected :)
        
        Signed-off-by: Luc Verhaegen <libv@skynet.be>

 

# 1. Using config of ITEAD:

Copy files below to `tree/` :

    ITEAD-OS_A10_201405121918_defconfig
    ITEAD-OS_A20_201405130901_defconfig


# 2. Replace linux logo with ITEAD logo

First, make logo.ppm from logo.png

File `png2ppm224.sh`, a script , can make logo.ppm from logo.png. The content of 
`cat png2ppm224.sh` is:  

    #!/bin/bash
    
    ITEAD_LOGO_NAME=${1/.png/}
    
    pngtopnm ${ITEAD_LOGO_NAME}.png > ${ITEAD_LOGO_NAME}.pnm 
    pnmquant 224 ${ITEAD_LOGO_NAME}.pnm > ${ITEAD_LOGO_NAME}224.pnm 
    pnmtoplainpnm ${ITEAD_LOGO_NAME}224.pnm > ${ITEAD_LOGO_NAME}224.ppm
    
    rm -f ${ITEAD_LOGO_NAME}.pnm  ${ITEAD_LOGO_NAME}224.pnm 
    
    echo ${ITEAD_LOGO_NAME}224.ppm generated successfully !!!

***Note:***
You should run the script with `sudo ./png2ppm224.sh logo.png` .

Second, replace `dervers/video/logo/logo_linux_clut224.ppm` with our `logo.ppm`

    cp ../../bin/logo/logo_linux_clut224.ppm drivers/video/logo/


# 3. Commit and Create Tag

    git add *
    git commit -a -m "First commit of ITEAD: Add config files and replace logo by Wu Pengfei"
    git tag -a vITEAD-3.4.79+ -m "commit[e75ce8566de810fabfe109c7c877c7cde57364b6]"


# 4. 打I2S驱动补丁并更新内核到`3.4.103`

## 下载补丁文件：

    网址：http://www.cubieforums.com/index.php?topic=1413.msg13540#msg13540
    文件：0001-I2S-module-rework.patch

## 打补丁

在 `vITEAD-3.4.79+` 打补丁并提交得到 `vITEAD-OS-3.4.79+_with_I2S_patch`

    patch -p1 < ../0001-I2S-module-rework.patch

## 更新内核版本

从`linux-sunxi`更新`sunxi-3.4`到最新版`3.4.103`。

## 修改配置文件，将i2s驱动编译进内核

基于 `ITEAD-OS_A20_201405130901_defconfig` 将配置选项 `SND_SUNXI_SOC_I2S_INTERFACE`
配置为`y`

    Symbol: SND_SUNXI_SOC_I2S_INTERFACE [=m]                                                                                     |  
      | Type  : tristate                                                                                                             |  
      | Prompt: SoC i2s interface for the AllWinner sun4i, sun5i and sun7i chips                                                     |  
      |   Defined at sound/soc/sunxi/i2s/Kconfig:1                                                                                   |  
      |   Depends on: SOUND [=y] && !M68K && !UML && SND [=y] && SND_SOC [=y] && SOUND_SUNXI [=y] && (ARCH_SUN4I [=n] || ARCH_SUN5I  |  
      |   Location:                                                                                                                  |  
      |     -> Device Drivers                                                                                                        |  
      |       -> Sound card support (SOUND [=y])                                                                                     |  
      |         -> Advanced Linux Sound Architecture (SND [=y])                                                                      |  
      |           -> ALSA for SoC audio support (SND_SOC [=y])                                                                       |  
      |             -> SOUND driver for sunxi (SOUND_SUNXI [=y])       
    
保存.config为 `ITEAD-OS_A20_20141029_defconfig` 且提交为 `vITEAD-OS-3.4.103` 。

# 5. 将nand驱动编译进内核[日期：11/4/2014 10:29:33 AM]

基于 `ITEAD-OS_A20_20141029_defconfig` 将nand驱动编译进内核，将配置文件保存为
`ITEAD-OS_A20_20141104_defconfig`。

提交记录为：

    commit 4e7a8ea2a94f879fd79c7e9fb21338f7b1487464
    Author: shennongmin <wupangfee@gmail.com>
    Date:   Tue Nov 4 10:25:07 2014 +0800
    
        Add ITEAD-OS_A20_20141104_defconfig: buildin nand driver.

--------------------------------------------------------------------------------

#Record End }

--------------------------------------------------------------------------------
