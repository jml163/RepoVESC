#THIS FILE IS AUTO GENERATED FROM THE TEMPLATE! DO NOT CHANGE!
set(CMAKE_SYSTEM_NAME Generic)
set(CMAKE_SYSTEM_VERSION 1)
cmake_minimum_required(VERSION 3.19)
# 设置工具链路径 - 根据您的实际安装位置修改此路径
if(NOT DEFINED ARM_TOOLCHAIN_PATH)
    if(WIN32)
        # Windows 默认安装路径例子，请根据实际情况修改
        set(ARM_TOOLCHAIN_PATH "D:/work2025/MyCLionProject/VESC_Drive/bldc/tools/gcc-arm-none-eabi-7-2018-q2-update/bin")
        #set(ARM_TOOLCHAIN_PATH "C:/Program Files (x86)/GNU Arm Embedded Toolchain/10 2021.07/bin")
        #C:\Program Files (x86)\GNU Arm Embedded Toolchain\10 2021.07\bin\
        # 如果使用了 ARM 官方工具或单独安装的工具链，可能是:
        # set(ARM_TOOLCHAIN_PATH "C:/Program Files (x86)/GNU Tools ARM Embedded/10 2021.10/bin")
    elseif(UNIX)
        # Linux/MacOS 默认安装路径
        set(ARM_TOOLCHAIN_PATH "/usr/local/bin")
    endif()


    # 检查路径是否存在
    if(EXISTS "${ARM_TOOLCHAIN_PATH}")
        message(STATUS "Using ARM toolchain from: ${ARM_TOOLCHAIN_PATH}")
    else()
        # 如果配置的路径不存在，尝试从环境变量PATH中查找
        message(WARNING "Configured toolchain path does not exist: ${ARM_TOOLCHAIN_PATH}")
        message(STATUS "Will try to use toolchain from system PATH")
        set(ARM_TOOLCHAIN_PATH "")
    endif()
endif()
# specify cross compilers and tools
#set(CMAKE_C_COMPILER arm-none-eabi-gcc)
#set(CMAKE_CXX_COMPILER arm-none-eabi-g++)
#set(CMAKE_ASM_COMPILER arm-none-eabi-gcc)
#set(CMAKE_AR arm-none-eabi-ar)
#set(CMAKE_OBJCOPY arm-none-eabi-objcopy)
#set(CMAKE_OBJDUMP arm-none-eabi-objdump)
if(ARM_TOOLCHAIN_PATH)
    set(ARM_TOOLCHAIN_PREFIX "${ARM_TOOLCHAIN_PATH}/")
else()
    set(ARM_TOOLCHAIN_PREFIX "")
endif()

# Windows 需要明确指定 .exe 扩展名
if(WIN32)
    set(EXE_SUFFIX ".exe")
else()
    set(EXE_SUFFIX "")
endif()


# 设置所有交叉编译工具（使用完整路径）
# 设置编译器（明确指定扩展名）
set(CMAKE_C_COMPILER "${ARM_TOOLCHAIN_PREFIX}arm-none-eabi-gcc${EXE_SUFFIX}")
set(CMAKE_CXX_COMPILER "${ARM_TOOLCHAIN_PREFIX}arm-none-eabi-g++${EXE_SUFFIX}")
set(CMAKE_ASM_COMPILER "${ARM_TOOLCHAIN_PREFIX}arm-none-eabi-gcc${EXE_SUFFIX}")
set(CMAKE_AR "${ARM_TOOLCHAIN_PREFIX}arm-none-eabi-ar${EXE_SUFFIX}")
set(CMAKE_OBJCOPY "${ARM_TOOLCHAIN_PREFIX}arm-none-eabi-objcopy${EXE_SUFFIX}")
set(CMAKE_OBJDUMP "${ARM_TOOLCHAIN_PREFIX}arm-none-eabi-objdump${EXE_SUFFIX}")
set(CMAKE_SIZE "${ARM_TOOLCHAIN_PREFIX}arm-none-eabi-size${EXE_SUFFIX}")
set(CMAKE_STRIP "${ARM_TOOLCHAIN_PREFIX}arm-none-eabi-strip${EXE_SUFFIX}")
set(SIZE arm-none-eabi-size)
set(CMAKE_TRY_COMPILE_TARGET_TYPE STATIC_LIBRARY)

