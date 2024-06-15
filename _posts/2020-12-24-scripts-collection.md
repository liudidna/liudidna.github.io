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
- PDB record: **I** is int, **S** is string, **F** is float;
- 
  | Record  | recordName | serial     | name             | resName          | chainID         | resSeq      | x           | y           | z           |
  |---------|------------|------------|------------------|------------------|-----------------|-------------|-------------|-------------|-------------|
  | Type    | String(6)  | String(5)  | String(4)        | String(3)        | String(1)       | Integer(4)  | Real(8.3)   | Real(8.3)   | Real(8.3)   |
  | Get     | S[:6]      | I(S[6:11]) | S[12:16].strip() | S[17:20].strip() | S[21] (?S[20]?) | I(S[22:26]) | F(S[30:38]) | F(S[38:46]) | F(S[46:54]) |
  | Example | HETATM     |     2      |   PB             | GTP              |  X              |   10        |             |             |             |
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
- For the former usage, the file is reordered based on the alphabetic order of the chains;
- For the latter usage, residues (chainID and resSeq) are changed by adapting `res_renumber_pdb_atom.py` (which calls **SimRNA**'s `pdb_2_SimRNA_dist_restrs_aligned.py`);
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
  1. It first moves an ideal nucleotide (*"sample_nt"*) so that it is aligned with the 5'-nucleotide (*"curr_head_res"*) based on *"C5'", "O4'", "C4'", "C1'"* ("O5'" is not necessary, though it can be added by using [pdbfixer](https://github.com/openmm/pdbfixer){:target="_blank"} ); 
  2. Then, the moved *"sample_nt"* is slightly moved so that its "C5'" is aligned with that of *"curr_head_res"* with the movement matrix reported to console for evaluation; 
  3. Extra "O5'" atoms will be deleted; 
  4. Lastly, the phosphate of *"sample_nt"* is grafted onto *"curr_head_res"*.
- The output PDB file has the serials reordered.
- <i class="far fa-comment-dots"></i> ***Coding notes:***
  - `import numpy as np`, to use the `np.array()` function to create `ndarray`, to use `np.matmul()` function for matrix product, and to use `tolist()` method of `ndarray`;
  - `copy.deepcopy(some_instance)` from `copy` module;
  - `superpose3d.Superpose3D(frozen_cloud, mobile_cloud)` from `superpose3d` module: the function returns `(RMSD, R, T, c)`, [more details here](https://pypi.org/project/superpose3d/){:target="_blank"}.

### <i class="fab fa-python"></i> <i class="far fa-comment-dots"></i> 3 related scripts: (a) `add_triphosphate_pdb.py`: add 5'-triphosphate, (b) `add_cyclicphosphate_pdb.py`: add 2',3'-cyclic phosphate, and (c) `remove_extraphosphate_pdb.py`: remove 5'-triphosphate or 2',3'-cyclic phosphate
- Access `add_triphosphate_pdb.py` by the [link](https://www.dropbox.com/s/5zq14t0w1u6ybos/add_triphosphate_pdb.py?dl=0){:target="_blank"}, and use by `python add_triphosphate_pdb.py PDBfile_name chainID1 chainID2 ...`;
- Access `add_cyclicphosphate_pdb.py` by the [link](https://www.dropbox.com/s/djcwcciktaglebx/add_cyclicphosphate_pdb.py?dl=0){:target="_blank"}, and use by `add_cyclicphosphate_pdb.py PDBfile_name chainID1 chainID2 ...`;
- A current limitation with `add_cyclicphosphate_pdb.py` is that the chainID of the input PDB file should be ordered;
- Access `remove_extraphosphate_pdb.py` by the [link](https://www.dropbox.com/s/o826vcld860vv0y/remove_extraphosphate_pdb.py?dl=0){:target="_blank"}, and use by `remove_extraphosphate_pdb.py PDBfile_name`;
- The three scripts work in a similar with as the `add_phosphate_pdb.py` [above](#--add_phosphate_pdbpy-add-5-end-phosphates-to-rna-strands).
- <i class="far fa-comment-dots"></i> ***Coding notes:***
  - To merge two dictionaries: 
    ```python
    resName_change = resName_triphosphate_change.copy()
    resName_change.update(resName_cyclicphosphate_change)
    ``` 
  - [more details from stackoverflow](https://stackoverflow.com/questions/38987/how-do-i-merge-two-dictionaries-in-a-single-expression-taking-union-of-dictiona#){:target="_blank"}.

### <i class="fab fa-python"></i> <i class="far fa-comment-dots"></i> `extend_helix_pdb.py`: extend helix using ideal A-form helix, depending on two script (a) `append_one_bp_pdb.py`: adding one bp each time, and (b) `insert_res_pdb_atom.py`: adding a list of pdb atom records
- Access `extend_helix_pdb.py` by the [link](https://www.dropbox.com/s/qekykhrmi5suo3r/extend_helix_pdb.py?dl=0){:target="_blank"}, and use by `python extend_helix_pdb.py file.pdb 123A AUCG 153B CCGGU ...`;
- Access `append_one_bp_pdb.py` by the [link](https://www.dropbox.com/s/weg6bgx7eurz5xi/append_one_bp_pdb.py?dl=0){:target="_blank"}, and use its core function by `append_one_bp_pdb.append_one_bp(rec_list, target_3P, each_base, target_5P)`, `target_3P` and `target_5P` are the residue markers (such as `99c`) for the 3' and 5' residues, and `target_5P` is optional (can be located by `find_pair_5P(rec_list, target_3P)`);
- Access `insert_res_pdb_atom.py` by the [link](https://www.dropbox.com/s/1motv9ukvj2zz9x/insert_res_pdb_atom.py?dl=0){:target="_blank"}, and use its core function by `res_insert(rec_list, target_3P, rec_insert, mode="post")` or `res_insert(rec_list, target_3P, rec_insert, mode="post"`);
- Warnings will be given for `TER` record when the end of the file does not have `TER` record.
- <i class="far fa-comment-dots"></i> ***Coding notes:***
  - Floor division operator `//`: 
    ```python
    print(5//2) # 2
    print(-5//2) # -3
    print(5.0//2) # 2.0
    ``` 
  - Handle potential errors:
    ```python
    try:
      if rec_list[insert_index+len(rec_insert)].recordName == "TER":
        rec_list[insert_index+len(rec_insert)].update_resName(rec_insert[-1].resName)
        rec_list[insert_index+len(rec_insert)].update_resSeq(rec_insert[-1].resSeq)
    except IndexError:
      print("IndexError occurs at %s%s" % (rec_insert[-1].resSeq, rec_insert[-1].chainID))
      print("But no worries! It's the end that has no TER record.")
    ```
  - distance between two points: `curr_dist = np.linalg.norm(np.array(coord_5P_O3)-np.array(coord_curr_P))` with `import numpy as np`.

### <i class="fab fa-python"></i>  `chains_pdb.py`: get chain properties from an input pdb file
- Access `chains_pdb.py` by the [link](https://www.dropbox.com/s/2vbfx019pmfyft5/chains_pdb.py?dl=0){:target="_blank"}, and use by `python chains_pdb.py filename` for printing the properties of all chains to console, or `python chains_pdb.py A B X ...` for the first usage plus extracting individual chains;
- Core function `def get_chain_properties(rec_list, chain_rec_list = None):` which `return chain_dict_list`, a list of chain_dict (containing the properties of a certain chain); if `chain_rec_list` is provided (as an an empty list) atoms of all the chains will be written to this `chain_rec_list`;
- Printing function `def print_chain_properties(chain_dict_list):`. 

### <i class="fab fa-python"></i>  `ligate_strand_pdb.py`: ligate chains from an input pdb file
- Access `ligate_strand_pdb.py` by the [link](https://www.dropbox.com/s/mz3fgz41yxkc0zj/ligate_strand_pdb.py?dl=0){:target="_blank"}, and use by `python ligate_strand_pdb.py file.pdb A B X ...` to ligate the chains start with A, B, X... (automatically searching for the next possible chains for ligation);
- This script also relies on `chains_pdb.py` ([link](/2020-12-24-scripts-collection/#--chains_pdbpy-get-chain-properties-from-an-input-pdb-file)), `insert_res_pdb_atom.py` ([link](/2020-12-24-scripts-collection/#--extend_helix_pdbpy-extend-helix-using-ideal-a-form-helix-depending-on-two-script-a-append_one_bp_pdbpy-adding-one-bp-each-time-and-b-insert_res_pdb_atompy-adding-a-list-of-pdb-atom-records)) and `delete_res_pdb_atom.py` (accessible by [link](https://www.dropbox.com/s/ct0u3by92t27u82/delete_res_pdb_atom.py?dl=0){:target="_blank"}). 

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

### <i class="fab fa-python"></i> `stack_pdb_atom.py`: create a PDB file containing a stacked helix or dinucleotide
- Access `stack_pdb_atom.py` by the [link](https://www.dropbox.com/s/39sjy98a2zzr11d/get_sticky_from_seq_ss.py?dl=0){:target="_blank"};
- Use by, for example, `python stack_pdb_atom.py GC 203X 204X 77C 105D` (usage1 for 4 nt or 2 bp) or `python stack_pdb_atom.py GC 203X 204X` (usage2 for 2 nt), where `GC` is the sequence for `203X` and `204X` while `77C` is complementary to `204X` and `105D` is complementary to `203X`;
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
- Use by `python transfer_savepos_chimera.py from-filename to-filename` to reorder records, while ***from-file*** must exist, ***to-file*** with `to-filename` will be created if not existing;
- The script looks for the line starting with `"\tformattedPositions = "` to locate the **dictionary** of the saved positions (note that for a Chimera session (`.py`) file with no `savepos`, `formattedPositions = {}` exists in the `.py` file);
- <i class="far fa-comment-dots"></i> ***Coding notes:***
  - `target_frompos = ast.literal_eval(target_frompos_string)`: `literal_eval()` function from `ast` module to convert strings to Python literal structures;
  - `if 'tolines' not in locals():` to check whether ( or not) the variable has been defined, *i.e.* `locals()` function returns the dictionary of current local symbol table.

### <i class="fab fa-cuttlefish"></i> `compare_chimera_swapna.cpp` need to convert from C++ to python3

## Related to Phenix
### <i class="fab fa-python"></i> <i class="far fa-comment-dots"></i> `phenix_na_ss.py`: create the class for storing records of [secondary structure restraints of Phenix](https://phenix-online.org/documentation/reference/secondary_structure.html){:target="_blank"} and reorder these records
- Access `phenix_na_ss.py` by the [link](https://www.dropbox.com/s/54a7iumd3j1ne9m/phenix_na_ss.py?dl=0){:target="_blank"};
- Use by `python phenix_na_ss.py from-filename to-filename`, while ***from-file*** must exist, ***to-file*** with `to-filename` will be created;
- `class base_pair_record` and `class stacking_pair_record` is created by 3 or 2 lines of the records in file:
  ```python
  pdb_interpretation {
    secondary_structure {
      nucleic_acid {
        base_pair {
          base1 = chain 'A' and resid 372
          base2 = chain 'A' and resid 399
          saenger_class = 20 # 19 for GC, 20 for AU, 28 for GU
        }
        stacking_pair {
          base1 = chain 'A' and resid 417
          base2 = chain 'A' and resid 418
        }
      }
      enabled = True
    }
  }
  ```
- <i class="far fa-comment-dots"></i> ***Coding notes:***
  - **tuples** can be compared, such as `if (self.base1["chain"], self.base1["resid"]) > (self.base2["chain"], self.base2["resid"]):`;
  - **tuples** can be used as the `key` for `sorted()`, such as `sorted(record_list, key=lambda x: (x.base1["chain"], x.base1["resid"]))`.

### <i class="fab fa-python"></i> <i class="far fa-comment-dots"></i> `phenix_res_from_seq_ss.py`: convert secondary structure denotations to Phenix secondary structure restraints
- Access `phenix_res_from_seq_ss.py` by the [link](https://www.dropbox.com/s/8fmkbve1ubft34g/phenix_res_from_seq_ss.py?dl=0){:target="_blank"};
- Use by `python phenix_res_from_seq_ss.py seq_file ss_file out_file`, where `seq_file` and `ss_file` are **SimRNA** sequence file and secondary-structure file;
- The script is dependent on [`get_sticky_from_seq_ss.py`](#-get_sticky_from_seq_sspy-create-stacking-restraints-for-simrna) and [`phenix_na_ss.py`](#--phenix_na_sspy-create-the-class-for-storing-records-of-secondary-structure-restraints-of-phenix-and-reorder-these-records).
- <i class="far fa-comment-dots"></i> ***Coding notes:***
  - Even use `from file_other improt some_function`, all the codes in `file_other` will be executed, solution is `if __name__ == '__main__':`;
  - Directly assigning a new value to an object such as `list` in function will not change its value outside of the function, solution is `return`.


## Other
### <i class="fab fa-python"></i> <i class="far fa-comment-dots"></i> `ranking_energy_nanotiler.py`: post-process Nanotiler output log files
- Access `ranking_energy_nanotiler.py` by the [link](https://www.dropbox.com/s/bk25p4jo339n445/ranking_energy_nanotiler.py?dl=0){:target="_blank"};
- Use by `python ranking_energy_nanotiler.py nt_outfile` or `python ranking_energy_nanotiler.py nt_outfilename1 nt_outfilename2`, where ***nt_outfile(1/2)*** is a **Nanotiler** output log file;
- For the latter use, both files will be processed, and an extra "Energy_sum ranking" file of `.sum` will be generated.
- The output files should contain the markers written by the nanotiler script files, which should contain:
  ```bash
  # 100*"#"
  echo ####################################################################################################
  echo HBP1: ${HBP1}   HBP2: ${HBP2}
  # 96*"#"
  echo ################################################################################################
  ```
- So the script can be modified based on the markers written by the **Nanotiler** script;
- <i class="far fa-comment-dots"></i> ***Coding notes:***
  - `from pathlib import Path` to convert a string to a `Path` object by `p=Path(nt_outfilename)`, most commonly used `p.is_file()`, `p.stem`, and `p.suffix`, for [more info](https://rednafi.github.io/digressions/python/2020/04/13/python-pathlib.html){:target="_blank"};
  - **Regex**: use `re` module's `re.compile()` function to create **Regex** objects: `float_re = re.compile(r"([+-]?)(\d+)\.(\d+)([Ee]?)([+-]?)(\d*)")` & `float_str = float_re.search(s_s_str).group(0)` for search patterns of a *float* and get its *string*, [phenix_na_ss.py](/2020-12-24-scripts-collection/#--phenix_na_sspy-create-the-class-for-storing-records-of-secondary-structure-restraints-of-phenix-and-reorder-these-records) also uses **Regex**;
  - `sorted(dict_energy.items(), key=lambda x: x[1])` to get a list containing the items of a *dictionary* sorted by the *values*.

### <i class="fab fa-python"></i>  `ranking_energy_nanotiler.py`: search 
- Access `RNA_complement_search.py` by the [link](blank){:target="_blank"};
- Use by `python ranking_energy_nanotiler.py nt_outfile` or `python ranking_energy_nanotiler.py nt_outfilename1 nt_outfilename2`, where ***nt_outfile(1/2)*** is a **Nanotiler** output log file;
- For the latter use, both files will be processed, and an extra "Energy_sum ranking" file of `.sum` will be generated.
- The output files should contain the markers written by the nanotiler script files, which should contain:
  ```bash
  #short
  seq_short1 (must be in a single line)
  seq_short2

  #long
  seq_long (can be in multiple lines)

  #restriction
  GAAGAGC (can be multiple sequences in multiple lines)

  #start
  ```
- So the script can be modified based on the markers written by the **Nanotiler** script;


### <i class="fab fa-cuttlefish"></i> `gen_qrnas_bp_restraint.cpp` need to convert from C++ to python3 (qrna_bp_from_seq_ss.py will be added!)