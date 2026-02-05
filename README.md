# SepWalks.mlx

A MATLAB-based pipeline for clustering 3D spatial genomic coordinates into individual chromosome walks.

## Prerequisites
* **MATLAB** (R2021b or later)
* **Statistics and Machine Learning Toolbox** (Required for `kmeans` and cluster evaluation)
* **Parallel Computing Toolbox** (Recommended for faster processing)

## Inputs
1. **Spatial Data (.csv):** Must contain columns for `x`, `y`, `z`, and `time-point`.
2. **Metadata (.txt):** A file (e.g., `WalkInfo.txt`) with columns defining (chromosome#, start coordinate, stop coordinate, number of oligos, targetname, time-point number) for each Sequential OligoSTORM target

## Usage
1. **Set Paths:** Open the script and update the `folderPath` and `WalkInfo` variables to point to your files.
2. **Choose Clustering Mode:** <br>
   - When prompted in "Separate Walks" section: <br>
     - Set `n = 0` to automatically estimate the number of clusters. <br>
     - Set `n = [Number]` to manually specify the cluster count.
3. **Refine (Optional):** If clusters need manual adjustment, toggle the `MergePcls` or `splitFlag` variables to `1` and follow the prompts. <br>
    - In "Merge Clusters" specify which two Walk-IDs need to merged into one
      - Can merge multiple clusters at the same time
    - In "Split Merged Walks" specify the WalkID needing to be split (k) and how many clusters it should be split into.
4. **Run:** Execute the remainder of the script when satisfied with walk clustering.

## Outputs
* **`SepWalk.mat`:** A full workspace backup of the processed data.
* **Chromosome Folders:** Subfolders named by chromosome (e.g., `Chr1/`, `Chr2/`) containing:
  - **`WalkSeparated.csv`:** The final cleaned and clustered data with an appended "Walk-IDs" column.