#project(ODriveFirmware C CXX ASM)

project(VESC-DRIVE-STM32F4-fw C CXX ASM)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_C_STANDARD 11)




# 定义ARM工具链
#set(ARM_TOOLCHAIN_PATH "" CACHE PATH "ARM compiler path")




set(CMAKE_SIZE ${ARM_TOOLCHAIN_PREFIX}arm-none-eabi-size)
set(CMAKE_OBJDUMP ${ARM_TOOLCHAIN_PREFIX}arm-none-eabi-objdump)


# 可选：定义为变量以便复用
#set(HW_CONFIG_SOURCE "hwconf/trampa/100_250/hw_100_250.c")
#set(HW_CONFIG_HEADER "hwconf/trampa/100_250/hw_100_250.h")

# VESC 4.12 - 适合小电流
set(HW_CONFIG_SOURCE "hwconf/other/vesc4/hw_40.c")           # 改为小电流版本
set(HW_CONFIG_HEADER "hwconf/other/vesc4/hw_40.h")           # 改为小电流版本
set(GIT_BRANCH "master")
set(GIT_HASH "acb8f4b1")
set(ARM_VERSION "7.3.1")

# 使用变量定义宏
add_definitions(
        -DHW_SOURCE="${HW_CONFIG_SOURCE}"
        -DHW_HEADER="${HW_CONFIG_HEADER}"
        -DGIT_BRANCH_NAME="${GIT_BRANCH}"
        -DGIT_COMMIT_HASH="${GIT_HASH}"
        -DARM_GCC_VERSION="${ARM_VERSION}"
)
# 设置编译和链接标志
#set(MCU_FLAGS "-mthumb -mcpu=cortex-m4 -mfloat-abi=hard -mfpu=fpv4-sp-d16")
#set(COMMON_FLAGS "${MCU_FLAGS} -Wall -Wdouble-promotion -Wfloat-conversion -fdata-sections -ffunction-sections -g -Wno-psabi -Wno-nonnull")
#add_definitions(-DSINGLE_AXIS_OPTIMIZATION)

#add_definitions(-DUSE_USB_OTG_FS -DUSE_HAL_DRIVER) #
#add_compile_definitions(USE_USB_OTG_FS)
# for use printf & scanf with float
#set(COMMON_FLAGS "-specs=nosys.specs -specs=nano.specs -u _printf_float -u _scanf_float")
#-DUSE_STDPERIPH_DRIVER -DUSE_STDPERIPH_DRIVER USE_HAL_DRIVER  USE_HAL_DRIVE
#Uncomment for hardware floating point-ok
add_compile_definitions(ARM_MATH_CM4;ARM_MATH_MATRIX_CHECK;ARM_MATH_ROUNDING)

add_compile_options(-mfloat-abi=hard -mfpu=fpv4-sp-d16 -DFPU_FPV4)



add_compile_options(-mcpu=cortex-m4 -mthumb -mthumb-interwork)
add_compile_options(-ffunction-sections -fdata-sections -fno-common -fmessage-length=0)

