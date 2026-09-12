<h2 align="center">
  <a href=#><img src="https://raw.githubusercontent.com/armbian/.github/master/profile/logosmall.png" alt="Armbian logo"></a>
  <br><br>
</h2>

# bcmdhd-dkms

## Purpose of This Repository

This repository provides the DKMS source packaging for the Broadcom `bcmdhd` (BCM ap6xxx-series) Wi-Fi driver, forked from BCMDHD 101.10.591.52.27 and adapted for the Rockchip platform. It builds three separate DKMS variants of the driver — one per host bus type (SDIO, PCIe, and USB).

## Packages

The Debian source tree in `debian/` produces three DKMS binary packages, each wrapping the same `src/` tree built with a different bus back-end:

| Package | Bus |
|---|---|
| `bcmdhd-sdio-dkms` | SDIO |
| `bcmdhd-pcie-dkms` | PCIe |
| `bcmdhd-usb-dkms`  | USB  |

The bus variant is selected at build time through the `CONFIG_BCMDHD_SDIO`, `CONFIG_BCMDHD_PCIE`, and `CONFIG_BCMDHD_USB` switches in `src/Makefile`, which determine the resulting module name (`bcmdhd_sdio`, `bcmdhd_pcie`, or `bcmdhd_usb`).

## Repository Layout

```
.
├── src/            # bcmdhd driver sources (C headers, Makefile, Kconfig)
│   └── include/    # Broadcom/802.11/OS abstraction headers
├── debian/         # Debian packaging for the three DKMS variants
└── .github/
    └── workflows/  # CI (see CI overview link below)
```

## Building

The driver is written in **C** and built as a Linux kernel module through **DKMS**. The Debian packages are produced with the standard Debian toolchain (`dpkg-buildpackage`).

To build the `.deb` packages locally on a Debian/Ubuntu system:

```sh
sudo apt-get update
sudo apt-get build-dep --no-install-recommends -y .
dpkg-buildpackage -us -uc
```

The resulting `bcmdhd-{sdio,pcie,usb}-dkms_*.deb` files will be produced in the parent directory. Once installed, DKMS will (re)build the matching `bcmdhd_<variant>.ko` module against the running kernel.

Firmware files (e.g. `fw_bcmdhd.bin`, `nvram.txt`) are not shipped by this repository; the driver expects them under `/lib/firmware/` (see `CONFIG_BCMDHD_FW_PATH` / `CONFIG_BCMDHD_NVRAM_PATH` in `src/Makefile`).

## Continuous Integration

Builds and releases are driven by GitHub Actions. A per-repository overview of workflow runs is available at:

<https://actions.armbian.com/?repo=bcmdhd-dkms>

On tagged commits, CI additionally publishes the built `.deb` artifacts as a GitHub Release.

## License

The driver sources are provided by Broadcom under the terms of the **GNU General Public License, version 2**, with the linking exception described in the headers of `src/Makefile` and the individual source files. See <http://www.broadcom.com/licenses/GPLv2.php> for the upstream license text.

## Related Links

- Armbian project: <https://www.armbian.com>
- Armbian documentation: <https://docs.armbian.com>
