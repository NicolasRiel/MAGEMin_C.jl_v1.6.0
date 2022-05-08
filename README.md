# MAGEMin_C.jl

[![Build Status](https://github.com/JuliaGeodynamics/GeoParams.jl/workflows/CI/badge.svg)](https://github.com/ComputationalThermodynamics/MAGEMin_C.jl/actions)


Julia interface to the MAGEMin C package, which performs thermodynamic equilibrium calculations.
See the [MAGEMin](https://github.com/ComputationalThermodynamics/MAGEMin) page for more details and documentation on how to use it.


## Quick guide
First install julia. We recommend downloading the official binary from the [julia](julialang.org) webpage. 

Next, install the `MAGEMin_C` package with: 
```julia
julia> ]
pkg> add MAGEMin_C
```  
By pushing `backspace` you come back to the main julia terminal. This will download a compiled version of the library as well as some wrapper functions to your system.

Next, you can do calculations with
```julia
julia> using MAGEMin_C
Using libMAGEMin.dylib from MAGEMin_jll
julia> gv, DB = init_MAGEMin();	# initialize database
julia> test = 0;
julia> bulk_rock   = get_bulk_rock(gv, test);	 
julia> gv.verbose  = -1; 							
julia> P_kbar, T_C = 8.0, 1300.0;		
julia> out         = point_wise_minimization(P_kbar,T_C, bulk_rock, gv, DB)  
Pressure          : 8.0      [kbar]
Temperature       : 1300.0    [Celcius]
     Stable phase | Fraction 
              opx   0.22201 
              cpx   0.01321 
              liq   0.14214 
               ol   0.62263 
Gibbs free energy : -856.885052  (71 iterations; 116.82 ms)
```  
After the calculation is finished, the structure `out` holds all the information about the stable assemblage, including seismic velocities, melt content, melt chemistry, densities etc.
You can show a full overview of that with
```julia
julia> print_info(out)
```
If you are interested in the density or seismic velocity at the point,  access it with
```julia
julia> out.rho
3144.282577840362
julia> out.Vp
5.919986959559542
```
Once you are done with all calculations, release the memory with
```julia
julia> finalize_MAGEMin(gv,DB)
```