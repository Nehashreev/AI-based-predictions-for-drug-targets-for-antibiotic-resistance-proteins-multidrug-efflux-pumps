# AI-Based Prediction of Drug Candidates Against Antibiotic Resistance Proteins: MexB Multidrug Efflux Pump

## 1. Project Overview

Antibiotic resistance is a major global healthcare challenge. Multidrug efflux pumps contribute to bacterial resistance by transporting antibiotics and other compounds out of bacterial cells, reducing their intracellular accumulation.

This project uses a structure-based computational workflow to investigate potential inhibitors of **MexB**, a component of the MexAB-OprM multidrug efflux system in *Pseudomonas aeruginosa*.

The study focuses on the **Deep Binding Pocket (DBP)** of MexB and evaluates candidate compounds using molecular docking and residue-level geometric analysis.

The workflow combines protein structure preparation, ligand docking, candidate ranking, reference-compound comparison, and pocket-contact analysis.

> **Note:** This is an in-silico screening project. Predicted docking scores and structural contacts do not establish experimentally confirmed binding, efflux inhibition, or antibiotic-resistance reversal.

## 2. Objectives

- Investigate the MexB multidrug efflux transporter as a potential target for efflux-pump inhibition.
- Define a docking search region around the Deep Binding Pocket.
- Perform molecular docking using AutoDock Vina.
- Rank candidate ligands according to predicted docking scores.
- Compare predicted candidates with reference compounds.
- Evaluate proximity to six selected DBP residues.
- Generate reproducible tables and molecular visualizations for further investigation.

## 3. Workflow

The project follows these major steps:

1. **Protein structure selection:** Obtain the MexB structure and identify the binding-state protomer.
2. **Receptor preparation:** Prepare the receptor structure for docking and convert it to PDBQT format.
3. **Pocket definition:** Define the DBP search region using the recorded docking-box coordinates and dimensions.
4. **Ligand preparation:** Obtain candidate and reference compounds and prepare their docking-compatible structures.
5. **Molecular docking:** Run AutoDock Vina and collect predicted binding scores and poses.
6. **Candidate ranking:** Rank predicted compounds by docking score.
7. **Structural analysis:** Calculate minimum distances between ligand atoms and selected core residues.
8. **Reference comparison:** Compare predicted candidates with reference compounds and their recorded activity data.
9. **Visualization:** Inspect the receptor, docking box, ligand poses, and selected residues using PyMOL.
10. **Interpretation:** Summarize computational findings and identify candidates for future validation.

## 4. Software and Requirements

The workflow was developed in a Windows environment.

| Software or resource | Purpose |
|---|---|
| AutoDock Vina 1.2.7 | Molecular docking and pose scoring |
| PyMOL | Molecular visualization |
| Python 3 | Analysis scripts and result-table generation |
| Biopython or other structure-handling tools, if required by the scripts | Structure processing |
| MexB structural coordinates | Receptor model |
| Ligand structure files | Candidate and reference compounds |

Use the same software versions and input files where possible to improve reproducibility. Additional Python dependencies depend on the contents of the analysis scripts.

### Installation

Install Python 3 and PyMOL using their official distribution channels.

Download AutoDock Vina from its official repository:

- AutoDock Vina: https://github.com/ccsb-scripps/AutoDock-Vina
- PyMOL: https://pymol.org/
- Python: https://www.python.org/

Verify that Vina runs by opening Command Prompt and executing:

```bash
vina --version
```

If Vina is not added to the Windows PATH, execute it using its full executable path instead.

For the project environment described during development, the executable was located at:

```text
C:\Users\nehas\MexB_Ligand_Dataset\vina.exe
```

Replace local paths with paths appropriate for your computer.

## 5. Repository Structure

A suggested repository structure is:

```text
mexB-efflux-pump-screening/
│
├── README.md
├── data/
│   ├── receptor/
│   ├── candidate_ligands/
│   └── reference_ligands/
│
├── config/
│   └── MexB_DBP_vina.txt
│
├── scripts/
│   ├── analyze_core_DBP_geometry.py
│   ├── analyze_reference_core_DBP_geometry.py
│   └── create_MexB_validation_table.py
│
├── results/
│   ├── docking_scores/
│   ├── geometry/
│   └── figures/
│
└── docs/
    └── methodology.md
```

This is a recommended organization. Adapt it to the files actually committed to the repository. The analysis scripts listed above were used during project development; include them in the repository if you want others to reproduce those analyses.

## 6. Docking Configuration

The recorded AutoDock Vina search-box parameters are:

