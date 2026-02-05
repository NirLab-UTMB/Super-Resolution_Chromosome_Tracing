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


--- 



# seqOSTORM.mlx

A MATLAB pipeline for analyzing 3D chromosome tracing data, specialized for circular DNA (e.g., Salmonella), drift correction, and parabolic fitting of genomic structures.

## Prerequisites
* **MATLAB R2021b or later**
* **Required Toolboxes:**
  * Bioinformatics Toolbox (for `fastaread`)
  * Statistics and Machine Learning Toolbox (for `pdist` and `squareform`)
* **Required Custom Functions:** <br> Ensure `wrcmap.mlx`, `squareform_nostats.mlx`, and `findParabolicRingTraces.mlx` are in your MATLAB path.

## Inputs
* **PathToData.txt:** A comma-delimited text file listing the full paths to all spatial `.csv` files to be analyzed. (All files must be first run through SepWalks.mlx to generate `Walk-IDs` column)
* **Metadata (.txt):** A file (e.g., `WalkInfo.txt`) with columns defining (chromosome#, start coordinate, stop coordinate, number of oligos, targetname, time-point number) for each Sequential OligoSTORM target
* **Genome.fa:** The FASTA file of the imaged genome build.

## Usage
1. **Configure Settings:** Open the script and update the "Settings" section:
    * `folderPath`: Where results will be saved.
    * `circDNA`: Set to `1` for circular DNA (rings) or `0` for linear.
    * `SameTargetFlag`: Set to `1` if you have repeated imaging steps for drift correction.
         - specify which time-points are the repeated imaging targets
    * `disHmapColorLim`: Set colorbar limits for center-to-center distance heatmaps
2. **Run Script:** Execute in MATLAB.
3. **Interactive Prompts:**
    * **Append Data:** The script will ask if you want to add additional datasets. Use `y` to select a text file with more paths.
4. **Drift Filtering:** Adjust the `maxDrift` variable (default 200nm) to filter walks based on re-imaging precision.

## Outputs
* **Heatmaps**
    * Pair-wise Center-to-Center Euclidean distances.
* **Repeat Imaging Step data**
    * For analysis of drift between re-imaging steps
* **Data Files:**
    * `AnalyzedData.mat`: Full workspace save (excluding UI figures).
    * `Data.txt`: Concatenated and cleaned spatial data for all walks.
* **ParabolaDiagnostics/:**
    * A subfolder containing fitting plots for circular DNA traces.
