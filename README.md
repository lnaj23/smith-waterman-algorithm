Optimized C++ implementation of the Smith-Waterman algorithm for local alignment of protein sequences, including multithreading and memory management in O(N).

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
*   `database/`: Target database files.
*   `query/`: Input test protein file (FASTA format).
*   `blosum/`: Text files containing the scoring matrices.
*   `main.cpp`: Main entry point of the program executing the multithreaded version.
*   `Makefile`: Configuration file defining the compilation targets of the project.
*   `INFO_H304_Groupe01.pdf`: Detailed technical report of the project.

## Compilation and Execution

The project uses a `Makefile` to automate compilation. 

```bash
# Compile the project
make

# Execute the program
./[generated_executable_name]
