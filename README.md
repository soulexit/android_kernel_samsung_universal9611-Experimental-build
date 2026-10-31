# Something New — Experimental build

Experimental kernel for the **Samsung Galaxy A51 (a51 / gta4xl)** and other
**Exynos 9611** devices, based on the Samsung `A515F` kernel source.

This tree is the source used for the experimental builds published in the
[Releases](../../releases) section.

## Features

| Item | Value |
|---|---|
| SoC | Exynos 9611 (4x Cortex-A73 + 4x Cortex-A53) |
| GPU | Mali-G72 MP3, Arm DDK **r38p1** (`drivers/gpu/arm/bv_r38p1`) |
| CPU little | 403 – **2054 MHz** (stock 1742 MHz) |
| CPU big | 728 – **2496 MHz** (stock 2314 MHz) |
| GPU G3D | 260 – **1200 MHz** (stock 1053 MHz) |
| Security | SELinux enforcing, seccomp + seccomp filter enabled |
| KernelSU | Not included (stock boot flow) |

The overclock only lifts the ACPM/ECT `MINMAX` and DVFS-domain caps and adds
voltage headroom where a new top level has to be created. If the firmware does
not expose the higher levels, the driver falls back to the stock maximum
automatically.

## Building

Requirements: Linux x86_64, `zip`, and the toolchain installed by `setup.sh`
(WeebX Clang 19 / LLVM).

```bash
# first time only: downloads the toolchain into ./toolchain
bash setup.sh

DEVICE=a51_soulexit ROM=aosp SELINUX=e KSU=0 bash build_kernel.sh
```

The flashable zip is written to:

```
kernel_zip/experimental-<ROM>-<DEVICE>_<E|P>_<YYYYMMDD>.zip
```

Environment variables:

| Variable | Values | Meaning |
|---|---|---|
| `DEVICE` | `a51_soulexit` | defconfig name (`arch/arm64/configs/<DEVICE>_defconfig`) |
| `ROM` | `aosp`, `oneui` | init/ramdisk variant |
| `SELINUX` | `e`, `p` | enforcing / permissive label in the zip name |
| `KSU` | `0`, `1` | KernelSU variant |

## Flashing

1. Flash the zip from the Releases page in a custom recovery (TWRP / OrangeFox).

## Notes

- The AnyKernel3 package only replaces `Image`; the device tree is not included
  in the zip. The kernel handles the overclock in code so it also works with the
  stock DTB.
- The packaged modules are built-in (`MODPOST 0 modules`), no separate module
  flashing is needed.

## Credits

- Samsung / Exynos9611Development — base `android_kernel_samsung_universal9611`
  source: <https://github.com/Exynos9611Development/android_kernel_samsung_universal9611>
- linux4-bringup-priv — `gta4xl` bring-up tree
- osm0sis — AnyKernel3
- Royna (@roynatech) — kernel work and A51 support

## License

GPL-2.0, same as the upstream kernel. See `COPYING`.
