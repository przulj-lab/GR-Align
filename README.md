# GR-Align: Graphlet-Based Protein Structure Alignment

A fast heuristic procedure for aligning and comparing protein structures based on Contact Map Overlap (CMO) maximization.

## Authors

- Noel Malod-Dognin
- Natasa Przulj

**Corresponding Author:** Dr. Noel Malod-Dognin  
📧 n.malod-dognin@cs.ucl.ac.uk

## Overview

GR-Align is a protein structure alignment tool that uses a novel approach based on graphlet degree similarity. The algorithm applies a modified Needleman-Wunsch procedure where the cost of matching two residues is determined by their graphlet degree similarity in contact map representations.

**Key Features:**
- Fast heuristic alignment based on Contact Map Overlap (CMO)
- Graphlet-based structural comparison
- Efficient processing of protein structure databases

## Version Information

**Current Version: 1.5**

**Update:** Outputs now include residues labeled with their PDB IDs for easier parsing and interpretation.

## Resources

- **[Supplementary Material](http://www0.cs.ucl.ac.uk/staff/natasa/GR-Align/GR-Align_Supplemental.pdf)** (PDF)

*Note: If download links don't work when clicked, copy and paste the URLs directly into your browser's address bar.*

## Software Components

GR-Align consists of three integrated tools:

1. **CMap** - Converts protein structures (`.pdb`) into contact map graphs (LEDA format `.gw`)
2. **DCount** - Computes graphlet degree signatures (`.ndump` files) from contact maps
3. **GR-Align** - Aligns protein structures using contact maps and signatures

## Requirements

- Protein structure files in PDB format
- Linux/Unix operating system

## Installation

```bash
git clone https://github.com/przulj-lab/GR-Align.git
cd GR-Align

# Make executables
chmod +x CMap DCount GR-Align
```

## Usage

### Display Help

Each tool displays usage directions when run without parameters:

```bash
./CMap -h
./DCount -h
./GR-Align -h
```

### Step 1: Create Contact Map

Convert a PDB structure file into a contact map graph:

```bash
./CMap -i input.pdb -c chain_id -o output.gw -d distance_threshold
```

**Parameters:**
- `-i` : Input PDB file
- `-c` : Chain identifier (e.g., A, B, C)
- `-o` : Output contact map file (`.gw` format)
- `-d` : Distance threshold in Ångströms (e.g., 7.5)

**Example:**
```bash
./CMap -i 1amk.pdb -c A -o 1amkA.gw -d 7.5
```

This creates a contact map for chain A of structure 1amk using a 7.5Å distance cutoff.

### Step 2: Compute Graphlet Degree Signatures

Calculate graphlet signatures from the contact map:

```bash
./DCount -i contact_map.gw -o signatures.ndump
```

**Parameters:**
- `-i` : Input contact map file (`.gw` format)
- `-o` : Output signature file (`.ndump` format)

**Example:**
```bash
./DCount -i 1amkA.gw -o 1amkA.ndump
```

### Step 3: Align Protein Structures

Perform structure alignment using contact maps and signatures:

```bash
./GR-Align -q structure_list.txt -r structures_directory -o results.txt
```

**Parameters:**
- `-q` : File listing structures to align (one per line)
- `-r` : Directory containing contact maps (`.gw`) and signatures (`.ndump`)
- `-o` : Output alignment results file

**Example:**
```bash
./GR-Align -q skolnick.lst -r ./skolnick -o results.txt
```

This aligns all structures listed in `skolnick.lst`, with their contact maps and signatures located in the `./skolnick` directory.

## Complete Workflow Example

### Single Structure Alignment

```bash
# 1. Create contact maps for two structures
./CMap -i protein1.pdb -c A -o protein1A.gw -d 7.5
./CMap -i protein2.pdb -c A -o protein2A.gw -d 7.5

# 2. Compute graphlet signatures
./DCount -i protein1A.gw -o protein1A.ndump
./DCount -i protein2A.gw -o protein2A.ndump

# 3. Create structure list file
echo "protein1A" > structures.lst
echo "protein2A" >> structures.lst

# 4. Run alignment
./GR-Align -q structures.lst -r ./ -o alignment_results.txt
```

### Batch Processing Example (Skolnick Set)

```bash
# Directory structure
skolnick/
├── 1amkA.gw
├── 1amkA.ndump
├── 1b0nA.gw
├── 1b0nA.ndump
└── ... (more structures)

# List file (skolnick.lst)
1amkA
1b0nA
...

# Run alignment
./GR-Align -q skolnick.lst -r ./skolnick -o skolnick_results.txt
```

## File Formats

### Input Files

**PDB Format (`.pdb`)**
- Standard Protein Data Bank format
- Contains 3D coordinates of atoms

**Structure List (`.lst`)**
- Plain text file
- One structure name per line (without file extensions)
- Example:
  ```
  1amkA
  1b0nA
  1c3wA
  ```

### Output Files

**Contact Map (`.gw`)**
- LEDA graph format
- Represents residue contacts as graph edges

**Graphlet Signatures (`.ndump`)**
- Binary format containing graphlet degree vectors for each residue

**Alignment Results (`.txt`)**
- Version 1.5: Includes PDB IDs for easier parsing
- Contains aligned residue pairs and alignment scores

## Algorithm Details

GR-Align uses a modified **Needleman-Wunsch dynamic programming algorithm** where:
- The similarity score between two residues is based on their **graphlet degree similarity**
- Contact maps capture the 3D structure as a graph
- Graphlet degrees provide local structural signatures
- The algorithm maximizes **Contact Map Overlap (CMO)**

## Performance Notes

- Fast heuristic suitable for large-scale structure comparisons
- Efficient graphlet-based scoring
- Suitable for protein structure database searches
- Scales well with database size

## Use Cases

- **Protein structure alignment** - Compare structural similarity
- **Fold recognition** - Identify proteins with similar folds
- **Structure classification** - Group proteins by structural features
- **Function prediction** - Infer function from structural similarity
- **Evolutionary analysis** - Study structural conservation

## Distance Threshold Guidelines

The `-d` parameter in CMap determines contact definition:
- **7.5 Å** - Standard for Cα contact maps (recommended)
- **8.0 Å** - More permissive contacts
- **6.0 Å** - Stricter contact definition

## Troubleshooting

### Missing Chain ID
If chain identifier is unknown:
```bash
grep "^ATOM" protein.pdb | awk '{print $5}' | sort -u
```

### File Name Conventions
Ensure consistency:
- If structure list contains `1amkA`
- Directory must contain both `1amkA.gw` and `1amkA.ndump`

### Permission Issues
Make executables runnable:
```bash
chmod +x CMap DCount GR-Align
```

## Contact

For questions or issues, please contact Dr. Noel Malod-Dognin at n.malod-dognin@cs.ucl.ac.uk
