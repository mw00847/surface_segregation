# UNDERSTANDING SURFACE SEGREGATION OF POLYMER BLENDS WITH MARTINI 3.

## Abstract
Predicting the surface composition of a polymer blend is often determined by the component with the lowest surface energy (1,2). However the structure (entropic) and functional groups (enthalpic) contributions have shown to contradict the surface energy belief. This works varies blends of differing surface energy, chain length and concentration helping to provide an understanding of the driving force in different types of polymer blends.

## Introduction

Surface energy is the free energy per unit area of creating an interface with air or vacuum. Surface tension is the force per unit length along the surface opposing the interface of air or vacuum (1). The surface of polymer blends is often different from the bulk material (1). This in itself provides the opportunity to add functionality at the surface interface for coatings to have properties such as corrosion inhibition, hydrophobicity, conductance and anti cratering. Flow aids are commonly used in small quantities in coatings to improve the wetting and work of adhesion and also enhance the visual effect of the coating (4). These flow aids are often a low weight polyacrylic in comparison to the blend of polymers that make up the binder and topocoat. A prerequisite of segregation is imiscibility in the polymer blend (2), using an extended Flory Huggins parameter it is suggested that ester monomers with a similar backbone structure increase miscibility. 

This work uses molecular dynamics with GROMACS with the MARTINI 3 forcefield to vary surface tension, molecular weight and concentration of useful polymer blends to further understand the key reasons for surface segregation. Blends of linear homopolymers that vary only with molecular weight produce melts where the lower weight chains are found at the surface (9).

The polyply suite (7), provides a simple process to produce a wide range of polymer systems built using the MARTINI forcefield. As with the tutorial on their github page of dextran and PEO (6), simulation of 500ns of the blend produces a phase seperated system as below. This work has been repeated from their paper (3).

![alt text](https://github.com/mw00847/surface_segregation/blob/main/dextran_PEO.png?raw=True)

A surface-vacuum interface can be created by extending the Z axis of an equilibriated polymer blend box (8). Due to periodic boundary constraints, two interfaces are created.

## Method 

This work is split into several different polymer blend systems, with the experimental variable changed in each case. Initial polymer chain coordinates were generated using polyply, producing starting configurations at a density of 1000 kg/m³ in a cubic box with dimensions determined by blend concentration and solvent content. The Martini 3 coarse-grained force field was used throughout, with solvent (chlorobenzene) parameters taken from the Martini 3 small molecule library. Polyply assigns identical residue names to polymer chains regardless of chain length, which prevents GROMACS from distinguishing between species of different length within `make_ndx`. This was resolved with a targeted `sed` edit to column 4 of the atoms section in the generated topology, giving each chain length a distinct residue name. Note that GROMACS truncates long residue names in GRO format (e.g. PEO477 is truncated to PEO47); index group names were chosen to match this truncated output rather than the original polyply naming.

The simulation box was extended to 50 nm in the z-direction using gmx editconf, creating a liquid-vapour slab geometry with vacuum regions at both interfaces. This geometry allows direct measurement of surface segregation through density profiles along the z-axis. No pressure coupling was applied at any stage after box extension; all subsequent runs used the NVT ensemble.

Energy minimisation was performed using the steepest descent algorithm, followed by a short NVT stabilisation run prior to production. NVT production runs of 1 microsecond (10⁸ steps at dt = 0.01 ps) were carried out using GROMACS 2024.3, with GPU acceleration on an NVIDIA RTX 4060. Temperature was maintained at 298 K using a velocity-rescale thermostat with a coupling time constant of τ = 1.0 ps. Electrostatics were treated using a reaction-field scheme with a cutoff of 1.1 nm, relative permittivity εᵣ = 15 and εᵣf = 0, consistent with Martini 3 recommendations. Van der Waals interactions used a cutoff scheme with potential shift and a cutoff of 1.1 nm. A Verlet neighbour list was used with a buffer tolerance of 0.005 and update frequency of every 10 steps. No constraints were applied to bonds.

## Results

The systems studied in this work include blends of 

* polyethene oxide, PEO 477 with PEO44, here where only the chain length and concentration is varied.
* (poly(3-hexylthiophene)) and polystyrene, P3HT126 with PS20, the surface tension, chain length and concentration is varied.
* (poly(3-hexylthiophene)) and polystyrene, P3HT20 with PS 20, varying surface tension. 

Polyethylene Oxide

Here in the blend of polyethylene oxide with longer chains of 477 monomers with 44 the experiment focuses on the chain length and entropy to understand the segregation of shorter and longer chains with little or no surface energy difference. The longer chains sit in the bulk of the slab where entropy is higher and there is more room for the chains to move. The smaller PEO44 chains have more freedom to move and are found at the interfaces where entropy is lower but they can fit in more efficently than the longer chains.

![P44 density profile](https://github.com/mw00847/surface_segregation/blob/main/P44.png?raw=true)
![P477 density profile](https://github.com/mw00847/surface_segregation/blob/main/P477.png?raw=true)

(1) Tailoring the Attraction of Polymers toward Surfaces
Gila E. Stein, Travis S. Laws, and Rafael Verduzco
Macromolecules 2019 52 (13), 4787-4802
DOI: 10.1021/acs.macromol.9b00492

(2) Self-stratified bio-based coatings: Formulation and elucidation of critical parameters governing stratification
Charlotte Lemesle a b, Séverine Bellayer a, Sophie Duquesne a, Anne-Sophie Schuller c, Laurent Thomas b, Mathilde Casetta a, Maude Jimenez a
https://doi.org/10.1016/j.apsusc.2020.147687

(3) Martini 3 Coarse-Grained Force Field for Carbohydrates
Fabian Grünewald, Mats H. Punt, Elizabeth E. Jefferys, Petteri A. Vainikka, Melanie König, Valtteri Virtanen, Travis A. Meyer, Weria Pezeshkian, Adam J. Gormley, Maarit Karonen, Mark S. P. Sansom, Paulo C. T. Souza, and Siewert J. Marrink
Journal of Chemical Theory and Computation 2022 18 (12), 7555-7569
DOI: 10.1021/acs.jctc.2c00757
https://pubs.acs.org/doi/epdf/10.1021/acs.jctc.2c00757

(4) Migration and segregation phenomena of a silicone additive in a multilayer organic coating
Steven J.Hinder, Chris Lowe, James T.Maxted, John F.Watts 
Progress in Organic Coatings 
doi:10.1016/j.porgcoat.2005.04.007

(5) A Molecular Dynamics Study on the Miscibility and Morphology of Polyester Blends used in Coil Coatings
Matthew Wearon, Brendan J. Howlin, Chris Lowe, Marie-Laure Abel, John F. Watts 
Progress in Organic Coatings 
https://doi.org/10.1016/j.porgcoat.2022.107065

(6)Tutorial: Martini Polymers 
https://github.com/marrink-lab/polyply_1.0/wiki/Tutorial:-Martini-Polymers

(7) Polyply suite 
https://github.com/marrink-lab/polyply_1.0

(8) Liquid-vapor tension
https://sites.psu.edu/simtech/liquid-vapor-tension/

(9) Entropic segregation of short polymers to the surface of
a polydisperse melt
P. Mahmoudi1 and M.W. Matsen
DOI 10.1140/epje/i2017-11575-7
