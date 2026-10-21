<div align="center">

<img src="images/header02.jpg" width="100%">

# Aligning Protein Structures with GR-Align (GRaphlet-based Aligner)

**Noël Malod-Dognin and Nataša Pržulj**

Corresponding author: Dr. N. Malod-Dognin

<img src="images/optimal_matching.png" width="400">

*Optimal CMO alignment*

</div>

GR-Align is a fast heuristic for aligning and comparing protein structures under the Contact Map Overlap maximization (CMO) paradigm. It uses a Needleman-Wunsch algorithm in which the cost of matching two residues is their graphlet degree similarity.

> [!NOTE]
> **Version 1.5:** outputs of the original software were hard to parse. Aligned residues are now labelled with their PDB IDs.

## Downloads

- [GR-Align 1.5 for Linux x64](https://github.com/przulj-lab/GR-Align/releases/download/v1.5/GR-Align_1.5.zip)
- [Supplementary material (PDF)](https://github.com/przulj-lab/GR-Align/releases/download/v1.5/GR-Align_Supplemental.pdf)

## How to run GR-Align

The package contains three programs:

- **CMap** converts protein structures (`.pdb`) into contact map graphs (LEDA graphs, `.gw`)
- **DCount** computes graphlet degree signatures (`.ndump`) from contact maps
- **GR-Align** aligns protein structures (needs both contact maps and signatures)

Run any of them without parameters (or with `-h`) to see usage directions.

## Example

Create a contact map from chain A of structure 1amk:
```bash
./CMap -i 1amk.pdb -c A -o 1amkA.gw -d 7.5
```

Compute its graphlet degree signature:
```bash
./DCount -i 1amkA.gw -o 1amkA.ndump
```

Align all structures of the Skolnick set, listed in `skolnick.lst`, with contact maps and signatures in `./skolnick`:
```bash
./GR-Align -q skolnick.lst -r ./skolnick -o results.txt
```

## Citation

N. Malod-Dognin and N. Pržulj. GR-Align: fast and flexible alignment of protein 3D structures using graphlet degree similarity. *Bioinformatics* 30(9), 1259–1265 (2014). https://doi.org/10.1093/bioinformatics/btu020
