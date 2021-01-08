---
layout: page
title:  "Collection of some scripts"
date: 2020-12-24
tags: research RNA pdb python C++
---

### Contents {#top_title}
{: .no_toc}

- TOC
{:toc}

---

<a href="#top_title">
    <button style="position: fixed; top: 90%; right: 10%; border-radius: 50%; padding: 0.5em 1em;"><i class="fas fa-sync"></i> To top</button>
</a>

<!-- Shared files are in the dropbox folder "/Users/wyssuser/AllDropbox/Dropbox (Personal)/research/LabSharing/softwares/self_pythonpath_db". This folder is synced with "/Users/wyssuser/programs/self_pythonpath".  -->

{: .box-note}
***Note**: If not specified otherwise, all python scripts should be run with `python3.x`. The scripts are currently only tested with `macOS Mojave`. Feel free to contact me if the links expire or there's any other question.*

## Related to PDB files
### <i class="fab fa-python"></i> <i class="far fa-comment-dots"></i> `edit_pdb_atom.py`: parse and edit PDB files
- Access `edit_pdb_atom.py` by the [link](https://www.dropbox.com/s/b712mcc35a0kc9z/edit_pdb_atom.py?dl=0){:target="_blank"};
- It is used to parse pdb files, mainly for being imported of other scripts: so consider adding the directory containing it to `PYTHONPATH` by `export PYTHONPATH=$PYTHONPATH:PATH_TO_THE_DIRECTORY`, or use `sys.path.append("PATH_TO_THE_DIRECTORY")`;
- It contains:
  - class `pdb_atom_record`
  - class `pdb_ter_record` (inherited from `pdb_atom_record`) 
  - functions `file2rec` and `rec2file`: to read from and write into files
- <i class="far fa-comment-dots"></i> ***Coding notes:***
  - Pass a class member as a default value for a class method, see `update_resSeq` method:
    ```python
    def update_resSeq(self, new_resSeq=None, by_shift=0): # new_resSeq=self.resSeq generates error!
        if new_resSeq is None:
            new_resSeq = self.resSeq
        ...
    ```
  -  Use`print(edit_pdb_atom.__doc__)` or `help(edit_pdb_atom)` to read the `docstring` or more info.

### <i class="fab fa-python"></i> <i class="far fa-comment-dots"></i> `res_renumber_pdb_atom.py`: renumber the residue by both resSeq and chainID
- Access `res_renumber_pdb_atom.py` by the [link](https://www.dropbox.com/s/o3ljcw4xflpm93c/res_renumber_pdb_atom.py?dl=0){:target="_blank"};
- Use by `python res_renumber_pdb_atom.py file.pdb 123A 456X 153B 356Y xxX yyY`, where `xxX` and `yyY` are old and new residues;
- It will also generate restraints for **SimRNA** by calling `pdb_2_SimRNA_dist_restrs_aligned.py` with `os.system("some string of CMD")`, useful for restraining known motifs;
- <i class="far fa-comment-dots"></i> ***Coding notes:***
  - `sys.argv`, `sys.exit(1)` from `sys` module;
  - `os.path.isfile(some_file_path)`, `os.system("some string of CMD")` from `os` module;
  - `os.path...` should not be confused with `sys.path...` (from `sys` module).

### <i class="fab fa-python"></i> `reorder_pdb_atom_by_chain.py`: reorder the records of atoms based on the alphabetic order of chainIDs
- Access `reorder_pdb_atom_by_chain.py` by the [link](https://www.dropbox.com/s/8bb08e2g1kulo80/reorder_pdb_atom_by_chain.py?dl=0){:target="_blank"};
- Use by `python reorder_pdb_atom_by_chain.py file.pdb` or `python reorder_pdb_atom_by_chain.py file.pdb 123A 456X 153B 356Y xxX yyY`, where `xxX` and `yyY` are old and new residues;
- For the latter, residues (chainID and resSeq) are changed by adapting `res_renumber_pdb_atom.py`
- If you only want to change the chainID, you can do something like `python reorder_pdb_atom_by_chain.py file.pdb 1A 1X 1B 1Y ... ...` so that there is **no shift** of the resSeq.

### <i class="fab fa-python"></i> <i class="far fa-comment-dots"></i> `symmetry_2_reorder_args.py`: generate args based on symmetry for `reorder_pdb_atom_by_chain.py`
- Access `symmetry_2_reorder_args.py` by the [link](https://www.dropbox.com/s/juxs8gh7eqdp4af/symmetry_2_reorder_args.py?dl=0){:target="_blank"};
- Two modes to use:
  1.  *Concise*: `python symmetry_2_reorder_args.py 3 AF`, where `3` is the symmetry fold, `AF` means contains chains A to F;
  2.  *Verbose*: `python symmetry_2_reorder_args.py AB CD EF`, where all the symmetry-related components are indicated.
- Outputs to appear in console can be directly used as args for script `reorder_pdb_atom_by_chain.py`;
- The three scripts `symmetry_2_reorder_args.py`, `reorder_pdb_atom_by_chain.py` and `average_structure_pdb_atom.py` are useful to generate symmetric oligomers (see [an example](/2020-12-26-create-symmetric){:target="_blank"});
- <i class="far fa-comment-dots"></i> ***Coding notes:***
  - An iterator, once exhausted, may not be reused. In Python 3.x, `zip()` returns an iterator. [Similar question](https://stackoverflow.com/questions/50465966/re-using-zip-iterator-in-python-3/50466346);
  - Enable its repeated usage by converting to a list `zipping = list(zip(*sym_partners))`, where `sym_partners` is a list of lists (*i.e.* 2D array) in the script.

### <i class="fab fa-python"></i> `average_structure_pdb_atom.py`: generate the average coordinates of the structures from multiple PDB files
- Access `average_structure_pdb_atom.py` by the [link](https://www.dropbox.com/s/en45yzes2xfu4r6/average_structure_pdb_atom.py?dl=0){:target="_blank"};
- Use by `python average_structure_pdb_atom.py file1.pdb file2.pdb ...`;
- The output files from `reorder_pdb_atom_by_chain.py` are preferred due to the cleaned formats;
- Useful to generate the averaged symmetric structure, see the [link](/2020-12-16-RNA-triangle){:target="_blank"}.

### <i class="fab fa-python"></i> <i class="far fa-comment-dots"></i> `add_phosphate_pdb.py`: add 5'-end phosphates to RNA strands
- Access `add_phosphate_pdb.py` by the [link](https://www.dropbox.com/s/ik90nozp48040wr/add_phosphate_pdb.py?dl=0){:target="_blank"};
- Use by `python add_phosphate_pdb.py PDBfile_name`;
- Operations:
  1. It first moves an ideal nucleotide (*"sample_nt"*) so that it is aligned with the 5'-nucleotide (*"curr_head_res"*) based on *"C5'", "O4'", "C4'", "C1'", "O5'"*; 
  2. Then, the moved *"sample_nt"* is slightly moved so that its "O5'" is aligned with that of *"curr_head_res"* with the movement matrix reported to console for evaluation; 
  3. Lastly, the phosphate of *"sample_nt"* is grafted onto *"curr_head_res"*.
- The output PDB file has the serials reordered.
- <i class="far fa-comment-dots"></i> ***Coding notes:***
  - `import numpy as np`, to use the `np.array()` function to create `ndarray`, to use `np.matmul()` function for matrix product, and to use `tolist()` method of `ndarray`;
  - `copy.deepcopy(some_instance)` from `copy` module;
  - `superpose3d.Superpose3D(frozen_cloud, mobile_cloud)` from `superpose3d` module: the function returns `(RMSD, R, T, c)`, [more details here](https://pypi.org/project/superpose3d/){:target="_blank"}.

### <i class="fas fa-terminal"></i> `convertPDB`: shell script for PDB format conversion
- Access `convertPDB` by the [link](https://www.dropbox.com/s/fkm339n6qfb2hvf/convertPDB?dl=0){:target="_blank"} or [preview as text](https://www.dropbox.com/s/ovnp2rid9nvcpg6/convertPDB.sh?dl=0){:target="_blank"};
- `convertPDB -h` for usage help;
- Useful for dealing with old format PDB, **qrnas** output, and **vmd** output.

## Related to SimRNA
### <i class="fab fa-python"></i> `get_sticky_from_seq_ss.py`: create stacking restraints for SimRNA
- Access `get_sticky_from_seq_ss.py` by the [link](https://www.dropbox.com/s/39sjy98a2zzr11d/get_sticky_from_seq_ss.py?dl=0){:target="_blank"};
- Use by `python get_sticky_from_seq_ss.py seq_file ss_file`, where `seq_file` and `ss_file` are **SimRNA**'s sequence file and secondary-structure file;
- It works by:
  1. Extracting the sticky-end information from the two input files;
  2. Then creating a PDB file containing the stacked bases from an ideal RNA helix corresponding to each sticky-end by calling `stack_pdb_atom.py`;
  3. Lastly generating the SimRNA restraints for sticky-end (more precisely, nicked end; output file `restraintsSE_xX.dat`) by calling `pdb_2_SimRNA_dist_restrs_aligned.py`.
- Be prepared that a lot of PDB files and restraint files will be generated.

### <i class="fab fa-python"></i> `stack_pdb_atom.py`: create a PDB file containing a stacked helix
- Access `stack_pdb_atom.py` by the [link](https://www.dropbox.com/s/39sjy98a2zzr11d/get_sticky_from_seq_ss.py?dl=0){:target="_blank"};
- Use by, for example, `python stack_pdb_atom.py GC 203X 204X 77C 105D`, where `GC` is the sequence for `203X` and `204X` while `77C` is complementary to `204X` and `105D` is complementary to `203X`;
- It works by:
  1. Reading the identities of the 4 residues by the 5 input args;
  2. Then creating a PDB file containing the stacked bases using an ideal RNA helix.
- The main function is for generating restraint files of sticky-ends together with `pdb_2_SimRNA_dist_restrs_aligned.py` for **SimRNA**.

### <i class="fab fa-python"></i> `terminal_pdb_atom.py`: create a PDB file containing a base pair
- Access `terminal_pdb_atom.py` by the [link](https://www.dropbox.com/s/5icjzo1av8p3zyh/terminal_pdb_atom.py?dl=0){:target="_blank"};
- Use by, for example, `python terminal_pdb_atom.py GC 203A 77B`, for a `GC` pair formed by `203A` and `77B`;
- It works by:
  1. Reading the identities of the 2 residues by the 3 input args;
  2. Then creating a PDB file containing the pairing bases using an ideal RNA helix.
- The main function is for generating restraint files of base pairs together with `pdb_2_SimRNA_dist_restrs_aligned.py` for **SimRNA**.

### <i class="fab fa-python"></i> `pdb_2_SimRNA_dist_restrs_aligned.py`: create SimRNA distance restraints
- Access `pdb_2_SimRNA_dist_restrs_aligned.py` by the [link](https://www.dropbox.com/s/0lshfjut2q2loov/pdb_2_SimRNA_dist_restrs_aligned.py?dl=0){:target="_blank"};
- It is adapted from [*Michal Boniecki*](https://link.springer.com/protocol/10.1007/978-1-0716-0708-4_6){:target="_blank"}'s script and changes including aligned output and compatibility for python3.x;
- Use by `python pdb_2_SimRNA_dist_restrs_aligned.py file.pdb TOLERANCE(optional, default:0.2)`.


### <i class="fab fa-python"></i> `trafl_reduce.py`: reduce the amount of coordinates in the SimRNA trafl file
- Access `trafl_reduce.py` by the [link](https://www.dropbox.com/s/bme5awehqc90ube/trafl_reduce.py?dl=0){:target="_blank"};
- Use by `python trafl_reduce.py filename.trafl reduction_rate(such as '5' or 'P' for backbone phosphate)`;
- If `reduction_rate` is an `int`, it will reduce the amount of residues by a factor of this `int`; if `P`, only coordinates for P atoms will be extracted.


## Related to USCF Chimera
### <i class="fab fa-python"></i> <i class="far fa-comment-dots"></i> `transfer_savepos_chimera.py`: transfer Chimera `savepos` positions
- Access `transfer_savepos_chimera.py` by the [link](https://www.dropbox.com/s/tsyx2e3h9vxx072/transfer_savepos_chimera.py?dl=0){:target="_blank"};
- Use by `python transfer_savepos_chimera.py from-filename to-filename`, while ***from-file*** must exist, ***to-file*** with `to-filename` will be created if not existing;
- The script looks for the line starting with `"\tformattedPositions = "` to locate the **dictionary** of the saved positions (note that for a Chimera session (`.py`) file with no `savepos`, `formattedPositions = {}` exists in the `.py` file);
- <i class="far fa-comment-dots"></i> ***Coding notes:***
  - `target_frompos = ast.literal_eval(target_frompos_string)`: `literal_eval()` function from `ast` module to convert strings to Python literal structures;
  - `if 'tolines' not in locals():` to check whether ( or not) the variable has been defined, *i.e.* `locals()` function returns the dictionary of current local symbol table.

### <i class="fab fa-cuttlefish"></i> `compare_chimera_swapna.cpp` need to convert from C++ to python3

## Other
### <i class="fab fa-python"></i> <i class="far fa-comment-dots"></i> `ranking_energy_nanotiler.py`: post-process Nanotiler output log files
- Access `ranking_energy_nanotiler.py` by the [link](https://www.dropbox.com/s/bk25p4jo339n445/ranking_energy_nanotiler.py?dl=0){:target="_blank"};
- Use by `python ranking_energy_nanotiler.py nt_outfile`, where ***nt_outfile*** is a **Nanotiler** output log file;
- The  output files should contain the markers written by the nanotiler script files, which should contain:
  ```bash
  # 100*"#"
  echo ####################################################################################################
  echo HBP1: ${HBP1}   HBP2: ${HBP2}
  # 96*"#"
  echo ################################################################################################
  ```
- So the script can be modified based on the markers written by the **Nanotiler** script;
- <i class="far fa-comment-dots"></i> ***Coding notes:***
  - `from pathlib import Path` to convert a string to a `Path` object by `p=Path(nt_outfilename)`;
  - **Regex**: use `re` module's `re.compile()` function to create **Regex** objects: `float_re = re.compile(r"([+-]?)(\d+)\.(\d+)([Ee]?)([+-]?)(\d*)")` & `float_str = float_re.search(s_s_str).group(0)` for search patterns of a *float* and get its *string*;
  - `sorted(dict_energy.items(), key=lambda x: x[1])` to get a list containing the items of a *dictionary* sorted by the *values*.


### <i class="fab fa-cuttlefish"></i> `gen_qrnas_bp_restraint.cpp` need to convert from C++ to python3