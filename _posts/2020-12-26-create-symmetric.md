---
layout: page
title:  "Perfect symmetry from pseudosymmetry"
date: 2020-12-26
tags: research RNA pdb
---
### A symmetric RNA knot {#top_title}
<a href="#top_title">
    <button style="position: fixed; top: 90%; right: 10%; border-radius: 50%; padding: 0.5em 1em;"><i class="fas fa-sync"></i> To top</button>
</a>

We may often obtain a PDB file of a homomeric assembly which, though expected to be symmetric, is not perfectly symmetric. This blog demonstrates how to obtain the perfect symmetry using [three python scripts](/2020-12-24-scripts-collection){:target="_blank"}: `symmetry_2_reorder_args.py`, `reorder_pdb_atom_by_chain.py` and `average_structure_pdb_atom.py`.

I'm using my favorite kind of structure to illustrate the progress. Here is a [PDB file (TJKnot-opt-39-26-B20000000.pdb)](https://www.dropbox.com/s/g6j60opny09ekrh/TJKnot-opt-39-26-B20000000.pdb?dl=0){:target="_blank"} of an RNA knot constructed with **Nanotiler** by connecting six K-turn motifs with RNA helices. **Note** that this file has been pre-treated by [convertPDB](/2020-12-24-scripts-collection){:target="_blank"} so that the PDB file is of the new format.

This RNA knot is expected to be of **C<sub>3</sub>** symmetry. The following image shows the chainIDs for the RNA structure in the original PDB file. The structure contains chains A to X, and the symmetry-related components are [ABCDMNOP, EFGHQRST, IJKLUVWX].
![original](https://www.dropbox.com/s/kbx0tl84a56covw/image-original-chains-TJKnot-forMS.png?raw=1){:width="60%" class="center"}

1.  Use `symmetry_2_reorder_args.py` (mode #2 because the noncontinuous chain order) to get the args for the next-step permutation:
    ```bash
    # command:
    python symmetry_2_reorder_args.py ABCDMNOP EFGHQRST IJKLUVWX
    # output:
    1A 1E 1B 1F 1C 1G 1D 1H 1M 1Q 1N 1R 1O 1S 1P 1T 1E 1I 1F 1J 1G 1K 1H 1L 1Q 1U 1R 1V 1S 1W 1T 1X 1I 1A 1J 1B 1K 1C 1L 1D 1U 1M 1V 1N 1W 1O 1X 1P
    1A 1I 1B 1J 1C 1K 1D 1L 1M 1U 1N 1V 1O 1W 1P 1X 1E 1A 1F 1B 1G 1C 1H 1D 1Q 1M 1R 1N 1S 1O 1T 1P 1I 1E 1J 1F 1K 1G 1L 1H 1U 1Q 1V 1R 1W 1S 1X 1T 
    ```
    <br>
2.  Use `reorder_pdb_atom_by_chain.py` to do the chain permutation based on symmetry:
    ```bash
    # command (using the previous outputs):
    python reorder_pdb_atom_by_chain.py TJKnot-opt-39-26-B20000000.pdb
    python reorder_pdb_atom_by_chain.py TJKnot-opt-39-26-B20000000_reordered.pdb 1A 1E 1B 1F 1C 1G 1D 1H 1M 1Q 1N 1R 1O 1S 1P 1T 1E 1I 1F 1J 1G 1K 1H 1L 1Q 1U 1R 1V 1S 1W 1T 1X 1I 1A 1J 1B 1K 1C 1L 1D 1U 1M 1V 1N 1W 1O 1X 1P
    python reorder_pdb_atom_by_chain.py TJKnot-opt-39-26-B20000000_reordered.pdb 1A 1I 1B 1J 1C 1K 1D 1L 1M 1U 1N 1V 1O 1W 1P 1X 1E 1A 1F 1B 1G 1C 1H 1D 1Q 1M 1R 1N 1S 1O 1T 1P 1I 1E 1J 1F 1K 1G 1L 1H 1U 1Q 1V 1R 1W 1S 1X 1T
    ```
    <br>
3.  Operations in **UCSF Chimera**:
   Open the three newly generated PDB files in **UCSF Chimera** and rotate to align by `match` to **model #0**: `match #1 #0` and `match #2 #0`. Then output the three structures as PDB file with coordinates relative to **model #0**, with filenames such as `TJKnot-opt-39-26-B20000000_sym1.pdb`, `TJKnot-opt-39-26-B20000000_sym2.pdb` and `TJKnot-opt-39-26-B20000000_sym3.pdb`. **Note** that even the coordinates of first PDB file have not been changed, it is safe to save it with **Chimera** so that all the files going into `average_structure_pdb_atom.py` (next step) are of the same format, especially for the "TER" records. It is on my to-do list to improve `average_structure_pdb_atom.py` to make it check the identity of the atoms to be averaged.
   <br>

4.  Use `average_structure_pdb_atom.py` to generate average coordinates:
    ```bash
    # command
    python average_structure_pdb_atom.py TJKnot-opt-39-26-B20000000_sym1.pdb TJKnot-opt-39-26-B20000000_sym2.pdb TJKnot-opt-39-26-B20000000_sym3.pdb
    ```
    The output PDB now has the desired **C<sub>3</sub>** symmetry. It should be something like this file ([TJKnot-opt-39-26-B20000000_sym1_averaged.pdb](https://www.dropbox.com/s/14l2cnv1s266nwj/TJKnot-opt-39-26-B20000000_sym1_averaged.pdb?dl=0){:target="_blank"}).