# 添加重要的警告标志
add_compile_options(
        -O2 -ggdb -fomit-frame-pointer -falign-functions=16 -std=gnu99 -D_GNU_SOURCE
        -DLBM_USE_DYN_FUNS
        -DLBM_USE_DYN_MACROS
        -DLBM_USE_DYN_LOOPS
        -DLBM_USE_TIME_QUOTA
        -DLBM_USE_ERROR_LINENO
        -Wno-psabi          # 抑制GCC 10中关于ABI兼容性的提示
        -Wall               # 启用所有常见警告
    #-Wdouble-promotion  # 警告float隐式转换为double
   # -Wfloat-conversion  # 警告浮点类型间可能丢失精度的转换
       -fsingle-precision-constant
       -Wdouble-promotion
      #-specs=nosys.specs
)
# uncomment to mitigate c++17 absolute addresses warnings -ok
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wno-register")
# 设置链接脚本
#set(LINKER_SCRIPT "${CMAKE_SOURCE_DIR}/Board/v3/STM32F405RGTx_FLASH.ld")
#set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -T${LINKER_SCRIPT}")
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -O2")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O2")
set(CMAKE_ASM_FLAGS "${CMAKE_ASM_FLAGS} -x assembler-with-cpp")
#start fibre protocol
#set(FIBRE_ENABLE_SERVER  1)
# 添加为编译定义
#add_compile_definitions(FIBRE_ENABLE_SERVER=${FIBRE_ENABLE_SERVER})
# 包含目录
include_directories(
        .
         util
         driver
         driver/nrf
         qmlui
         comm
         util/lzo
         ChibiOS_3.0.5/os/hal/ports/common/ARMCMx
         ChibiOS_3.0.5/ext/stdperiph_stm32f4/inc
         ChibiOS_3.0.5/os/common/ports/ARMCMx/devices/stm32f4xx
         ChibiOS_3.0.5/os/hal/ports/STM32/STM32F4xx
         ChibiOS_3.0.5/os/ext/CMSIS/include
         ChibiOS_3.0.5/os/ext/CMSIS/ST
         ChibiOS_3.0.5/os/hal/ports/STM32/LLD
         ChibiOS_3.0.5/os/hal/ports/STM32/LLD/DACv1
         ChibiOS_3.0.5/os/hal/ports/STM32/LLD/GPIOv2
         ChibiOS_3.0.5/os/hal/ports/STM32/LLD/I2Cv1
         ChibiOS_3.0.5/os/hal/ports/STM32/LLD/OTGv1
         ChibiOS_3.0.5/os/hal/ports/STM32/LLD/RTCv2
         ChibiOS_3.0.5/os/hal/ports/STM32/LLD/SPIv1
         ChibiOS_3.0.5/os/hal/ports/STM32/LLD/TIMv1
         ChibiOS_3.0.5/os/hal/ports/STM32/LLD/USARTv1
         #ChibiOS_3.0.5/os/nil/include
         ChibiOS_3.0.5/os/rt/include
         #ChibiOS_3.0.5/os/nil/ports/ARMCMx
         ChibiOS_3.0.5/os/rt/ports/ARMCMx
         ChibiOS_3.0.5/os/rt/ports/ARMCMx/compilers/GCC
        ChibiOS_3.0.5/os/hal/include
        ChibiOS_3.0.5/os/hal/ports/common/ARMCMx
        #ChibiOS_3.0.5/os/hal/osal/nil
        #ChibiOS_3.0.5/os/hal/osal/os-less/ARMCMx
        ChibiOS_3.0.5/os/hal/osal/rt
        ChibiOS_3.0.5/os/various
        ChibiOS_3.0.5/os/hal/lib/streams
        # ChibiOS_3.0.5/os/hal/ports/STM32/LLD/FSMCv1
        motor
        hwconf
        hwconf/luna
        hwconf/other
        lispBM
        lispBM/lispBM/repl
        lispBM/lispBM
        lispBM/lispBM/include
        lispBM/lispBM/platform/chibios/include
        applications
        libcanard
        libcanard/dsdl
        imu
        imu/BMI160_driver
        imu/Fusion
        blackmagic
        blackmagic/target
        encoder

)

add_definitions(-DUSE_STDPERIPH_DRIVER -DSTM32F4 -DSTM32F4xx -DSTM32F405xx -DconfigAPPLICATION_ALLOCATED_HEAP)

# 源文件列表
set(VESC_MOTOR_SOURCES
        motor/foc_math.c
        motor/mc_interface.c
        motor/mcpwm.c
        motor/mcpwm_foc.c
        motor/virtual_motor.c
 )

