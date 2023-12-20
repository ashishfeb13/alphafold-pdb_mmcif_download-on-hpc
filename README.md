## AlphaFold pdb_mmcif_download-on-hpc
## Get in Touch
- Email: ashishfeb13@gmail.com, akj4@nyu.edu
------------------------------------------------------

alphafold pdb_mmcif_download on hpc without error

1# Slurm Script for Downloading MMCIF Data
## Overview
This script uses Slurm directives to download mmCIF data from rsync mirrors. It fetches files from rsync.rcsb.org and saves them to a specified directory.

## Steps:
1. **Set Up**:
   - Ensure the Slurm environment is available.
   - Modify the script according to the destination directory where files will be downloaded (`/path/to-download-pdb_mmcif-data/`).

2. **Script Execution**:
   - Submit the script to Slurm using `sbatch script.sh` (replace `script.sh` with your actual script filename).

3. **Monitoring**:
   - Check the logs (`Job_%A_%a.out` and `Job_%A_%a.err`) for download progress and errors.
   - Review the `log.txt` file for download start and finish times.

## Notes:
- Update the destination directory (`/path/to-download-pdb_mmcif-data/`) before execution.
- Check Slurm documentation for any specific configurations required.
########### script ##############
#!/bin/bash

####################################################
# Slurm script to download mmCIF data from mirrors #
# Script by: Ashish Jaiswal                        #
# Contact: akj4@nyu.edu                            #
####################################################

#SBATCH -o Job_%A_%a.out
#SBATCH -e Job_%A_%a.err
#SBATCH -n 1
#SBATCH --cpus-per-task=12
#SBATCH --mem=2GB
#SBATCH --time=168:00:00  ## time for 7days 168:00:00
#SBATCH --job-name=alphafold
#SBATCH --output=alphafold-download
#SBATCH --mail-type=END
#SBATCH --mail-user=

# Log start time in log.txt
echo "-- Started pdb_mmcif files download at $(date)" > log.txt

# Downloading mmcif files from rsync mirrors
rsync --bwlimit=1000 --timeout=600 --recursive --links --perms --times --compress --info=progress2 --delete --port=33444 rsync.rcsb.org::ftp_data/structures/divided/mmCIF/ /path/to-download-pdb_mmcif-data/
##	* ftp.pdbj.org::ftp_data/structures/divided/mmCIF/ (Asia)

# Log end time in log.txt
echo "-- Finished pdb_mmcif files download at $(date)" >> log.txt

##########################################################################################
##	rsync options
##	Running rsync to fetch all mmCIF files (note that the rsync progress estimate might be inaccurate)...
##	If the download speed is too slow, try changing the mirror to:
##	* rsync.ebi.ac.uk::pub/databases/pdb/data/structures/divided/mmCIF/ (Europe)
##	* ftp.pdbj.org::ftp_data/structures/divided/mmCIF/ (Asia)
##	or see https://www.wwpdb.org/ftp/pdb-ftp-sites for more download options.
##	All source args must come from the same machine.
##########################################################################################

