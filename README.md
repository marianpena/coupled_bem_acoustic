# Coupled BEM acoustic

This code provides an implementation to address the acoustic scattering problem of two
penetrable scatterers under a BEM philosophy, using planar triangular meshes.
Tested under Julia 1.1.
Details from the theoretical formulation are in the work:
[Boundary element method to analyze acoustic scattering from a coupled swimbladder-fish body configuration](https://www.sciencedirect.com/science/article/abs/pii/S0022460X20304405?via%3Dihub).

A preprint version of this work exists at the [arXiv](https://arxiv.org/abs/1909.11781)

## Required packages

The Julia packages `SpecialFunctions`, `DelimitedFiles`, `Distributed` and `LinearAlgebra` are required,
so make sure you have them installed in your Julia environment.

## Using the code

The code is organized in one main file
```
jul_main.jl
```
which must be loaded previously to any script-run. This file loads all the necessary
files (which reside in the `src` directory).
In this file the user can configure its parallel environment (i.e. how many processors
he has in the machine running Julia). The line `n_cores` defines how many cores are 
added to the main processor. For example, in a machine with 4 cores `n_cores=3` is the
allowable maximum value that makes sense. I recommend using as many cores as you can.

Once loaded the main file, by typing
```
include("jul_main.jl")
```
you are ready to execute the two-spheres example provided (backscattering from two
penetrable spheres) that corresponds to Fig. 3 (see details in the arXiv or the
JSV paper). This is accomplished by typing
```
include("Script_Shells_bem_K.jl")
```
During the script execution  some informative text will be displayed at the screen. 
Lines as
```
Calculating the frequency : 15206.520916753354 ::: 9 of 10
```
allow to see the current stage of the calculation and how much work remains to be
done (i.e. frequencies to be calculated).

When done (with 3 i5-3570@3.4 GHz cores took me one hour to calculate ten points) the
resulting TS versus frequency is saved in the `out` directory.
This result can be seen in the following figure, compared against the benchmark
solution.

![image info](fig_test_spheres.png)

The parameters from the execution can be altered in the script file. The `meshes`
directory contains the STL format meshes corresponding to the scatterers.

## General considerations

* This code is not friendly end-user software.
* This code is a research code aimed to implement acoustic scattering.
* I am not a programmer, I'm a physicist that solves stimulating physical problems 
aided with the computer. There is a gap in between.
* Concerning scattering, a mesh is an appropriate representation of the scattering
object only in a range of frequencies. Make sure that a proper relation
between the acoustic wavelength and the triangle's longitude in the mesh is
fulfilled (some details in the paper).

## Memory considerations

* This BEM formulation ensambles a matrix of size `2(N+L) x 2(N+L)`, where `N` and `L`
are the sizes (numbers of triangles) of the meshes. Bearing in mind this constraint 
because by default each element is a 128 bits complex (64 bits for each, real and 
imaginary parts). Avoid a matrix size which surpasses your physical memory.

## Citing the code?

If you have used the code and find useful for you research work, please cite the article
> Boundary element method to analyze acoustic scattering from a coupled swimbladder-fish body configuration.

A bibtext entry follows:
```
@article{gonzalez2020boundary,
  title={Boundary element method to analyze acoustic scattering from a coupled swimbladder-fish body configuration},
  author={Gonzalez, Juan D and Lavia, Edmundo F and Blanc, Silvia and Maas, Martin and Madirolas, Adri{\'a}n},
  journal={Journal of Sound and Vibration},
  pages={115609},
  year={2020},
  publisher={Elsevier}
}
```

## Final words

Thank you for tryng the code. Other example scripts will be uploaded soon!
Any suggestions and comments will be welcome at sivasadartantasvueltasnosirve@gmail.com.
