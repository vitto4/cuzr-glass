# cuzr-glass

<p align="center">

<picture>
<img style="width: 60%;" alt="banner" src="https://github.com/user-attachments/assets/7cbb4a55-8d8d-4ac8-8180-8afa14679a2a" />
</picture>

</p>

Molecular dynamics simulation of $\text{Cu} _{64} \text{Zr} _{36}$ and $\text{Cu} _{50} \text{Zr} _{50}$ metallic glass formation by quenching from the molten state using _LAMMPS_[^lammps] (students' project)

[^lammps]: S. J. Plimpton, A. Kohlmeyer, A. P. Thompson, S. G. Moore, and R. Berger, LAMMPS: Large-scale Atomic/Molecular Massively Parallel Simulator. (Aug. 02, 2023). Zenodo. doi: 10.5281/ZENODO.3726416.

<p align="center">
  <a href="#foreword">
    Foreword
  </a>
  <!-- -->
  <picture><img width="0.2%" height="0px"></picture>
  <span>•</span>
  <picture><img width="0.2%" height="0px"></picture>
  <!-- -->
  <a href="#overview">
    Overview
  </a>
  <!-- -->
  <picture><img width="0.2%" height="0px"></picture>
  <span>•</span>
  <picture><img width="0.2%" height="0px"></picture>
  <!-- -->
  <a href="#simulated-system">
    Simulated system
  </a>
  <!-- -->
  <picture><img width="0.2%" height="0px"></picture>
  <span>•</span>
  <picture><img width="0.2%" height="0px"></picture>
  <!-- -->
  <a href="#thermodynamic-stability">
    Thermodynamic stability
  </a>
  <!-- -->
  <picture><img width="0.2%" height="0px"></picture>
  <span>•</span>
  <picture><img width="0.2%" height="0px"></picture>
  <!-- -->
  <a href="#quench">
    Quench
  </a>
  <!-- -->
  <picture><img width="0.2%" height="0px"></picture>
  <span>•</span>
  <picture><img width="0.2%" height="0px"></picture>
  <!-- -->
  <a href="#radial-distribution-function-rdf">
    RDF
  </a>
  <!-- -->
  <picture><img width="0.2%" height="0px"></picture>
  <span>•</span>
  <picture><img width="0.2%" height="0px"></picture>
  <!-- -->
  <a href="#validating-the-results">
    Validating the results
  </a>
  <!-- -->
  <picture><img width="0.2%" height="0px"></picture>
  <span>•</span>
  <picture><img width="0.2%" height="0px"></picture>
  <!-- -->
  <a href="#bonus">
    Bonus
  </a>
</p>

## Foreword

I was in charge of the code for this project, and as we couldn't find many complete _LAMMPS_ examples online, I asked for permission to make it public.
If you're just getting started with MD and / or $\text{CuZr}$ metallic glasses, the code is commented extensively, so it might be a worthy read.

## Overview

Simply put, the goal was to :
1. Place atoms of $\text{Cu}$ and $\text{Zr}$ at random positions in a small 3D volume and at a high temperature (over the $\text{CuZr}$ melting point).
2. Let it sit there for a while to reach thermodynamic stability ; and quench.
3. Then try and find out if we produced a metallic glass, by computing the resulting radial distribution function $g(r)$.

## Simulated system

- The simulation is carried out in 3D, with periodic boundary conditions. $\hspace{3pt} \text{⬦} \hspace{3pt}$ <a href="https://github.com/vitto4/cuzr-glass/blob/59611e3f48e352903b66cb583bd0edbd64eac768/cuzr-glass.lmp#L24-L30"><kbd>go to code</kbd></a>
- A mathematical model of the interatomic potential has to be selected through `pair_style`, and its parameters through `pair_coeff`. We ended up going with _Mendelev, M. I., et al_[^mendelev_development_2009], after first mistakenly using a potential for crystalline CuZr ! $\hspace{3pt} \text{⬦} \hspace{3pt}$ <a href="https://github.com/vitto4/cuzr-glass/blob/59611e3f48e352903b66cb583bd0edbd64eac768/cuzr-glass.lmp#L86-L92"><kbd>go to code</kbd></a>
- _LAMMPS_' `metal` unit style is used, as presented in <ins>Table 1</ins>.

<div align="center">

| Mass | Time | Temperature | Pressure | Density | Distance |
|:-:|:-:|:-:|:-:|:-:|:-:|
| $\text{g} \cdot \text{mol}^{-1}$ | $\text{ps}$ | $\text{K}$ | $\text{bar}$ | $\text{g} \cdot \text{cm}^{-\text{dim}}$ | $Å$ |
<p align="center"><sup><ins>Table 1</ins> -- Quantities and associated units in <i>LAMMPS</i>' <code>metal</code> unit style.</sup></p>
</div>

- For the temperature, $1 \thinspace 500 \text{ K}$ should be sufficient to exceed the melting point of our alloy[^sato_predictive_2017] [^bendert_containerless_2014]. $\hspace{3pt} \text{⬦} \hspace{3pt}$ <a href="https://github.com/vitto4/cuzr-glass/blob/59611e3f48e352903b66cb583bd0edbd64eac768/cuzr-glass.lmp#L49"><kbd>go to code</kbd></a>
- A density of $7$ seems fitting for the molten alloy ($\approx 7,02$ at the liquidus temperature for $\text{Cu}_ {48} \text{Zr}_ {52}$[^bendert_containerless_2014]), while staying close to the experimental density of amorphous solid $\text{Cu}_ {50} \text{Zr}_ {50}$ : $\rho = 7,3$[^kim_structural_2018] and $\text{Cu}_ {64} \text{Zr}_ {36}$ : $8,07$[^johnson_universal_2005] (densities given in $\text{g} \cdot \text{cm}^{-3}$). $\hspace{3pt} \text{⬦} \hspace{3pt}$ <a href="https://github.com/vitto4/cuzr-glass/blob/59611e3f48e352903b66cb583bd0edbd64eac768/cuzr-glass.lmp#L39"><kbd>go to code</kbd></a>
- As for the timestep, we were told $10^{-3} \text{ ps}$ is a standard value, and it is also the default for the `metal` units style. $\hspace{3pt} \text{⬦} \hspace{3pt}$ <a href="https://github.com/vitto4/cuzr-glass/blob/59611e3f48e352903b66cb583bd0edbd64eac768/cuzr-glass.lmp#L43"><kbd>go to code</kbd></a>

[^mendelev_development_2009]: M. I. Mendelev, M. J. Kramer, R. T. Ott, D. J. Sordelet, D. Yagodin, and P. Popel, ‘Development of suitable interatomic potentials for simulation of liquid and amorphous Cu–Zr alloys’, Philosophical Magazine, vol. 89, no. 11, pp. 967–987, Apr. 2009, doi: 10.1080/14786430902832773.

[^sato_predictive_2017]: Y. Sato, C. Nakai, M. Wakeda, and S. Ogata, ‘Predictive modeling of Time-Temperature-Transformation diagram of metallic glasses based on atomistically-informed classical nucleation theory’, Sci Rep, vol. 7, no. 1, p. 7194, Aug. 2017, doi: 10.1038/s41598-017-06482-8.

[^bendert_containerless_2014]: J. C. Bendert and K. F. Kelton, ‘Containerless Measurements of Density and Viscosity for a Cu $$_{48}$$ 48 Zr $$_{52}$$ 52 Liquid’, Int J Thermophys, vol. 35, no. 9–10, pp. 1677–1686, Oct. 2014, doi: 10.1007/s10765-014-1664-7.

[^kim_structural_2018]: M.-H. Kim, J.-Y. Suh, E. Fleury, S. G. Han, and K. T. Hong, ‘Structural homogeneity and mass density of bulk metallic glasses revealed by their rough surfaces and ultra-small angle neutron scattering (USANS)’, Sci Rep, vol. 8, no. 1, p. 12986, Aug. 2018, doi: 10.1038/s41598-018-30333-9.

[^johnson_universal_2005]: W. L. Johnson and K. Samwer, ‘A Universal Criterion for Plastic Yielding of Metallic Glasses with a ( T / T g ) 2 / 3 Temperature Dependence’, Phys. Rev. Lett., vol. 95, no. 19, p. 195501, Nov. 2005, doi: 10.1103/PhysRevLett.95.195501.

- Once the proportions in copper $p_\text{Cu}$, in zirconium $p_\text{Zr}$ and the density $\rho_\text{CuZr}$ have been set, the atomic density of our material $d_\text{at}$ and then the number of atoms to spawn in the simulation box can be established. $\hspace{3pt} \text{⬦} \hspace{3pt}$ <a href="https://github.com/vitto4/cuzr-glass/blob/59611e3f48e352903b66cb583bd0edbd64eac768/cuzr-glass.lmp#L61-L64"><kbd>go to code</kbd></a>

$$
\mathcal{M}_ \text{avg} = p_\text{Cu} \cdot \mathcal{M}_ \text{Cu} + p_\text{Zr} \cdot \mathcal{M}_ \text{Zr} \hspace{16mm} d_\text{at} = \frac{\rho_\text{CuZr}}{\mathcal{M}_ \text{avg}} \cdot \mathcal{N}_\text{A}
$$

$$
\begin{aligned}
\implies \mathcal{N}_ \text{Cu} &= \left\lfloor \thinspace p_ \text{Cu} \cdot d_ \text{at} \cdot L^3 \thinspace \right\rfloor \newline
\implies \mathcal{N}_ \text{Zr} &= \left\lfloor \thinspace p_ \text{Zr} \cdot d_ \text{at} \cdot L^3 \thinspace \right\rfloor
\end{aligned}
$$

> [!IMPORTANT]
> As you'll see in the code, our volume is $20^3 \space \small Å^3$, which is quite small. Using the formulae above it only gives us $\approx 450$ atoms to work with. If you run this simulation yourself, you might want to set `L equal 5e1` or above.

- The simulation box is created with a random distribution for the atomic positions. Something to keep in mind is that with this random positioning, two atoms can end up really close to one another. To avoid any instability problem that this could induce, we will also relax the material through energy minimisation before performing any simulation. $\hspace{3pt} \text{⬦} \hspace{3pt}$ <a href="https://github.com/vitto4/cuzr-glass/blob/59611e3f48e352903b66cb583bd0edbd64eac768/cuzr-glass.lmp#L103-L108"><kbd>go to code</kbd></a>

- One last thing to set is the random velocity assigned to each atom. These are drawn following a Gaussian distribution for a given target temperature. We were told a target temperature of two times what we want to end up with is a good starting point. $\hspace{3pt} \text{⬦} \hspace{3pt}$ <a href="https://github.com/vitto4/cuzr-glass/blob/59611e3f48e352903b66cb583bd0edbd64eac768/cuzr-glass.lmp#L52-L54"><kbd>go to code</kbd></a>

Here are the resulting simulation boxes, viewed via OVITO[^OVITO]. The copper is displayed in red, and the zirconium in blue.

<div align="center">

| <picture><img style="width: 50%;" alt="cuzr_50_50_box" src="https://github.com/user-attachments/assets/aff1cd1d-5a45-4c9c-ae27-097f59b81487" /></picture> | <picture><img style="width: 50%;" alt="cuzr_64_36_box" src="https://github.com/user-attachments/assets/70bcdce2-b6cf-480f-a15f-c234f977ef57" /></picture> |
|:-:|:-:|
| <sup><ins>Figure 1</ins> -- Simulation box generated for $\text{Cu} _{50} \text{Zr} _{50}$.</sup> | <sup><ins>Figure 2</ins> -- Simulation box generated for $\text{Cu} _{64} \text{Zr} _{36}$.</sup> |

</div>

[^OVITO]: A. Stukowski, ‘Visualization and analysis of atomistic simulation data with OVITO–the Open Visualization Tool’, Modelling Simul. Mater. Sci. Eng., vol. 18, no. 1, p. 015012, Jan. 2010, doi: 10.1088/0965-0393/18/1/015012.

## Thermodynamic stability

To verify that the system is in a stable state, we will run the simulation at constant temperature and pressure for $10 \thinspace 000 \space \Delta t$. We can then extract the variation of the temperature, pressure, volume and density with respect to the timestep.

> [!IMPORTANT]
> Since one timestep is $1 \space \text{fs}$, $10 \thinspace 000$ is a very small amount. The same can be said for other parts of the simulation.
> While we found the results didn't vary significantly, doing a 10x or even 25x on the number of timesteps throughout the code wouldn't be a bad idea.

You should find that the system remains around the values you had set for $T$, $V$ and $\rho$. However, $P$ is a different story, see below.


<div align="center">

<img style="width: 70%;" alt="cuzr_64_36_thermodynamic_stabilisation_graphs" src="https://github.com/user-attachments/assets/594b80ed-363c-4b4a-a412-9d3b40da031d" />

<p align="center"><sup><ins>Figure 3</ins> -- Changes in the monitored quantities as a function of timestep for the $\text{Cu} _{64} \text{Zr} _{36}$.</sup></p>
</div>

> [!NOTE]
> Yeah, the pressure variation is huge : $\overline{P} = 77 \approx 0 \space \text{bar}$ with a standard deviation of $\sigma = 6104 \space \text{bar}$ !
> But there's a reason for this : our system has very small dimensions ($20^3 \space \small Å^3$) and number of atoms ($\approx 450$), so it is probable that _LAMMPS_' _Nosé-Hoover barostat_ is not exactly up to the task.

As a matter of fact, doubling `L` and simulating over $75\thinspace 000$ timesteps brings that standard deviation down to $\sigma \approx 1300 \space \text{bar}$. Since the pressure oscillates around a constant value of $\approx 0 \space \text{bar}$ and the volume is stable, it is unlikely to be a sign of thermodynamic instability, so let's just brush it off.


## Quench

- The $\text{CuZr}$ is currently sitting at $T = 1 \thinspace 500 \space \text{K}$ in liquid state. It will now be quenched to $T = 300 \space \text{K}$ in order to form a solid phase. The same variables as before will be logged over the timestep. The quench will last for a total of $20 \thinspace 000 + 5 \thinspace 000 = 25 \thinspace 000 \space \Delta t$. $\hspace{3pt} \text{⬦} \hspace{3pt}$ <a href="https://github.com/vitto4/cuzr-glass/blob/59611e3f48e352903b66cb583bd0edbd64eac768/cuzr-glass.lmp#L199"><kbd>go to code</kbd></a>
- The second fix is a hack to get the system to actually reach the target $300 \space \text{K}$, because it would otherwise not cool all the way down, even when running for up to $15 \thinspace 000$ additional timesteps. $\hspace{3pt} \text{⬦} \hspace{3pt}$ <a href="https://github.com/vitto4/cuzr-glass/blob/59611e3f48e352903b66cb583bd0edbd64eac768/cuzr-glass.lmp#L206"><kbd>go to code</kbd></a>

<div align="center">

<img style="width: 70%;" alt="cuzr_64_36_quench_graphs" src="https://github.com/user-attachments/assets/e2a6c0b1-f17c-4d6d-a3b2-32b05131c300" />

<p align="center"><sup><ins>Figure 4</ins> -- Changes in the monitored quantities as a function of timestep during quenching for the $\text{Cu} _{64} \text{Zr} _{36}$.</sup></p>
</div>

## Radial distribution function (RDF)

- To confirm the proper formation of a glass, one can use the RDF $g(r)$, which qualifies the atomic density perceived by an atom (the reference, $r = 0$), as a function of $r$ (the distance from said reference).
- With our binary alloy, the RDF can be computed for :
  - $\text{Cu}$ and $\text{Zr}$ atoms indistinctly, denoted « $\text{All}$ »
  - $\text{Zr}$ atoms, with $\text{Cu}$ atoms as reference, denoted « $\text{CuZr}$ »
  - $\text{Cu}$ atoms only, denoted « $\text{CuCu}$ »
  - $\text{Zr}$ atoms only, denoted « $\text{ZrZr}$ »
  
  It should be noted that the RDF « $\text{CuZr}$ » and « $\text{ZrCu}$ » are conceptually identical. $\hspace{3pt} \text{⬦} \hspace{3pt}$ <a href="https://github.com/vitto4/cuzr-glass/blob/59611e3f48e352903b66cb583bd0edbd64eac768/cuzr-glass.lmp#L231-L241"><kbd>go to code</kbd></a>

> [!IMPORTANT]
> With `1 1 2` for `Nevery`, `Nrepeat` and `Nfreq`, it computes the _instantaneous_ RDF, as in without any averaging over the timestep (also, I can't exactly remember why I set `2` for `Nfreq` instead of 1).
> You can try with some amount of averaging, like `10 100 1000` followed by a `run 1000`, the curve will look better.


- The results are presented heareafter in <ins>Figure 5</ins>. But look at <ins>Figure 6</ins> instead, I have something better to show.

<div align="center">

<details><summary><ins>Figure 5</ins></summary>


<img style="width: 70%;" alt="cuzr_RDF_graphs" src="https://github.com/user-attachments/assets/69b4492a-fc40-4895-b4d0-b0752b9a79f4" />
<p align="center"><sup><ins>Figure 5</ins> -- Radial distribution functions for our simulated $\text{Cu} _{64} \text{Zr} _{36}$ and $\text{Cu} _{50} \text{Zr} _{50}$. These curves aren't the best (visually) due to a lack of averaging over the timestep, a very small system and a short simulation.</sup></p>


</details>

</div>


- To have better RDF curves to show you, I ran the simulation again but over $475 \thinspace 000 \space \Delta t$, with $L = 4,3 \mathring{\text{A}}$, and averaged the RDF over $1 \thinspace 000 \space \Delta t$. On <ins>Figure 6</ins>, the typical « splitting of the second peak » that is said to be characteristic of $\text{CuZr}$ metallic glasses is clearly visible.

<div align="center">

<img style="width: 70%;" alt="cuzr_64_36_individual_RDF_graphs" src="https://github.com/user-attachments/assets/474c75b8-2bfb-4d40-8310-be35ac589032" />


<p align="center"><sup><ins>Figure 6</ins> -- Average (over $1 \thinspace 000 \space \Delta t$) distribution functions for our simulated $\text{Cu} _{64} \text{Zr} _{36}$. Different simulation run than in the rest of this document, with $475 \thinspace 000$ timesteps and $L = 4,3 \mathring{\text{A}}$.</sup></p>
</div>

## Validating the results

Let's extract the nearest-neighbor values (x-coordinate of the first peak) from the computed RDF. These are then compared with results from relevant publications in <ins>Table 3</ins>.

<div align="center">

| Composition | Source | $\text{CuCu}$ | $\text{CuZr}$ | $\text{ZrZr}$ | Total (All) |
|:-:|:-:|:-:|:-:|:-:|:-:|
| $\text{Cu} _{64} \text{Zr} _{36}$ | This document (Fig. 5) | $2,74$ | $2,77$ | $3,23$ | $2,70$ |
| $\text{Cu} _{64} \text{Zr} _{36}$ | This document (Fig. 6) | $2,62$ | $2,82$ | $3,26$ | $2,69$ |
| $\text{Cu} _{64} \text{Zr} _{36}$ | W. B. Zhang et al.[^zhang_size_2017] (Fig. 2, bulk) | $\approx 2,5$ | $\approx 2,80$ | $\approx 3,05$ | $2,78$ |
| | | | | | |
| $\text{Cu} _{50} \text{Zr} _{50}$ | This document (Fig. 5) | $2,61$ | $2,77$ | $3,15$ | $2,77$ |
| $\text{Cu} _{50} \text{Zr} _{50}$ | A. Paduraru [^panduraru_simulations_2008] | $2,56$ | $2,75$ | $3,13$ | $\text{N.A.}$ |

<p align="center"><sup><ins>Table 3</ins> -- Distances to the nearest neighbor (in $\mathring{\text{A}}$) from relevant publications, compared with this document's results.</sup></p>
</div>

[^zhang_size_2017]: W. B. Zhang et al., ‘Size effect on atomic structure in low-dimensional Cu-Zr amorphous systems’, Sci Rep, vol. 7, no. 1, p. 7291, Aug. 2017, doi: 10.1038/s41598-017-07708-5.
[^panduraru_simulations_2008]: A. Paduraru, Simulations of Mechanical Properties of CuZr and CuMg Metallic Glasses. 2008. [Online]. Available: https://orbit.dtu.dk/en/publications/simulations-of-mechanical-properties-of-cuzr-and-cumg-metallic-gl

## Bonus

If you're a professor reading this and looking for exercise ideas, here are a couple things I thought of :
- Try and detect the glass transition temperature $T_g$ while heating up the material, and compare the results with S. Liu, C. Yang, R. Qi, and M. Sun[^liu_glass_2024].
- Try and detect the crystallisation temperature $T_x$ described in Y. Champion and M. Blétry[^champion_proprietes_2009] while heating up. Results to be compared with Y. Sato, C. Nakai, M. Wakeda, and S. Ogata[^sato_predictive_2017] for example. It could also be nice to visualise it using _OVITO_.
- More difficult, likely needs a simulation on a larger scale : try and validate the results of S. Sohn _et al._[^sohn_framework_2023] on plasticity in metallic glasses.
- Try and observe the forming of an amorphous phase from a $\text{CuZr}$ crystal first melted and then quenched. I.e. the full process, whereas in the present simulation we only quenched $\text{CuZr}$ from the liquid state.

[^liu_glass_2024]: S. Liu, C. Yang, R. Qi, and M. Sun, ‘The glass transition of CuZr metallic glasses in the perspective of machine learning’, Computational Materials Science, vol. 244, p. 113205, Sep. 2024, doi: 10.1016/j.commatsci.2024.113205.

[^champion_proprietes_2009]: (sorry this one is in french :/)\
  Y. Champion and M. Blétry, ‘Propriétés mécaniques des verres métalliques’, Matériaux fonctionnels - Matériaux biosourcés, Oct. 2009, doi: 10.51257/a-v1-n2720.

[^sohn_framework_2023]: S. Sohn et al., ‘A framework for plasticity in metallic glasses’, Materialia, vol. 31, p. 101876, Sep. 2023, doi: 10.1016/j.mtla.2023.101876.

<h3>Bibliography</h3>