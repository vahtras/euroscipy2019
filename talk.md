<script type="text/javascript"
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>

<script type="text/x-mathjax-config">
  MathJax.Hub.Config({
    tex2jax: {
      inlineMath: [ ['$','$'], ["\\(","\\)"] ],
      processEscapes: true
    }
  });
</script>


# VeloxChem
## Python meets quantum chemistry and HPC

### EuroSciPy2019

### Olav Vahtras 


---

layout: false

## About <a href="https://www.kth.se/profile/vahtras">me</a>

* Professor of Theoretical Chemistry
* KTH Royal Institute of Technology <img src="https://kth.se/polopoly_fs/1.77257.1550147376!/KTH_Logotyp_RGB_2013-2.svg" * alt="KTH:s logotyp" height="40" width="40"> 
* Software Carpentry/Data Carpentry <img src="https://carpentries.org/assets/img/TheCarpentries.svg" height="40">
* Co-editor of <a href="http://www.scipy-lectures.org/">SciPy Lecture Notes</a>
* Teaching Python at master and Ph.D. levels
* Background in HPC

<img src="pdc.png" height="300">
---

## What we do

<img src="qmmm.png" height="500">

---
## Solving the Schrödinger equation

$$
(H + V)\Psi = i\hbar\frac{\partial\Psi}{\partial t}
$$

* Determine ground state
* Molecular properties from perturbation theory 

---

## Paul M. Dirac

<img src="https://upload.wikimedia.org/wikipedia/commons/5/50/Paul_Dirac%2C_1933.jpg" height="300">

*The underlying physical laws necessary for the mathematical theory of a large
part of physics and the whole of chemistry are thus completely known, and the
difficulty is only that the exact application of these laws leads to equations
much too complicated to be soluble.* 

*It therefore becomes desirable that
approximate practical methods of applying quantum mechanics should be
developed, which can lead to an explanation of the main features of complex
atomic systems without too much computation.*

Proc.Roy.Soc. A 123, 94 (1929)

---

## Numerical bottleneck: integrals

* Discrete representation of state -> basis functions
* Partial differential equations -> Matrix equations
* Electron-electron interactions represented by 4-dimensional tensors


$$ F[p, q] = \sum_{rs} L[p, q, r, s] D[r, s] $$

* Typically the part that is parallelized in QC codes

---

## What we do in practice

* Solve large eigenvalue equations

$$
Ex = \lambda x
$$

* Solve large linear systems of equations

$$
(E-\omega S)N = V^\omega
$$

<img src="spectrum.png" height="200">

* Typical dimension $10^6$
* Matrix too large to store
* Solved by iteration

---
## Iterative solvers

* project equation on a reduces space of trial vectors

$$ (b^T(E-wS)b) (b^T N) = (b^T V) $$

* solve small problem with `numpy.linalg.solve` (lapack/mkl)

* calculate residual error - converged when small enough

$$ \tilde N = b(b^T N) $$
$$ R = (E - wS)\tilde N  - V$$

* generate new trial vector and repeat

---

## Our codes

<img src="bg_header.jpg" height="200">

* The Dalton project http://www.daltonprogram.org
* Research development tool since 1980s
* Mostly Fortran/C
* 1M *SLOC*
* Open-source since 2016
* Successful (mostly) Scandinavian collaboration
* Code has evolved organically over > 30 years

---

### Problems

* No unit tests
* A number of (long) end to end tests
* Difficult to modify/extend



### However

* A lot of man-years invested
* A lot of functionality that will never be reimplemented
* Code will not go away
* Still ok for teaching

---

## Introducing VeloxChem

* First release October 2019
* Python driven 
* Modern C++ implementations with MPI/OpenMP

<a href="https://veloxchem.org"><img src="vlx2.png" height="300"></a>

* https://veloxchem.org
* https://docs.veloxchem.org 

---

<img src="code-structure_alt.svg" height="400">

* High-level logic in Python
* Low-level numerical work in (C++ Pybind11)
* Combination of pytest and Google test unit testing
* Hosted on gitlab with CI

~~~
$ pip install veloxchem # after release
$ mpirun -N 100 python -m veloxchem molecule.inp molecule.out
~~~

---

### Status

* Manuscript submitted to Wiley Interdisciplinary Reviews: Computational Molecular Science
* VeloxChem: a Python-driven density-functional theory program for spectroscopy simulations in high-performance computing environments 

* Authors
    * Zilvinas Rinkevicius, 
    * Xin Li, 
    * Olav Vahtras, 
    * Karan Ahmadzadeh
    * Manuel Brand
    * Magnus Ringholm, 
    * Nanna Holmgaard List
    * Maximilian Scheurer
    * Mikael Scott, 
    * Andreas Dreuw, 
    * Patrick Norman

* GPU work in progress

---

## Summary

* VeloxChem - a new quantum chemistry program aimed at spectroscopies
* Open source
* Easy to obtain and install (pip)
* Runs on large-scale HPC clusters
* Highly competitive with other similar softwares
* First release October 2019

<iframe src="https://veloxchem.org" height="400" width="500"> </iframe>