set(VESC_LISPBM_SOURCES
            lispBM/lispBM/src/env.c
            lispBM/lispBM/src/fundamental.c
	        lispBM/lispBM/src/heap.c
            lispBM/lispBM/src/lbm_memory.c
            lispBM/lispBM/src/print.c
            lispBM/lispBM/src/stack.c
            lispBM/lispBM/src/symrepr.c
            lispBM/lispBM/src/tokpar.c
            lispBM/lispBM/src/extensions.c
            lispBM/lispBM/src/lispbm.c
            lispBM/lispBM/src/eval_cps.c
            lispBM/lispBM/platform/chibios/src/platform_mutex.c
            lispBM/lispBM/src/lbm_channel.c
            lispBM/lispBM/src/lbm_c_interop.c
            lispBM/lispBM/src/lbm_custom_type.c
            lispBM/lispBM/src/lbm_flags.c
            lispBM/lispBM/src/lbm_flat_value.c
            lispBM/lispBM/src/lbm_prof.c
            lispBM/lispBM/src/lbm_defrag_mem.c
            lispBM/lispBM/src/lbm_image.c
            lispBM/lispBM/src/extensions/array_extensions.c
            lispBM/lispBM/src/extensions/math_extensions.c
            lispBM/lispBM/src/extensions/string_extensions.c
            lispBM/lispBM/src/extensions/mutex_extensions.c
            lispBM/lispBM/src/extensions/lbm_dyn_lib.c
            lispBM/lispif.c
            lispBM/lispif_vesc_extensions.c
            lispBM/lispif_c_lib.c
            lispBM/lbm_vesc_utils.c

)

set(VESC_HAL_SOURCES
        ChibiOS_3.0.5/os/hal/src/hal.c
        ChibiOS_3.0.5/os/hal/src/hal_queues.c
        ChibiOS_3.0.5/os/hal/src/hal_mmcsd.c
        ChibiOS_3.0.5/os/hal/src/adc.c
        ChibiOS_3.0.5/os/hal/src/can.c
        ChibiOS_3.0.5/os/hal/src/dac.c
        ChibiOS_3.0.5/os/hal/src/ext.c
        ChibiOS_3.0.5/os/hal/src/gpt.c
        ChibiOS_3.0.5/os/hal/src/i2c.c
        ChibiOS_3.0.5/os/hal/src/i2s.c
        ChibiOS_3.0.5/os/hal/src/icu.c
        ChibiOS_3.0.5/os/hal/src/mac.c
        ChibiOS_3.0.5/os/hal/src/mmc_spi.c
        ChibiOS_3.0.5/os/hal/src/pal.c
        ChibiOS_3.0.5/os/hal/src/pwm.c
        ChibiOS_3.0.5/os/hal/src/rtc.c
        ChibiOS_3.0.5/os/hal/src/sdc.c
        ChibiOS_3.0.5/os/hal/src/serial.c
        ChibiOS_3.0.5/os/hal/src/serial_usb.c
        ChibiOS_3.0.5/os/hal/src/spi.c
        ChibiOS_3.0.5/os/hal/src/st.c
        ChibiOS_3.0.5/os/hal/src/uart.c
        ChibiOS_3.0.5/os/hal/src/usb.c
)
# 直接包含所有 STM32F4xx 平台文件
set(PORTS_STM32F4_PLATFORM_SOURCES
        ChibiOS_3.0.5/ext/stdperiph_stm32f4/src/misc.c
        ChibiOS_3.0.5/ext/stdperiph_stm32f4/src/stm32f4xx_adc.c
        ChibiOS_3.0.5/ext/stdperiph_stm32f4/src/stm32f4xx_dma.c
        ChibiOS_3.0.5/ext/stdperiph_stm32f4/src/stm32f4xx_exti.c
        ChibiOS_3.0.5/ext/stdperiph_stm32f4/src/stm32f4xx_flash.c
        ChibiOS_3.0.5/ext/stdperiph_stm32f4/src/stm32f4xx_rcc.c
        ChibiOS_3.0.5/ext/stdperiph_stm32f4/src/stm32f4xx_syscfg.c
        ChibiOS_3.0.5/ext/stdperiph_stm32f4/src/stm32f4xx_tim.c
        ChibiOS_3.0.5/ext/stdperiph_stm32f4/src/stm32f4xx_iwdg.c
        ChibiOS_3.0.5/ext/stdperiph_stm32f4/src/stm32f4xx_wwdg.c

        ChibiOS_3.0.5/os/hal/ports/common/ARMCMx/nvic.c
        # STM32F4xx 特定文件
        ChibiOS_3.0.5/os/hal/ports/STM32/STM32F4xx/stm32_dma.c
        ChibiOS_3.0.5/os/hal/ports/STM32/STM32F4xx/hal_lld.c
        ChibiOS_3.0.5/os/hal/ports/STM32/STM32F4xx/adc_lld.c
        ChibiOS_3.0.5/os/hal/ports/STM32/STM32F4xx/ext_lld_isr.c

        # STM32 LLD (低级驱动) 文件
        ChibiOS_3.0.5/os/hal/ports/STM32/LLD/can_lld.c
        ChibiOS_3.0.5/os/hal/ports/STM32/LLD/ext_lld.c
        ChibiOS_3.0.5/os/hal/ports/STM32/LLD/mac_lld.c
        ChibiOS_3.0.5/os/hal/ports/STM32/LLD/sdc_lld.c
        ChibiOS_3.0.5/os/hal/ports/STM32/LLD/DACv1/dac_lld.c
        ChibiOS_3.0.5/os/hal/ports/STM32/LLD/GPIOv2/pal_lld.c
        ChibiOS_3.0.5/os/hal/ports/STM32/LLD/I2Cv1/i2c_lld.c
        ChibiOS_3.0.5/os/hal/ports/STM32/LLD/OTGv1/usb_lld.c
        ChibiOS_3.0.5/os/hal/ports/STM32/LLD/RTCv2/rtc_lld.c
        ChibiOS_3.0.5/os/hal/ports/STM32/LLD/SPIv1/i2s_lld.c
        ChibiOS_3.0.5/os/hal/ports/STM32/LLD/SPIv1/spi_lld.c
        ChibiOS_3.0.5/os/hal/ports/STM32/LLD/TIMv1/gpt_lld.c
        ChibiOS_3.0.5/os/hal/ports/STM32/LLD/TIMv1/icu_lld.c
        ChibiOS_3.0.5/os/hal/ports/STM32/LLD/TIMv1/pwm_lld.c
        ChibiOS_3.0.5/os/hal/ports/STM32/LLD/TIMv1/st_lld.c
        ChibiOS_3.0.5/os/hal/ports/STM32/LLD/USARTv1/serial_lld.c
        ChibiOS_3.0.5/os/hal/ports/STM32/LLD/USARTv1/uart_lld.c
)

