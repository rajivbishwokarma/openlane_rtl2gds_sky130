# Advanced Physical Design Workshop on OpenLANE using Skywater 130nm PDK

## Introduction
ASIC design is an involved process. In the distant past (few decades ago), ASIC designers were people who designed powerful processors (think Intel 8080 microprocessor) using "[rubylith](https://en.wikipedia.org/wiki/Rubylith), light boards, rulers, electric erasers, and a [digitizer](https://en.wikipedia.org/wiki/Digitization)" [[1]](https://en.wikipedia.org/wiki/Intel_8086). Soon people began realizing how useful these tiny things called processors were in making their lives better and within a year (1974), MITS released their legendary [Altair 8800](https://en.wikipedia.org/wiki/Altair_8800) microcomputer. And, thus, the fascinating story of how humanity's search to automatically add decimal numbers (in 1812 by Charle's Babbage) [3] took a much-needed turn into the right direction, which was to make that technology available to everyday people. This led people to start thinking of new ways to design these pretty tiny but huge pieces of silicon. And that is everyone was rushing to design Electronic Design Automation (EDA) tools and make money out of them. Quickly, within a span of few years, companies started designing tools to design Very Large Integrated Circuits (VLIC) very quickly. And, thus, the very end of 1900s started the very beginning of new technological era, an era that would create the most technological acceleration than what humans had achieved throughout their entire existence. Somewhere in this acceleration, companies that specialized in these EDA tools and chip fabrication were becoming giants providing services to other companies, yet still out of reach of the everyday people. In few decades of their existence, these companies had created proprietary technologies that allowed them to share the whole IC design and fabrication market share between themselves (huge investments lead to huge gains). Then came the open-source revolution for software and hardware. In this open-source revolution, people started contributing to projects that are available in public (including the source and designs) so that everybody could collectively work on it and further the given project (think Linux). Then came the [OpenROAD project](https://openroad.readthedocs.io/en/latest/) in 2018 and with the release of the [Skywater 130nm PDK](https://github.com/google/skywater-pdk), the momentum of open-source IC design is gradually shifting towards the everyday people and that is why we are here, to get started in the path to learning these open-source EDA tools and PDKs.


## Table of Contents
| Day | Module |Part|                          Topic                                       | Status  |
|:---:|:------:|:------:|:--------------------------------------------------------------------:|:-------:|
| 1   |        |       |[Inception of open-source EDA, OpenLANE, and Sky130 PDK]()           |         |
|     | SK1    |       |[Introduction to RISC-V, QFN-48, Physical chip layout]()             |         |
|     | SK2    |       |[Simplified and detailed RTL2GDS flow using OpenLANE]()              |         |
|     | SK3    |       |[Using OpenLANE for synthesizing sample Pico-RISC-V module]()        |         |
| 2   |        |       |[Good floorplan vs bad floorplan and introduction to library cells](https://github.com/rajivbishwokarma/openlane_rtl2gds_sky130#day-2-good-floorplan-vs-bad-floorplan-and-introduction-to-library-cells)        |   :construction:      |
|     | SK1    |       |[Power planning and floor planning]()        |         |
| 3   |        |       |[Design library cell using Magic Layout and ngspice characterization]() |         |
| 4   |        |       |[Pre-layout timing analysis and importance of good clock tree]()      |         |
| 5   |        |       |[Final steps for RTL2GDS using tritonRoute and openSTA]()             |         |

## Day 2: Good floorplan vs bad floorplan and introduction to library cells
###  SK1: Power planning and floor planning
This section covers multiple topics ranging from utilization factor, aspect ratio, pre-placed cells and de-coupling capacitors to power planning and floor planning.

### [+] Utilization factor and aspect ratio

__Utilization factor refers to how much of the total area of the core are you using to place your logic.__ Using the following figure, for example, if a core has an area of 4 * 2 sq. unit and your logic uses four standard cells, each of area 1 sq. unit, which totals to 4 * 1 sq. unit = 4 sq. unit then your utilization factor can be calculated with the following formula.

<p align="center">
<img src="./day2/sk1/rectangle_core.jpg" width=600>
</p>

$$ Utilization\ Factor = \frac{Area\ occupied\ by\ the\ netlist}{Total\ area\ of\ the\ core} $$

For the above given data, 
```
Utilization factor = (4 * 1 sq. unit) / ( 4 * 2 sq. unit) = 0.5
```

**Aspect ratio is the ratio between height to width of your logic area.** Using the data from above example, you can calculate the aspect ratio using the following formula.

$$ Aspect\ Ratio = \frac{Height}{Width} $$

For the example above, 
```
Aspect ratio = 2 unit / 4 unit = 0.5
```

Therefore, utilization factor basically measure how much physical space in the core your logic blocks have used and how much space is remaining for your routing and logic cells. Aspect ratio, on the other hand tells you the shape of your core. 

### [+] Pre-placed cells
These are the modules that are in the top level design and reused multiple times, however, instead of instantiating them in multiple locations they are place in in one location and then connections are routed to and from wherever they are needed. Their location is fixed during the whole design cycle and the automatic placement tool will not move them, rather the automatic placement tool will place other modules by condering these modules. 


### [+] Decoupling capacitors
When the physical distance between the power source and a certain pre-placed cells is high, some power will be lost due to the resistance of the wires. Therefore, we need to surround the pre-placed cells with decoupling capacitors. We do this to make sure that when the output switches from logic zero to logic one, the high output is within noise margin (as shown in left figure below). The capacitors are placed as shown in right figure below.

<p float="left">
    <img width=400 src="./day2/sk1/noise_margin.jpg" >
    <img width=335 src="./day2/sk1/decoupling_cap.jpg" >
</p>

### [+] Power Planning