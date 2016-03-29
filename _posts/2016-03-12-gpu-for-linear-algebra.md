---
layout: post
title: GPU for linear algebra
categories:
- blog
--- 

Recently i have prepared a small piece of code which compares a Nvidia `cuSolver` and `scipy.sparse.linalg.spsolve` preformance when solving sparse double precision linear system. Code is [here](https://gist.github.com/mrkwjc/ebb22e8b592122cc8be6). For comparison, i had at my disposal a Nvidia Quadro 2000M GPU and Intel i7-2820QM CPU. It turned out that GPU is a dozen or so couple of times slower than CPU in this computation. And i expected a speedup :worried:. I asked a [question](http://stackoverflow.com/questions/35942068/comparison-of-cuda-and-cpu-sparse-solver-speed-python) to verify if my benchmark code is OK. And people told me that simply i should not expect to much from this relatively old GPU.

Checking this out i discovered that for Quadro 2000M the ratio DP/SP (double precision processing power over single precision) is about 1/24. Having for this card single precision processing power at 422 GFlops we get only 17 Gflops on double precision. The memory bandwidth is also about modest 28.8 GB/s.

So my question is what is the performance of new Nvidia cards? And what to buy to be satisfied not spending to much money? Here is a table of potential choices retrieved from Wikipedia [article](https://en.wikipedia.org/wiki/List_of_Nvidia_graphics_processing_units).

|---
|  | SP | DP | DP/SP | Memory | Bandwidth | Cooling | Cost |
|-|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| | [GFlops] | [GFlops] | - | GB | [GB/s] | | [PLN] |
|Quadro 4000 | 486 | 243 | 1/2 | 2 | 89.6 | active | [800][Quadro4000] |
|Quadro 5000 | 722 | 359 | 1/2 | 2.5 | 120 | active | [1500][Quadro5000] |
|Quadro 6000 | 1028 | 515 | 1/2 | 6 | 144 | active | [1700][Quadro6000] |
|Quadro 7000 | 1332 | 667 | 1/2 | 6 | 177 | ? | ? |
|Tesla C2050 |      |     |     |   |     | active | [700][C2050] |
|Tesla M2050 | 1030 | 515 | 1/2 | 3 | 144 | passive | 700 |
|Tesla C2070 |      |     |     |   |     | active | [1100][C2070] |
|Tesla C2075 |      |     |     |   |     | active | [2000][C2075] |
|Tesla M2070 | 1030 | 515 | 1/2 | 6 | 144 | passive | [250][M2070]  |
|Tesla M2090 | 1331 | 665 | 1/2 | 6 | 177 | passive | [1200][M2090] |
|Tesla S2050 | 4121 | 2060 | 1/2 | 4x3 | 4x148 | passive | ? |
|Tesla S2070 | 4121 | 2060 | 1/2 | 4x6 | 4x148 | | ? |
|Tesla K20(C) | 3524 | 1175 | 1/3 | 5 | 208 | active | [9000][K20] |
|Tesla K20X | 3935 | 1312 | 1/3 | 6 | 250 | passive | 4000 |
|Tesla K40(C) | 5040 | 1680 | 1/3 | 12 | 288 | active | > 12000 |
|Tesla K80 | 8736 | 2912 | 1/3 | 2x12 | 2x240 | passive | > 17000 |
|GeForce GTX Titan | 4500 | 1500 | 1/3 | 6 | 288 | active | 2500 |
|GeForce GTX Titan Black | 5120 | 1707 | 1/3 | 6 | 336 | active | [3000][TitanBlack] |
|GeForce GTX Titan Z | 8121 | 2707 | 1/3 | 2x6 | 2x336 | active | > 10 k |

---

It seems that the obly reasonable choice for my personal use is Tesla C2070 or C2075. Other cards can offer very good double precision performance but with a really high cost.

P.S. The following link is interesting:

* [K20 vs C2070](http://blog.accelereyes.com/blog/2013/02/21/benchmarking-tesla-k20)


[M2090]: http://www.ebay.pl/itm/New-Genuine-Nvidia-Tesla-M2090-6GB-PCI-E-x16-GPU-D-Pn-0775NK-775NK-/281959467482
[M2070]: http://www.ebay.pl/itm/NVIDIA-TESLA-M2070-COMPUTING-PROCESSOR-6GB-GDDR5-CARD-/252292880607
[C2070]: http://www.ebay.pl/itm/New-Nvidia-Tesla-C2070-PCIe-2-0-6GB-1-5GHz-GGRD5-144GB-s-T20-GPU-Accelerator-/272151652533
[C2075]: http://www.ebay.pl/itm/nVidia-Tesla-C2075-6GB-massively-parallel-processing-GPU-Graphics-Card-/121899865225
[C2050]: http://www.ebay.pl/itm/NVIDIA-Tesla-C2050-Graphics-Card-3GB-GDDR5-PCI-e-2-0-x16-DVI-I-/111930668011
[K20]: http://www.ebay.pl/itm/NVIDIA-Tesla-K20-5GB-Kepler-GPU-Computing-Accelerator-C2J97AA-Video-Graphics-/231028386073
[Quadro6000]: http://www.ebay.pl/itm/HP-nVidia-Quadro-6000-Graphics-Card-PCI-Express-2-0-x16-6GB-GDDR5-612953-001-/291704822055
[Quadro5000]: http://www.ebay.pl/itm/NVIDIA-Quadro-5000-2-5GB-GDDR5-PCI-e-x16-DVI-Dual-DP-DELL-0JFN25-NEW-/151993048067
[Quadro4000]: http://www.ebay.pl/itm/nVidia-Quadro-4000-2GB-GDDR5-PCI-E-Video-Card-608533-002-616076-001-WS095AT-HP-/221133838362
[TitanBlack]: http://www.ebay.pl/itm/EVGA-GeForce-GTX-TITAN-BLACK-Superclocked-6GB-ACX-Cooler-Graphics-Card-/301894523238