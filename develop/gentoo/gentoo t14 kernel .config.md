## CPU

[ryzen](https://wiki.gentoo.org/wiki/Ryzen)
[AMD microcode](https://wiki.gentoo.org/wiki/AMD_microcode)

``` bash
uname -p
AMD Ryzen 7 PRO 8840U w/ Radeon 780M Graphics

grep -m2 -P "(cpu family)|model[^ ]" /proc/cpuinfo
cpu family : 25
model : 117

### cpuid (sys-apps/cpuid)
cpuid -i | grep uarch
   (uarch synth) = AMD Zen 4, TSMC 4nm
   (uarch synth) = AMD Zen 4, TSMC 4nm
   (uarch synth) = AMD Zen 4, TSMC 4nm
   (uarch synth) = AMD Zen 4, TSMC 4nm
   (uarch synth) = AMD Zen 4, TSMC 4nm
   (uarch synth) = AMD Zen 4, TSMC 4nm
   (uarch synth) = AMD Zen 4, TSMC 4nm
   (uarch synth) = AMD Zen 4, TSMC 4nm
   (uarch synth) = AMD Zen 4, TSMC 4nm
   (uarch synth) = AMD Zen 4, TSMC 4nm
   (uarch synth) = AMD Zen 4, TSMC 4nm
   (uarch synth) = AMD Zen 4, TSMC 4nm
   (uarch synth) = AMD Zen 4, TSMC 4nm
   (uarch synth) = AMD Zen 4, TSMC 4nm
   (uarch synth) = AMD Zen 4, TSMC 4nm
   (uarch synth) = AMD Zen 4, TSMC 4nm

### make.conf
-march=native 또는 -march=znver4
```

```
## Processor type and features
CONFIG_SMP = y
CONFIG_X86_X2APIC = y
CONFIG_X86_AMD_PLATFORM_DEVICE = y
CONFIG_SCHED_MC = y
CONFIG_X86_MCE = y
CONFIG_X86_MCE_AMD = y
CONFIG_PERF_EVENTS_INTEL_RAPL = y

## Power management and ACPI options
CONFIG_CPU_FREQ_DEFAULT_GOV_ONDEMAND
CONFIG_X86_ACPI_CPUFREQ = y
CONFIG_X86_ACPI_CPUFREQ_CPB = y
CONFIG_X86_AMD_FREQ_SENSITIVITY = y
CONFIG_CPU_FREQ_DEFAULT_GOV_SCHEDUTIL = y
CONFIG_X86_AMD_PSTATE = y

## Device Drivers
CONFIG_FW_LOADER = y
CONFIG_EXTRA_FIRMWARE = "amd-ucode/microcode_amd_fam19h.bin"
CONFIG_EXTRA_FIRMWARE_DIR = "/lib/firmware"
CONFIG_HWMON = y
CONFIG_SENSORS_K10TEMP = y
CONFIG_IOMMU_SUPPORT = y
CONFIG_AMD_IOMMU_V2 = y
CONFIG_I2C_PIIX4 = y
CONFIG_PINCTRL = y
CONFIG_PINCTRL_AMD = y
CONFIG_CRYPTO = y
CONFIG_CRYPTO_HW = y
CONFIG_CRYPTO_DEV_SP_CCP = y
CONFIG_CRYPTO_DEV_CCP_CRYPTO = m
CONFIG_CRYPTO_DEV_CCP_DD = y
CONFIG_CRYPTO_DEV_SP_PSP = y
CONFIG_X86_PLATFORM_DEVICES = y
CONFIG_AMD_PMC = y
```

## GPU

[amdgpu](https://wiki.gentoo.org/wiki/AMDGPU)

```
### make.conf
VIDEO_CARDS="-* amdgpu radeonsi"

### 또는 package.use/00video
*/* VIDEO_CARDS: -* amdgpu radeonsi

###
VDPAU, VAAPI는 radeonsi 를 통해서 지원됨
```

Module로 빌드해야 쉬움

```
## Processor type and features
CONFIG_MTRR = y

## Memory management options
CONFIG_MEMORY_HOTPLUG = y
CONFIG_MEMORY_HOTREMOVE = y
CONFIG_ZONE_DEVICE = y
CONFIG_DEVICE_PRIVATE = y

## Device Drivers
CONFIG_DRM = m
CONFIG_DRM_FBDEV_EMULATION = y
CONFIG_DRM_AMDGPU = m
CONFIG_DRM_AMD_ACP = y
CONFIG_DRM_AMD_DC = y
CONFIG_HSA_AMD = y
CONFIG_HSA_AMD_SVM = y
CONFIG_SOUND = m
CONFIG_SND = m
CONFIG_SND_PCI = y
CONFIG_SND_HDA_INTEL = m
CONFIG_SND_HDA_PATCH_LOADER = y
CONFIG_SND_HDA_CODEC_HDMI = m
CONFIG_FW_LOADER = y

## amd-ucode/microcode_amd_fam19h.bin 포함
## 커널 빌드 후, 필요한 모듈 dmesg -t | grep amdgpu | grep firmware 로 확인 가능함
CONFIG_EXTRA_FIRMWARE = "amdgpu/psp_13_0_4_ta.bin amdgpu/psp_13_0_4_toc.bin amdgpu/dcn_3_1_4_dmcub.bin amdgpu/gc_11_0_1_imu.bin amdgpu/gc_11_0_1_me.bin amdgpu/gc_11_0_1_mec.bin amdgpu/gc_11_0_1_mes1.bin amdgpu/gc_11_0_1_mes_2.bin amdgpu/gc_11_0_1_mes.bin amdgpu/gc_11_0_1_pfp.bin amdgpu/gc_11_0_1_rlc.bin amdgpu/vcn_4_0_2.bin"
CONFIG_EXTRA_FIRMWARE_DIR = "/lib/firmware"
```

커널 파라미터에 `amdgpu.dpm=1`으로 DPM을 활성화 할 수 있음 (default: `dpm=-1`(auto))

`x11-apps/mesa-progs`, `sys-apps/amdgpu_top`으로 디버깅 가능

```
## firmware
media-libs/amdgpu-pro-amf

### /etc/portage/package.use/mesa
media-libs/mesa -opencl vulkan

### /etc/portage/package.accept_keywords/rocm-opencl-runtime
dev-libs/rocm-opencl-runtime ~amd64
```


## Ethernet

## Wifi

`CONFIG_ATH11K=m`

## touchpad

[thinkpad gentoo wiki](https://wiki.gentoo.org/wiki/Lenovo_ThinkPad_X1_Carbon_7th_generation)
[synaptics gentoo wiki](https://wiki.gentoo.org/wiki/Synaptics)

```
### Device drivers
```

## ACPI

[acpi](https://wiki.gentoo.org/wiki/ACPI)
[thinkpad special buttons](https://wiki.gentoo.org/wiki/ACPI/ThinkPad-special-buttons)

``` bash
emerge -avq sys-power/acpid

## acpid start
/etc/init.d/acpid start
rc-update add acpid default
```