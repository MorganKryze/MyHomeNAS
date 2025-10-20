# State of the art

## Hardware parts

### Case

- Compact NAS, ITX motherboard form factor, limited power, limited GPU (PAID), 4–5 disks maximum, minimal space footprint. Free and Paid tiers.  
    link: https://www.printables.com/model/590711-nas-itx-pc-case-with-stackable-expansions-modcase / https://www.printables.com/model/714333-modular-4-12-bay-nas-itx-case-modcase-mass
- Large NAS, ATX M-ATX ITX motherboards, no power limitation, <245mm GPU, 12 disks maximum, fair space footprint. Paid  
    link: https://www.printables.com/model/1278847-12-bay-atx-nas-case

### CPU

#### ITX

Source: https://nascompares.com/guide/updated-best-cpumotherboard-combo-for-your-m-itx-nas-build-ecc-pcie-gen-5-4x4-nvme-and-more/

|     |     |     |
| --- | --- | --- |
|     |     |     |
|     |     |     |

#### ATX

Source: https://nascompares.com/guide/recommended-atx-motherboards-for-diy-nas-builds/

Constraints:

- AMD for a better value/price ratio (and f…k Intel 👿)
- TDP < 100W for a low consumption, moderate efficiency server
- 6 cores, 12 threads for a standard CPU with sufficient load capacity
- Frequency > 3GHz for adequate performances
- Socket AM4, for great compatibility with motherboard
- Integrated GPU is a nice to have, but not needed with a dedicated card
- ECC as a nice to have if no big price difference, and depending on compatibility