set(CHIBIOS_HAL_OSAL_SOURCES
        #ChibiOS_3.0.5/os/hal/osal/nil/osal.c
        ChibiOS_3.0.5/os/hal/osal/rt/osal.c
        #ChibiOS_3.0.5/os/hal/osal/os-less/ARMCMx/osal.c
)
set(CHIBIOS_OS_RT_SOURCES
         ChibiOS_3.0.5/os/common/ports/ARMCMx/compilers/GCC/crt1.c
          # 添加关键的启动和向量文件
         ChibiOS_3.0.5/os/common/ports/ARMCMx/compilers/GCC/crt0_v7m.S
        ChibiOS_3.0.5/os/rt/ports/ARMCMx/compilers/GCC/chcoreasm_v7m.S
        ChibiOS_3.0.5/os/common/ports/ARMCMx/compilers/GCC/vectors.c

        ChibiOS_3.0.5/os/rt/ports/ARMCMx/chcore.c
        ChibiOS_3.0.5/os/rt/ports/ARMCMx/chcore_v7m.c
        #ChibiOS_3.0.5/os/rt/src/chstats.c
        ChibiOS_3.0.5/os/rt/src/chsys.c
         ChibiOS_3.0.5/os/rt/src/chdebug.c
         ChibiOS_3.0.5/os/rt/src/chvt.c
         ChibiOS_3.0.5/os/rt/src/chschd.c
         ChibiOS_3.0.5/os/rt/src/chthreads.c
         ChibiOS_3.0.5/os/rt/src/chtm.c
         ChibiOS_3.0.5/os/rt/src/chstats.c
         ChibiOS_3.0.5/os/rt/src/chdynamic.c
         ChibiOS_3.0.5/os/rt/src/chregistry.c
         ChibiOS_3.0.5/os/rt/src/chsem.c
         ChibiOS_3.0.5/os/rt/src/chmtx.c
         ChibiOS_3.0.5/os/rt/src/chcond.c
         ChibiOS_3.0.5/os/rt/src/chevents.c
         ChibiOS_3.0.5/os/rt/src/chmsg.c
         ChibiOS_3.0.5/os/rt/src/chmboxes.c
         ChibiOS_3.0.5/os/rt/src/chqueues.c
         ChibiOS_3.0.5/os/rt/src/chmemcore.c
         ChibiOS_3.0.5/os/rt/src/chheap.c
         ChibiOS_3.0.5/os/rt/src/chmempools.c
        
         ChibiOS_3.0.5/os/hal/lib/streams/chprintf.c
         ChibiOS_3.0.5/os/various/syscalls.c
)

