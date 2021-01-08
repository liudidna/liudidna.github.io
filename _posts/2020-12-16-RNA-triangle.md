---
layout: page
title:  "RNA triangle modeling"
date: 2020-12-16
tags: research RNA
---

### Contents {#top_title}
{: .no_toc}

- TOC
{:toc}

---

<a href="#top_title">
    <button style="position: fixed; top: 90%; right: 10%; border-radius: 50%; padding: 0.5em 1em;"><i class="fas fa-sync"></i> To top</button>
</a>

## Prepare RNA motif
### Prepare cleaned RNA motif from PDB file

We are going to use the PDB entry `3P59`, an RNA square, as an example. The conformation of the bulge motif formed by strands E and F is in an alternative conformation. We will first try to build an RNA triangle use the EF motif. 
In Chimera, trim the RNA structure so that only residues `50.E` to `60.E`, and `108.F` to `115.F` are left. Save the PDB of the new model. Now, the bulge motif is flanked by a 2-bp helix and a 3-bp helix on either side. This is problematic, because **nanotiler**[^1]. needs a helix of a minimum of 3-bp to recognize the helix.

[^1]: Shapiro.

### Align helix onto the motif (*only for motif flanked by less than 3-bp helix*)
To fix this, in Chimera, build an ideal 3-bp RNA A-form helix as a new model. Then align the newly built helix onto the 2-bp helix of the cleaned bulge model by the Chimera command:
```
match #1:2-3.A,1-2.B@c3',c4',c5' #0:112-113.F,50-51.E@c3',c4',c5'
```
Combine the two models, and save the PDB as `bulge_EF_aligned_helix.pdb`.  
<br>

---

## Construct triangle with nanotiler

### Locate the helix ends in nanotiler
We need to find the identifiers of the ends of helices for writing the subsequent **nanotiler** scripts. With terminal open in the working directories:

```
nanoscript
import bulge_EF_aligned_helix.pdb findstems=true name=bulge1
exportpdb bulge_EF_aligned_helix-TMP.pdb junction=false
tree hxend
exit
```
We will get a new PDB file `bulge_EF_aligned_helix-TMP.pdb` and get output like:
```
root.bulge1.stems.stemA_B_1.B_3_A_9 1.1.5.1.7 1 BranchDescriptor3D  41.804 -22.104  10.349
root.bulge1.stems.stemA_B_1.A_11_B_1 1.1.5.1.8 1 BranchDescriptor3D  34.526 -15.262  12.313
root.bulge1.stems.stemC_D_1.D_3_C_1 1.1.5.2.7 1 BranchDescriptor3D  42.802 -30.639  17.498
root.bulge1.stems.stemC_D_1.C_3_D_1 1.1.5.2.8 1 BranchDescriptor3D  35.113 -35.754  13.214
```
Use Chimera to open `bulge_EF_aligned_helix-TMP.pdb` (note that the numbering of chains and residues has been changed by **nanotiler**) for the identifiers of the helix ends, which are `root.bulge1.stems.stemA_B_1.A_11_B_1` and `root.bulge1.stems.stemC_D_1.C_3_D_1`. We will use `stems.stemA_B_1.A_11_B_1` and `stems.stemC_D_1.C_3_D_1` in the following scripts.

### Search the optimal lengths of connecting helices with nanotiler script
This [searching script](https://www.dropbox.com/s/0piubrqwyl2kyym/bulgeEF-newHelixScan-NanoTiler.script?dl=0){:target="_blank"} will search connecting helices lengths ranging from 12 to 17 bps. Note that we designated `stems.stemA_B_1.A_11_B_1` and `stems.stemC_D_1.C_3_D_1` as `JD1` and `JD2` in the script. Run the script with:
```bash
# no console output
nanoscript bulgeEF-newHelixScan-NanoTiler.script > bulgeEF-newHelixScan-NanoTiler.out 2>&1
```
Or:
```bash
# with console output
nanoscript bulgeEF-newHelixScan-NanoTiler.script 2>&1 | tee bulgeEF-newHelixScan-NanoTiler.out
```
The searching results are written in the `bulgeEF-newHelixScan-NanoTiler.out` output file. To quickly locate the energy (`start_score` of last helix restraint), use the commands:
```
grep -e "####" -e "HBP1: " -e "generated_helices=h_root_bulge3_stems_stemC_D_1_C_3_D_1_root_bulge1_stems_stemA_B_1_A_11_B_1_root" bulgeEF-newHelixScan-NanoTiler.out > bulgeEF-newHelixScan-NanoTiler-energy-summary.out
```
We can see that the optimal length of the connecting helix is 14 (`start_score=3430`)or 15 (`start_score=2193`).

### Rigid-body energy optimization with nanotiler script
Taking the helix length of 14, we next optimize the placement of the motifs and helices use the [optimization script](https://www.dropbox.com/s/7otydthgec83alb/bulgeEF_newOpt14-NanoTiler.script?dl=0){:target="_blank"}. Note that the final optimization steps (`optcycles` in the script) should not be too small. Run the commands:
```
nanoscript bulgeEF_newOpt14-NanoTiler.script 2>&1 | tee bulgeEF_newOpt14-NanoTiler.out
```
We get the output PDB file `bulgeEF_newOpt14-5000000.pdb`. 

### Post-processing of the output PDB file
Load the output file and remove the strands of the aligned helices:
```
nanoscript
import bulgeEF_newOpt14-5000000.pdb findstems=true name=triangle
tree strand
```

```
convertPDB bulgeEF_newOpt14-5000000-post-3.pdb  
```
<br>


---

## Coarse-grain simulation using simRNA

### Sequence file and secondary structure file
Prepare the sequence file and secondary structure file according to simRNA.
Add 5'-phosphate groups if they are lost in the structure.

### Motif and sticky-end stacking restraints
To generate motif restraints, first trim the helices flanking the bulge to 1-bp, save the PDB as `bulgeEF_1bp_for_restraints.pdb`, which can be accessed [here](https://www.dropbox.com/s/7u1rhyk0eovxi9v/bulgeEF_1bp_for_restraints.pdb?dl=0). Use the following commands:
```bash
cat > motif_restraints
python res_renumber_pdb_atom.py bulgeEF_1bp_for_restraints.pdb 58E 19A 110F 13B
python res_renumber_pdb_atom.py bulgeEF_1bp_for_restraints.pdb 58E 19C 110F 13D
python res_renumber_pdb_atom.py bulgeEF_1bp_for_restraints.pdb 58E 19E 110F 13F
<control+d>
source motif_restraints
ls | grep restraints_bulgeEF_1bp | xargs cat > restraints_motif.dat
```

For sticky-end stacking restraints, 
```
python /Users/wyssuser/programs/SimRNA_64bitIntel_MacOSX_staticLibs/simrna_scripts-master/get_sticky_from_seq_ss.py bulgeEF_triangle.seq bulgeEF_triangle.ss
ls | grep restraintsSE | xargs cat >> restraints_SEall.dat
```

Change the weight to 0.2
```
cat restraints_motif.dat restraints_SEall0.2.dat > Allrestraints_SE0.2.dat
```

### Run simulation with simRNA
```bash
ln -s $SIMRNADATA data
SimRNA -c config.dat -p bulgeEF_newOpt14-5000000-post-3.pdb -S bulgeEF_triangle.ss -r Allrestraints_SE0.2.dat -o bulgeEF_triangle_try1 2>&1 | tee bulgeEF_triangle_try1.out
```
<br>
<!-- <button><i class="fas fa-sync"></i> [To top](#top_title)</button> -->
