# Protein Sequence Alignment - Smith-Waterman Algorithm

This project provides a high-performance C++ implementation of the Smith-Waterman algorithm. It performs local alignment of a protein sequence (query) against a large database of protein sequences. 

This project was developed as part of the INFO-H304 course (Complements of programming and algorithmics) at the École Polytechnique de Bruxelles (ULB).

## Main Features

*   **Dynamic Programming Algorithm:** Strict implementation of the Smith-Waterman recurrence relations using a scoring matrix (BLOSUM) and applying gap opening penalties (GOP) and gap extension penalties (GEP).
*   **Binary File Processing:** Parsing of databases converted via NCBI BLAST+ (`.pin`, `.psq`, `.phr` formats) for optimized reading of offsets and sequences.
*   **Automatic Ranking:** Extraction and sorting of the top 20 best alignment results using a priority queue (heap) from the STL (Standard Template Library), offering an insertion cost of $\mathcal{O}(\log n)$.

## Implemented Optimizations

To efficiently process a database containing hundreds of thousands of proteins (e.g., 573,661 sequences), several optimizations were implemented:

*   **Memory Optimization:** The algorithm no longer stores the entire alignment matrices. By keeping only the current row and the previous row, the spatial complexity is reduced from $\mathcal{O}(M \cdot N)$ to $\mathcal{O}(N)$.
*   **Multithreading:** Parallelization of the search across multiple CPU cores via `std::thread`, allowing several threads to analyze distinct ranges of the database simultaneously before merging the results.
*   **Optimized Compilation:** Use of GCC `-O3` flags to enable automatic vectorization, inlining, and instruction reordering.

## Project Structure

*   `src/` & `headers/`: `.cpp` source files and `.h` headers separating the logic into different classes (`dataPin`, `query`, `Protein`, `Blosum`).
*   `database/`: Target database files. (To setup before starting)
*   `query/`: Input test protein file. (FASTA format, import it manually)
*   `blosum/`: Text files containing the scoring matrices. (Import it manually)
*   `main.cpp`: Main entry point of the program executing the multithreaded version.
*   `Makefile`: Configuration file defining the compilation targets of the project.
*   `INFO_H304_Groupe01.pdf`: Detailed technical report of the project.

## Compilation and Execution

The project uses a `Makefile` to automate compilation. 

```bash
# Compile the project
make projet

# Execute the program (example)
./projet query/P00533.fasta database/testdatabase.fasta blosum/BLOSUM62 11 1
```
You can also compile with `make projetopt`, the returned result will be much faster.

You will expect : 
```bash
sp|P00533|EGFR_HUMAN 6525
sp|P55245|EGFR_MACMU 6487
sp|Q01279|EGFR_MOUSE 5957
sp|P13388|XMRK_XIPMA 3327
sp|Q15303|ERBB4_HUMAN 3269
sp|Q61527|ERBB4_MOUSE 3268
sp|Q9QYX7|PCLO_MOUSE 65
sp|Q9JKS6|PCLO_RAT 62
sp|Q9Y6V0|PCLO_HUMAN 60
sp|P59594|SPIKE_SARS 53
sp|O88737|BSN_MOUSE 51
sp|Q0Q475|SPIKE_BC279 51
sp|Q3LZX1|SPIKE_BCHK3 51
sp|Q9UPA5|BSN_HUMAN 50
sp|P0DTC2|SPIKE_SARS2 47
sp|Q9PU36|PCLO_CHICK 47
sp|Q3I5J5|SPIKE_BCRP3 46
sp|A3EXG6|SPIKE_BCHK9 40
sp|P28469|ADH1A_MACMU 37
sp|P00325|ADH1B_HUMAN 35
```
For more information, check `INFO_H304_Groupe01.pdf` file.