set(VESC_APP_SOURCES
        applications/app.c
        applications/app_ppm.c
        applications/app_adc.c
        applications/app_sten.c
        applications/app_uartcomm.c
        applications/app_nunchuk.c
        applications/app_pas.c
        applications/app_custom.c
)
set(VESC_CANARDSRC_SOURCES
        libcanard/canard.c
        libcanard/canard_driver.c
        libcanard/dsdl/uavcan/equipment/esc/esc_Status.c
        libcanard/dsdl/uavcan/equipment/esc/esc_RawCommand.c
        libcanard/dsdl/uavcan/equipment/esc/esc_RPMCommand.c
        libcanard/dsdl/uavcan/protocol/protocol_GetNodeInfo.c
        libcanard/dsdl/uavcan/protocol/protocol_HardwareVersion.c
        libcanard/dsdl/uavcan/protocol/protocol_NodeStatus.c
        libcanard/dsdl/uavcan/protocol/protocol_SoftwareVersion.c
        libcanard/dsdl/uavcan/protocol/param/param_Empty.c
        libcanard/dsdl/uavcan/protocol/param/param_ExecuteOpcode.c
        libcanard/dsdl/uavcan/protocol/param/param_GetSet.c
        libcanard/dsdl/uavcan/protocol/param/param_NumericValue.c
        libcanard/dsdl/uavcan/protocol/param/param_Value.c
        libcanard/dsdl/uavcan/protocol/file/file_BeginFirmwareUpdate.c
        libcanard/dsdl/uavcan/protocol/file/file_Read.c
        libcanard/dsdl/vesc/vesc_RTData.c
)
set(VESC_IMU_SOURCES
        imu/mpu9150.c
	imu/icm20948.c
	imu/ahrs.c
	imu/imu.c
	imu/BMI160_driver/bmi160.c
	imu/bmi160_wrapper.c
	imu/lsm6ds3.c
	imu/Fusion/FusionAhrs.c
	imu/Fusion/FusionBias.c
        imu/Fusion/FusionCompass.c
)
set( VESC_BLACKMAGIC_SOURCES
         blackmagic/bm_if.c
         blackmagic/swdptap.c
         blackmagic/timing.c
         blackmagic/platform.c
         blackmagic/exception.c
         blackmagic/target/swdptap_generic.c
         blackmagic/target/target.c
         blackmagic/target/adiv5_swdp.c
         blackmagic/target/adiv5.c
         blackmagic/target/cortexm.c
         blackmagic/target/nrf51.c
         blackmagic/target/stm32f1.c
         blackmagic/target/stm32f4.c
         blackmagic/target/stm32h7.c
         blackmagic/target/stm32l0.c
         blackmagic/target/stm32l4.c
)
set(VESC_ENCODER_SOURCES
        encoder/encoder.c
        encoder/encoder_cfg.c
        encoder/enc_abi.c
        encoder/enc_ad2s1205.c
        encoder/enc_as5x47u.c
        encoder/enc_as504x.c
        encoder/enc_bissc.c
        encoder/enc_mt6816.c
        encoder/enc_sincos.c
        encoder/enc_tle5012.c
        encoder/enc_ts5700n8501.c
        encoder/enc_pwm.c
)
set(VESC_Main_SOURCES
        main.c
        irq_handlers.c
        terminal.c
        conf_general.c
        timeout.c
        flash_helper.c
        confgenerator.c
        bms.c
        qmlui/qmlui.c
        conf_custom.c
)
# 添加硬件配置源文件组
set(VESC_HWCONF_SOURCES
        events.c
        hwconf/board.c
        hwconf/luna/bbshd/luna_display_serial.c
        hwconf/luna/m600/luna_m600_display.c
        #${HW_CONFIG_SOURCE}
)
# 添加 VESC 驱动源文件组
set(VESC_DRIVER_SOURCES
        driver/eeprom.c
        driver/i2c_bb.c
        driver/ledpwm.c
        driver/servo_dec.c
        driver/spi_bb.c
        driver/timer.c
        driver/pwm_servo.c
        driver/lora/SX1278.c
        driver/lora/lora.c
        driver/lora/SX1278_hw.c
        driver/nrf/spi_sw.c
        driver/nrf/rf.c
        driver/nrf/rfhelp.c
        driver/nrf/nrf_driver.c
        hwconf/hw.c
        hwconf/drv8301.c
        hwconf/drv8305.c
        hwconf/drv8316.c
        hwconf/drv8320s.c
        hwconf/drv8323s.c
        hwconf/si8900.c
        hwconf/shutdown.c
)
set(VESC_COMM_SOURCES
        comm/commands.c
        comm/comm_usb_serial.c
        comm/comm_usb.c
        comm/comm_can.c
        comm/packet.c
        comm/log.c
)
set(VESC_UTIL_SOURCES
        util/buffer.c
        util/crc.c
        util/digital_filter.c
        util/mempools.c
        util/utils_math.c
        util/utils_sys.c
        util/worker.c
        util/lzo/minilzo.c
)
# 合并所有源文件
set(ALL_SOURCES
        ${VESC_Main_SOURCES}
        ${VESC_ENCODER_SOURCES}
        ${VESC_BLACKMAGIC_SOURCES}
        ${VESC_IMU_SOURCES}
        ${VESC_CANARDSRC_SOURCES}
        ${VESC_APP_SOURCES}
        ${VESC_HAL_SOURCES}
        ${VESC_LISPBM_SOURCES}
        ${VESC_MOTOR_SOURCES}
        ${PORTS_STM32F4_PLATFORM_SOURCES}
        ${CHIBIOS_HAL_OSAL_SOURCES}
        ${CHIBIOS_OS_RT_SOURCES}
        ${VESC_HWCONF_SOURCES}
        ${VESC_DRIVER_SOURCES}
        ${VESC_COMM_SOURCES}
        ${VESC_UTIL_SOURCES}

)