| Parameter | Value |
|---|---:|
| Center X | −11.731 Å |
| Center Y | −0.815 Å |
| Center Z | −28.409 Å |
| Size X | 32 Å |
| Size Y | 28 Å |
| Size Z | 28 Å |
| Exhaustiveness | 16 |
| Number of modes | 9 |
| Energy range | 3 kcal/mol |
| Random seed | 42 |

The box dimensions define the region searched by the docking algorithm. The center and dimensions must be used with receptor coordinates in the same coordinate system as the original docking run.

The configuration file, `MexB_DBP_vina.txt`, contains these parameters and the receptor path.

Example configuration:

```text
receptor = path/to/MexB_Binding_clean.pdbqt

center_x = -11.731
center_y = -0.815
center_z = -28.409

size_x = 32
size_y = 28
size_z = 28

exhaustiveness = 16
num_modes = 9
energy_range = 3
seed = 42
```

Update the receptor path before running Vina. The example above uses a placeholder path.

## 7. Reproducing the Docking Workflow

### Step 1: Prepare the receptor

Prepare the MexB binding-state structure for docking and save the resulting receptor in PDBQT format.

The receptor used during the recorded workflow was:

```text
MexB_Binding_clean.pdbqt
```

Verify that the prepared structure retains the intended DBP coordinates and that receptor preparation is consistent with the original workflow.

### Step 2: Prepare the ligands

Prepare the candidate ligands and reference compounds in PDBQT format using a consistent ligand-preparation procedure.

The four reference-compound identifiers used in the project were:

- CHEMBL191703
- CHEMBL297453
- CHEMBL4064721
- CHEMBL5569750

The five top-ranked predicted candidate identifiers were:

- CHEMBL254328
- CHEMBL1170
- CHEMBL1200374
- CHEMBL2107826
- CHEMBL1336

For exact reproduction, use the same ligand structures, protonation states, atom types, preparation settings, and input conformations as the original run. The identifiers alone are not sufficient to reconstruct every preparation choice.

### Step 3: Run AutoDock Vina

An example command is:

```bash
vina ^
  --receptor "path\to\MexB_Binding_clean.pdbqt" ^
  --ligand "path\to\candidate.pdbqt" ^
  --center_x -11.731 ^
  --center_y -0.815 ^
  --center_z -28.409 ^
  --size_x 32 ^
  --size_y 28 ^
  --size_z 28 ^
  --exhaustiveness 16 ^
  --num_modes 9 ^
  --energy_range 3 ^
  --seed 42 ^
  --out "path\to\candidate_out.pdbqt"
```

Run the command from Windows Command Prompt, replacing the placeholder paths with the actual receptor, ligand, and output paths. Ensure that the `vina` executable is available through PATH or invoke it using its full path.

Repeat this process for every candidate and reference ligand.

Save the docking log and output pose for each ligand. Record the Vina version, input files, configuration, and any warnings.

### Step 4: Rank the candidate ligands

Collect the best-mode docking scores from the Vina outputs and sort the predicted candidates by score.

More negative Vina scores indicate more favorable predictions under the scoring function. They should not be interpreted as measured binding free energies or direct evidence of inhibition.

### Step 5: Analyze DBP residue geometry

The analysis focuses on six core residues:

| Residue | Position |
|---|---:|
| VAL | 139 |
| PHE | 178 |
| TYR | 327 |
| PHE | 573 |
| PHE | 610 |
| PHE | 628 |

The geometry analysis scripts calculate minimum ligand-to-residue atom distances and generate CSV result tables.

The recorded analysis outputs include:

```text
MexB_core_DBP_geometry.csv
MexB_reference_core_DBP_geometry.csv
MexB_DBP_validation_summary.csv
```

If these scripts are included in `scripts/`, run them using the required Python environment and the corresponding input structures. Check their source code for expected filenames, paths, and dependencies before execution.

A distance threshold of less than 4 Å was used to count close contacts in the reference comparison. This is a geometric criterion, not proof of a favorable chemical interaction.

### Step 6: Create the validation summary

The summary table combines ligand identifiers, group labels, docking scores, available experimental activity records, core-residue contact counts, and individual residue distances.

Check that the ligand identifiers match across the docking outputs and geometry files. Preserve missing experimental values as unavailable rather than treating them as inactive compounds.

## 8. Recorded Results

### Top five predicted candidates

| Candidate | Vina score (kcal/mol) |
|---|---:|
| CHEMBL254328 | −12.90 |
| CHEMBL1170 | −11.72 |
| CHEMBL1200374 | −11.25 |
| CHEMBL2107826 | −10.98 |
| CHEMBL1336 | −10.44 |

