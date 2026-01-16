# PCOS_Therapeutic_Peptide
# 🧬 Computational Design of Peptide Inhibitors for PCOS (Targeting DDIT3)

**Hypothesis:** By engineering a high-affinity "decoy" peptide based on the **C/EBP-beta** leucine zipper, we can sequester CHOP (DDIT3), preventing it from translocating to the nucleus and triggering cell death in ovarian granulosa cells.

### Abstract
A rational peptide design project targeting the DDIT3 signaling pathway to mitigate apoptosis in Polycystic Ovary Syndrome (PCOS). This repository contains structural models, molecular docking data (**-110.6 kcal/mol**), and 20ns Molecular Dynamics (MD) simulation data for a novel C/EBP-beta mutant peptide.

---

### 🧪 Methodology
This project utilized a structural bioinformatics pipeline to design and validate the therapeutic peptide:

#### 1. Target Identification
* Retrieved FASTA sequences for Human DDIT3 (**UniProt: P35638**) and Human C/EBP-beta (**UniProt: P17676**).
* Targeted the **bZIP domains** (DDIT3: residues 134–169; C/EBP-beta: residues 199–345).
* Modeled the interaction using **AlphaFold** (with a 14-residue flanking buffer) to resolve the native binding interface, followed by structural truncation of the core Leucine Zipper using **PyMOL**.

#### 2. Rational Peptide Design & Mutagenesis
* **Initial Scan:** Used **BioSig** to scan the mutational landscape.
    * *Observation:* Top automated candidates promised lower binding energy but were rejected due to significant steric clashes and internal strain observed during visual inspection.
* **Rational Selection:** Prioritized structural stability over raw score by manually selecting mutations to optimize packing and electrostatics (Residue numbering corresponds to UniProt P17676):
    * **T299M:** Threonine $\to$ Methionine mutation to enhance hydrophobic packing efficiency at the dimer interface.
    * **V303N:** Valine $\to$ Asparagine mutation to replace non-specific hydrophobic contacts with precise polar interactions (Hydrogen Bonding).

#### 3. Molecular Docking
* Performed protein-peptide docking and MM/GBSA binding free energy calculation using **HawkDock**.
* Benchmarked the **Mutant Complex** against the **Wild Type** interaction.

#### 4. Dynamic Simulation (Validation)
Verified complex stability using **OpenMM** (via the Making-it-Rain pipeline) accelerated by NVIDIA Tesla T4 GPUs.

| Phase | Parameters |
| :--- | :--- |
| **System Setup** | Forcefield: AMBER ff19SB, Water: TIP3P, Box: 10.0Å, Neutralized (NaCl 0.15M) |
| **Minimization** | Steps: 1000, Force Constant: 500 kJ/mol/nm |
| **Equilibration** | Temp: 300K, Pressure: 1 bar, Time: 0.2ns |
| **Production** | Duration: **20ns**, Step size: 2fs, Total Steps: 10,000,000 |

#### 5. Physicochemical Profiling
* Assessed solubility (GRAVY) and evolutionary conservation (BLAST).

---

### 📊 Key Results

#### 1. Thermodynamic Stability (Mutant vs. Wild Type)
The engineered mutant demonstrates a significantly stronger binding affinity compared to the native C/EBP-beta interaction, supporting the "Molecular Trap" hypothesis.

![Binding Affinity Chart](images/Binding_affinity_chart(updated).png)
*(Figure 1: Comparison of Binding Free Energy. The mutant trap shows a >2-fold increase in affinity, achieving -110.6 kcal/mol.)*

#### 2. Structural Validation
The mutant peptide targets the leucine zipper domain of CHOP while preserving the structural integrity of the hydrophobic interface.

![Docking Complex](images/mutant_complex_HQ.png)
*(Figure 2: PyMOL visualization of the Mutant Peptide [Blue] bound to DDIT3 [Gray]. Key mutations are highlighted in yellow.)*

#### 3. Simulation Stability & Dynamics
The 20ns MD trajectory confirmed a dynamic but stable interaction. The RMSD fluctuates between **2Å and 5Å**, displaying characteristic "elastic rebound" behavior common in short peptide chains. This indicates the peptide maintains binding while exhibiting conformational flexibility, rather than dissociating.

![RMSD Plot](images/rmsd_ca.png)
*(Figure 3: 20ns MD Simulation trajectory showing dynamic equilibrium.)*

#### 4. Physicochemical Properties
* **Evolutionary Conservation:** BLAST analysis confirms a **96.77% sequence identity** with human C/EBP-beta, minimizing the risk of immunogenicity.
* **Solubility:** A GRAVY Score of **-1.455** indicates the peptide is highly hydrophilic, suggesting excellent solubility in aqueous cellular environments.

---

### 🔮 Future Directions
* **In Vitro Validation:** Competitive binding assays (EMSA) to confirm CHOP sequestration.
* **Delivery System:** Attaching Cell-Penetrating Peptides (CPPs) for intracellular delivery (essential due to hydrophilic GRAVY score).
* **Population Genetics:** Investigating the conservation of the target interface across diverse human populations.