add_compile_definitions(
        USE_STDPERIPH_DRIVER=1
        STM32F405xx
        STM32F4
        HSE_VALUE=8000000

        # 添加 ChibiOS 栈配置（匹配 fw.mk 第69-79行）
        USE_PROCESS_STACKSIZE=0x800        # 主线程栈大小
        USE_EXCEPTIONS_STACKSIZE=0x800     # 异常/中断栈大小
        USE_FPU=hard                       # 硬件浮点
        USE_THUMB=yes                      # Thumb 模式

        # 添加缺失的重要定义（来自 Linux 成功编译）
        BOARD_OTG_NOVBUSSENS=1          # USB OTG 配置
        USE_LISPBM=1                    # LispBM 支持
        CORTEX_USE_FPU=TRUE             # Cortex FPU 使用
        THUMB_PRESENT=1                 # Thumb 指令集
        THUMB_NO_INTERWORKING=1         # 禁用 Thumb 互操作

         # LispBM 配置
        LBM_USE_DYN_FUNS=1
        LBM_USE_DYN_MACROS=1
        LBM_USE_DYN_LOOPS=1
        LBM_USE_TIME_QUOTA=1
        LBM_USE_ERROR_LINENO=1       

        ${STM32_MODEL}
        ARM_MATH_CM$<IF:$<STREQUAL:${STM32_FAMILY},F7>,7,4>
)