CHEMBL254328 had the most favorable predicted docking score among the five listed candidates.

### Reference compounds

| Reference compound | Vina score (kcal/mol) | Recorded activity |
|---|---:|---|
| CHEMBL191703 | −10.030 | MIC 8.0 µg/mL |
| CHEMBL297453 | −10.720 | MIC 12.6992 µg/mL |
| CHEMBL4064721 | −9.355 | MEC >100 µM |
| CHEMBL5569750 | −9.448 | MEC 18.9404 µM |

These activity values are records from the project dataset and should be interpreted in the context of their original assays. MIC and MEC are different endpoints and should not be treated as directly interchangeable.

### Core-residue proximity

In the recorded geometry analysis, each of the five predicted candidates had a minimum distance below 4 Å to each of the six selected core residues.

This indicates that the analyzed poses were positioned close to the selected pocket residues under the chosen geometric criterion. It does not independently establish hydrogen bonding, binding affinity, inhibition, or biological efficacy.

## 9. Molecular Visualization Using PyMOL

PyMOL can be used to inspect the receptor, docked ligand, docking box, and selected DBP residues.

Load the receptor:

```python
load "path/to/MexB_Binding_clean.pdbqt", receptor
```

Load a docked ligand pose:

```python
load "path/to/CHEMBL254328_out.pdbqt", ligand
```

Display the structures:

```python
hide everything
show cartoon, receptor
color gray80, receptor
show sticks, ligand
color yellow, ligand
orient receptor
```

Replace the ligand filename with the actual output pose filename.

For the docking-box visualization, use the exact center and dimensions recorded in Section 6. Draw a wireframe box and inspect it from multiple orientations.

Highlight the six core residues if the receptor's chain and residue numbering match the analysis:

```python
select core_DBP, receptor and resi 139+178+327+573+610+628
show sticks, core_DBP
color cyan, core_DBP
```

Verify the selection before exporting the figure. Residue numbering and chain identifiers may differ between structures.

Save the PyMOL session and export a clear image showing the receptor, ligand, docking box, and labeled core residues.

## 10. Reproducibility and Limitations

For a reproducible run:

- Use the same receptor structure and coordinate system.
- Preserve the docking-box coordinates and dimensions.
- Record ligand sources and preparation settings.
- Use the same Vina version and search parameters.
- Retain docking logs, poses, and analysis outputs.
- Verify residue numbering and ligand-receptor alignment.
- Document any deviations from the original workflow.

### Limitations

1. Docking scores are approximate scoring-function outputs.
2. Close residue contacts do not prove favorable interactions.
3. Reference activity data may originate from different assays and conditions.
4. Docking alone does not establish efflux-pump inhibition or antibiotic sensitization.
5. Experimental testing is required to confirm biological activity.
6. Molecular dynamics, binding free-energy calculations, and experimental assays may be useful follow-up studies, but are not claimed as completed unless their results are separately documented.

## 11. Future Directions

Potential next steps include:

- Inspecting and validating the predicted ligand poses.
- Comparing ligand interactions across alternative MexB conformational states.
- Performing molecular dynamics simulations.
- Estimating binding free energies using an appropriate validated protocol.
- Testing candidate compounds experimentally for efflux inhibition and antibiotic-resistance modulation.

## 12. References

1. Trott, O., & Olson, A. J. (2010). AutoDock Vina: Improving the speed and accuracy of docking with a new scoring function, efficient optimization, and multithreading. *Journal of Computational Chemistry, 31*(2), 455–461. https://doi.org/10.1002/jcc.21334

2. Eberhardt, J., Santos-Martins, D., Tillack, A. F., & Forli, S. (2021). AutoDock Vina 1.2.0: New docking methods, expanded force field, and Python bindings. *Journal of Chemical Information and Modeling, 61*(8), 3891–3898. https://doi.org/10.1021/acs.jcim.1c00203

3. Murakami, S., Nakashima, R., Yamashita, E., & Yamaguchi, A. (2002). Crystal structure of bacterial multidrug efflux transporter AcrB. *Nature, 419*, 587–593. https://doi.org/10.1038/nature01050

Confirm the structure-specific references and any additional citations against the exact structural model and literature used in the final manuscript.

## 13. License and Citation

Choose a license appropriate for the repository and the permissions associated with included data and software. Cite the original software, structural data, and ligand sources used in the analysis.

If you use this workflow in academic work, describe the computational parameters, input structures, ligand preparation procedure, and limitations clearly so that the results can be independently assessed.