| CPU chip | Cores / threads | Frequency (GHz) | TDP (W) | Max Memory (GB) | Socket | Integrated graphics | ECC | Price (€) | PC Part Picker | Amazon |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| AMD Ryzen 5 5500GT | 6 / 12 | 3.6 | 65  | 128 | AM4 | Radeon Vega 7 | No  | 120 | [reference](https://pcpartpicker.com/product/VcvD4D/amd-ryzen-5-5500gt-36-ghz-6-core-processor-100-100001489box) | [link](https://amzn.eu/d/btNfppj) |
| AMD Ryzen 5 5600GT | 6 / 12 | 3.6 | 65  | 128 | AM4 | Radeon Vega 7 | No  | 145 | [reference](https://pcpartpicker.com/product/7XGhP6/amd-ryzen-5-5600gt-36-ghz-6-core-processor-100-100001488box) | [link](https://amzn.eu/d/2pCuW6H) |
| [AMD Ryzen 5 5600XT](https://www.amd.com/en/products/processors/desktops/ryzen/5000-series/amd-ryzen-5-5600xt.html) | 6 / 12 | 3.7 | 65  | 128 | AM4 | None | Yes | 140 | [reference](https://pcpartpicker.com/product/GY6NnQ/amd-ryzen-5-5600xt-37-ghz-6-core-processor-100-100001585box) | [link](https://www.amazon.fr/dp/B0DJLWFRCH/?psc=1) |

Constraints:

- AM4 chipset
- 4 DDR4 slots 128GB max

- Support at least one PCIE 4.0 ×16 and PCIE 3.0 ×16 for graphics and controllers (HDD)
- 2 NVMe
- Not too overpriced

- Support ECC optionnal

| Motherboard | PCIE | NVMe | ECC | Price (€) | Technical doc | Amazon |
| --- | --- | --- | --- | --- | --- | --- |
| AsRock B450 Pro4 | 2 PCIE 3.0×16 ; 4 PCIE 2.0 ×1 | 2   | Yes | 87  | [reference](https://www.asrock.com/mb/amd/b450%20pro4/index.asp#Specification) | [link](https://www.amazon.fr/ASROCK-MAINBOARDS-B450-PRO4-R2-0/dp/B08YX38PCV?s=computers&ufe=app_do%3Aamzn1.fos.2a4964d5-da8d-479b-a739-01ef3fadb618) |
| AsRock B550 Pro4 | PCIE 4.0 ×16 ; PCIE 3.0 ×16 ; 2 PCIE 2.0 ×1 | 2   | Yes | 106 | [reference](https://www.asrock.com/mb/AMD/B550%20Pro4/#CPU) | [link](https://www.amazon.fr/dp/B089W2ZTHF) |

### RAM

Constraints:

- Final objective is to max out the 128 GB that our CPU can handle, so we will look for 4 32GB
- For 4 lanes, our max frequency will be 2666 for our CPU
- UDIMM DDR4
- ECC is still a nice to have

| Brand | Price (€) | Price per lane (€) | ECC | Ebay |
| --- | --- | --- | --- | --- |
| Samsung | 519.6 | 129.9 | Yes | [link](https://www.ebay.fr/itm/375115669674?_skw=UDIMM%2032GB%20R2%202666%20DDR4%20ECC&itmmeta=01K6N22GFE40K56T0H5XV7E33W&itmprp=enc%3AAQAKAAABAFkggFvd1GGDu0w3yXCmi1do0g8DbNGAB9YPJ8lu06sk7mTLyqm8fPSAwVEVYY%2F35jgDkCDTuSBsZMYEH4NucLJ9GvynK8Bt5Dki2h6DUULQpuvvn61%2B7HjqhjpcYBMuk9ge5FR%2FSStreEscpvTXjOLK9gC1O1g4loi3ef5U8LnjltJjMYdvJWcfeCkTb0bwQJ2wtdHz1PQO8BpZ5YW3waP8sC3BhphQgLfONeJQnGeSOv3CB7DTzFUFib09V5dzK6QBrDdg6MjCZEH%2Fzj6AqSAOenl%2F%2B2ycl8tUJrLWHr6%2Bjktoz12JNVbf5mmCuaOaRZgELsUh1VhsA2eMJRLmGSM%3D%7Ctkp%3ABk9SR_6HiqK1Zg) |
| Samsung | 498 | 124.5 | Yes | [link](https://www.ebay.fr/itm/364626310639?_skw=UDIMM%2032GB%20R2%202666%20DDR4%20ECC&itmmeta=01K6N22GFFX8KYG0AY866VHRK1&itmprp=enc%3AAQAKAAAA4FkggFvd1GGDu0w3yXCmi1diYqKxBGsnpChj%2FP%2FPabTHzO%2FzvFI4kuHQM%2ByERceTfo9oMjU9XKdAoyGN1crUql%2BwxJ9zMMBUgtGl4oO2qbMLrMnoqg%2BWyvcCvI3i0gs%2B0Y%2F5102LfEmFyNJfsH8eYupPBML7I8odx9BqLDY06tOQtohs6Icb%2Fmlf5vt8jRVa1oA%2FIYRmCPLlmgH4kRgMAoz9C44C3fGpXOFiST1njmuPWb0wK%2F%2BQXW7xptIKF6yWRME5%2BOYcGCWvNstyN%2Bmv1qeDRyLscWFwRv5c1qHRJEyi%7Ctkp%3ABk9SR4CIiqK1Zg) |

### GPU

To reduce consumption, we will focus on an integrated graphic processor instead of an external device. However, having the possibility to add one to the build will be ensured (need one slot of PCIe 4.0 x16).

### Storage

Assuming an array of 4 disks, 1 for parity and 90% of effectiveness. Use of CMR over SMR or HM-SMR recommended.

#### Server Part Deals

| Name | Capacity per disk (TB) | Capacity (TB) | Capacity (TiB) | Price per disk (€) | Price (€) | Price per TB (€) | Link |
| --- | --- | --- | --- | --- | --- | --- | --- |
| WD REF | 20  | 60  | 54  | 242 | 968 | 12.10 | https://serverpartdeals.com/products/western-digital-wd200edgz-20tb-7-2k-rpm-sata-6gb-s-512e-3-5-recertified-hard-drive |
| Exos REF | 22  | 66  | 59.4 | 250 | 1000 | 11.36 | https://serverpartdeals.com/products/seagate-exos-st22000nm000c-22tb-7-2k-rpm-sata-6gb-s-3-5-recertified-hard-drive |
| Exos REF | 24  | 72  | 64.8 | 268 | 1072 | 11.16 | https://serverpartdeals.com/products/seagate-exos-st24000nm000c-24tb-7-2k-rpm-sata-6gb-s-512n-3-5-recertified-hard-drive |
| Exos REF | 26  | 78  | 70.2 | 289 | 1156 | 11.08 | https://serverpartdeals.com/products/seagate-exos-st26000nm000c-26tb-7-2k-rpm-sata-6gb-s-512e-cmr-3-5-recertified-hard-drive |
| Exos REF | 28  | 84  | 75.6 | 328 | 1312 | 11.71 | https://serverpartdeals.com/products/seagate-exos-st28000nm000c-28tb-7-2k-rpm-sata-6gb-s-512e-cmr-3-5-recertified-hard-drive |

#### Amazon

Assuming an array of 5 disks, 1 for parity and 90% of effectiveness. Use of CMR over SMR or HM-SMR recommended.

| Name | Capacity per disk (TB) | Capacity (TB) | Capacity (TiB) | Price per disk (€) | Price (€) | Price per TB (€) | Link |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Exos REF | 14  | 56  | 50.4 | 190 | 950 | 13.57 | https://www.amazon.de/-/en/Seagate-internal-server-ST14000NM001G-7200RPM/dp/B0D6BR9HLV |
| Exos REF | 16  | 64  | 57.6 | 240 | 1200 | 15  | https://www.amazon.fr/-/en/Seagate-Enterprise-Hyperscale-Drive-Number/dp/B0CF5XVHMS |
| Exos REF | 16  | 64  | 57.6 | 220 | 1100 | 13.75 | https://www.amazon.de/-/en/Seagate-Enterprise-Hyperscale-7200rpm-Improved/dp/B0CF5XVHMS |
| Exos REF | 18  | 72  | 64.8 | 274 | 1370 | 15.22 | https://www.amazon.de/-/en/Seagate-Enterprise-Hyperscale-7200rpm-Improved/dp/B0BSZL39NH |
| Exos REF | 26  | 104 | 93.6 | 335 | 1670 | 12.84 | https://www.amazon.de/-/en/Seagate-Internal-Server-ST26000NM000C-7200RPM/dp/B0DHLFXSTZ |
| Exos REF | 28  | 112 | 100.8 | 355 | 1775 | 12.67 | https://www.amazon.de/dp/B0DP4QKYK6 |

### Cache (SSD)

#### SLOG

Need: Mirror of two ssd, low capacity, high reliability.

| Name | Capacity per disk (GB) | Capacity (GB) | Capacity (GiB) | Price per disk (€) | Price (€) | Link |
| --- | --- | --- | --- | --- | --- | --- |
|     |     |     |     |     |     |     |
|     |     |     |     |     |     |     |

### PSU

| Component | Power per unit (W) | Number | Total power (W) |
| --- | --- | --- | --- |
| CPU | 65  | 1   | 65  |
| GPU | 0   | 0   | 0   |
| HDD | 10  | 4-8-12 | 40-80-120 |

Margin: 100W or 10%

Minimal target: 300W

| Brand | Power | Grade | Link |
| --- | --- | --- | --- |
|     |     |     |     |
|     |     |     |     |

## Software

### Operating System

TrueNAS Scale via NVMe SSD.

### Storage

Persistent: RAID 5, one disk is used and lost for parity

Cache: Vdev of 1 SSD