set(LINKER_SCRIPT ${CMAKE_SOURCE_DIR}/ld_eeprom_emu.ld)


# 使用这样的写法

#add_link_options(-specs=nosys.specs -specs=nano.specs)
#add_link_options(-Wl,-u,_printf_float)
#add_link_options(-Wl,-u,_scanf_float)

#add_link_options(-Wl,-gc-sections,--print-memory-usage,-Map=${PROJECT_BINARY_DIR}/${PROJECT_NAME}.map)
#add_link_options(-mcpu=cortex-m4 -mthumb -mthumb-interwork)
#add_link_options(-T ${LINKER_SCRIPT})

# 添加这两行解决栈符号未定义问题
# 删除所有 add_link_options 中的库链接
add_link_options(-mfloat-abi=hard -mfpu=fpv4-sp-d16 -DFPU_FPV4)
add_link_options(
        -specs=nosys.specs
        #-specs=nano.specs
        -Wl,-u,_printf_float -Wl,-u,_scanf_float
        -Wl,-gc-sections,--print-memory-usage,-Map=${PROJECT_BINARY_DIR}/${PROJECT_NAME}.map
        -mcpu=cortex-m4 -mthumb 
        -mno-thumb-interwork
        -T ${LINKER_SCRIPT}
        -Wl,--defsym=__main_stack_size__=0x800
        -Wl,--defsym=__process_stack_size__=0x800
)


#link_directories("ThirdParty/CMSIS/Lib/GCC")
#link_libraries("libarm_cortexM4lf_math.a")
# 添加可执行文件
#add_link_options(-lm)
# 使用现代CMake方式链接库
add_link_options(
        # 现有选项...
        -Wl,--defsym=__main_stack_size__=0x800
        -Wl,--defsym=__process_stack_size__=0x800
        # 强制链接整个数学库
        -Wl,--whole-archive -lm -Wl,--no-whole-archive
        -lc -lgcc -lnosys
)
add_executable(${PROJECT_NAME}.elf
        ${ALL_SOURCES}
)
#add_dependencies(${PROJECT_NAME}.elf generate_interfaces)

# 添加后期构建步骤 - HEX文件
add_custom_command(TARGET ${PROJECT_NAME}.elf POST_BUILD
        COMMAND ${CMAKE_OBJCOPY} -O ihex $<TARGET_FILE:${PROJECT_NAME}.elf> ${PROJECT_NAME}.hex
        COMMENT "Creating HEX file"
)

# 添加后期构建步骤 - BIN文件
add_custom_command(TARGET ${PROJECT_NAME}.elf POST_BUILD
        COMMAND ${CMAKE_OBJCOPY} -O binary -S $<TARGET_FILE:${PROJECT_NAME}.elf> ${PROJECT_NAME}.bin
        COMMENT "Creating BIN file"
)

# 显示大小信息
add_custom_command(TARGET ${PROJECT_NAME}.elf POST_BUILD
        COMMAND ${CMAKE_SIZE} $<TARGET_FILE:${PROJECT_NAME}.elf>
        COMMENT "Displaying size information"
)

# 可选 - 生成反汇编文件
if(ENABLE_DISASM)
    add_custom_command(TARGET ${PROJECT_NAME}.elf POST_BUILD
            COMMAND ${CMAKE_OBJDUMP} $<TARGET_FILE:${PROJECT_NAME}.elf> -dSC > ${PROJECT_NAME}.asm
            COMMENT "Creating ASM disassembly file"
    )
endif()

# 可选 - 添加doctest支持
if(DOCTEST)
    file(GLOB TEST_SOURCES Tests/*.cpp)
    add_executable(test_runner ${TEST_SOURCES})
    target_include_directories(test_runner PRIVATE
            .
            MotorControl
            fibre-cpp/include
            Drivers/DRV8301
            doctest
    )
    target_compile_options(test_runner PRIVATE -O3 -std=c++17)
    add_custom_command(TARGET test_runner POST_BUILD
            COMMAND $<TARGET_FILE:test_runner>
            COMMENT "Running tests"
    )
endif()