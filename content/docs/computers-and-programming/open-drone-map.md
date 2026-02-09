# Running Open Drone Map on HiPerGator

Following instructions in [containers](containers):

## Pull the container

Here, we've created a folder (on blue) to store the container image:

```bash
module load aptainer
srun -t 6:00:00 apptainer pull /blue/ewhite/your_user_id/odm/odm.sif docker://opendronemap/odm
```

## Setup directories

Setup directories. ODM needs the working folder code to exist, and a subfolder called images:

```bash
[your_user_id@login12 odm]$ tree
.
├── code
│   ├── images
│   │   ├── DSC00001.JPG
│   │   └── DSC00002.JPG
└── run.slurm
```

We'll create `run.slurm` next:

## Setup SLURM

We need to bind a working directory where the outputs + images will go. You'd think you could just bind to `/code`, which is what ODM would like, but that doesn't work because the EntryPoint of the container is `/code/run.py`. So we need to set our working directory somewhere else. This is analogous to `docker -v`:

```bash
#!/bin/bash
#SBATCH --job-name=odm-node
#SBATCH --nodes=1
#SBATCH --partition=hpg-turin
#SBATCH --cpus-per-task=16
#SBATCH --mem=64GB
#SBATCH --time=12:00:00
#SBATCH --gpus=1
#SBATCH --output=./slurm_logs/%A.out
#SBATCH --error=./slurm_logs/%A.err

printenv | grep -i slurm | sort

srun apptainer run --bind "${PWD}:/project" /blue/ewhite/your_user_id/odm/odm.sif --project-path /project --max-concurrency 16
```

Save this as `run.slurm` and make sure you've added any other SBATCH arguments like the correct user/group IDs.

## Copy data

Copy some drone images and go for a walk while rsync runs:

```bash
rsync -avz --progress /data/everglades/Flight_1/DCIM/  hpg:/home/your_user_id/code/odm/code/images
```

For larger jobs, you should use some scratch storage space on blue. You can delete the images from the working directory afterwards.

## Run

Launch and check the job was submitted:

```bash
sbatch run.slurm
squeuemine
```

## Check logs

Then tail the log while it runs to confirm it's doing something sensible:

```bash
[your_user_id@login12 odm]$ tail -f ./slurm_logs/20526676_4294967294.out
2025-12-08 21:50:58,774 DEBUG: Found 10000 points in 4.388810873031616s
2025-12-08 21:50:59,008 INFO: Extracting ROOT_DSPSIFT features for image DSC00077.JPG
2025-12-08 21:50:59,219 INFO: Extracting ROOT_DSPSIFT features for image DSC00098.JPG
2025-12-08 21:51:04,036 DEBUG: Found 10000 points in 4.76168966293335s
2025-12-08 21:51:04,185 DEBUG: Found 9406 points in 5.121237516403198s
2025-12-08 21:51:04,482 INFO: Extracting ROOT_DSPSIFT features for image DSC00108.JPG
2025-12-08 21:51:04,600 INFO: Extracting ROOT_DSPSIFT features for image DSC00003.JPG
2025-12-08 21:51:08,902 DEBUG: Found 10000 points in 4.365730285644531s
2025-12-08 21:51:09,338 INFO: Extracting ROOT_DSPSIFT features for image DSC00092.JPG
2025-12-08 21:51:09,627 DEBUG: Found 9079 points in 4.97150993347168s
2025-12-08 21:51:10,010 INFO: Extracting ROOT_DSPSIFT features for image DSC00141.JPG
....

[INFO]    No more stages to run
[INFO]    MMMMMMMMMMMNNNMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMNNNMMMMMMMMMMM
[INFO]    MMMMMMdo:..---../sNMMMMMMMMMMMMMMMMMMMMMMMMMMNs/..---..:odMMMMMM
[INFO]    MMMMy-.odNMMMMMNy/`/mMMMMMMMMMMMMMMMMMMMMMMm/`/hNMMMMMNdo.-yMMMM
[INFO]    MMN/`sMMMMMMMMMNNMm/`yMMMMMMMMMMMMMMMMMMMMy`/mMNNMMMMMMMMNs`/MMM
[INFO]    MM/ hMMMMMMMMNs.+MMM/ dMMMMMMMMMMMMMMMMMMh +MMM+.sNMMMMMMMMh +MM
[INFO]    MN /MMMMMMNo/./mMMMMN :MMMMMMMMMMMMMMMMMM: NMMMMm/./oNMMMMMM: NM
[INFO]    Mm +MMMMMN+ `/MMMMMMM`-MMMMMMMMMMMMMMMMMM-`MMMMMMM:` oNMMMMM+ mM
[INFO]    MM..NMMNs./mNMMMMMMMy sMMMMMMMMMMMMMMMMMMo hMMMMMMMNm/.sNMMN`-MM
[INFO]    MMd`:mMNomMMMMMMMMMy`:MMMMMMMNmmmmNMMMMMMN:`hMMMMMMMMMdoNMm-`dMM
[INFO]    MMMm:.omMMMMMMMMNh/  sdmmho/.`..`-``-/sddh+  /hNMMMMMMMMdo.:mMMM
[INFO]    MMMMMd+--/osss+:-:/`  ```:- .ym+ hmo``:-`   `+:-:ossso/-:+dMMMMM
[INFO]    MMMMMMMNmhysosydmNMo   /ds`/NMM+ hMMd..dh.  sMNmdysosyhmNMMMMMMM
[INFO]    MMMMMMMMMMMMMMMMMMMs .:-:``hmmN+ yNmds -:.:`-NMMMMMMMMMMMMMMMMMM
[INFO]    MMMMMMMMMMMMMMMMMMN.-mNm- //:::. -:://: +mMd`-NMMMMMMMMMMMMMMMMM
[INFO]    MMMMMMMMMMMMMMMMMM+ dMMN -MMNNN+ yNNNMN :MMMs sMMMMMMMMMMMMMMMMM
[INFO]    MMMMMMMMMMMMMMMMMM`.mmmy /mmmmm/ smmmmm``mmmh :MMMMMMMMMMMMMMMMM
[INFO]    MMMMMMMMMMMMMMMMMM``:::- ./////. -:::::` :::: -MMMMMMMMMMMMMMMMM
[INFO]    MMMMMMMMMMMMMMMMMM:`mNNd /NNNNN+ hNNNNN .NNNy +MMMMMMMMMMMMMMMMM
[INFO]    MMMMMMMMMMMMMMMMMMd`/MMM.`ys+//. -/+oso +MMN.`mMMMMMMMMMMMMMMMMM
[INFO]    MMMMMMMMMMMMMMMMMMMy /o:- `oyhd/ shys+ `-:s-`hMMMMMMMMMMMMMMMMMM
[INFO]    MMMMMMMMNmdhhhdmNMMM`  +d+ sMMM+ hMMN:`hh-  sMMNmdhhhdmNMMMMMMMM
[INFO]    MMMMMms:::/++//::+ho    .+- /dM+ hNh- +/`   -h+:://++/::/smMMMMM
[INFO]    MMMN+./hmMMMMMMNds-  ./oso:.``:. :-``.:os+-  -sdNMMMMMMmy:.oNMMM
[INFO]    MMm-.hMNhNMMMMMMMMNo`/MMMMMNdhyyyyhhdNMMMM+`oNMMMMMMMMNhNMh.-mMM
[INFO]    MM:`mMMN/-sNNMMMMMMMo yMMMMMMMMMMMMMMMMMMy sMMMMMMMNNs-/NMMm`:MM
[INFO]    Mm /MMMMMd/.-oMMMMMMN :MMMMMMMMMMMMMMMMMM-`MMMMMMMo-./dMMMMM/ NM
[INFO]    Mm /MMMMMMm:-`sNMMMMN :MMMMMMMMMMMMMMMMMM-`MMMMMNs`-/NMMMMMM/ NM
[INFO]    MM:`mMMMMMMMMd/-sMMMo yMMMMMMMMMMMMMMMMMMy sMMMs-/dMMMMMMMMd`:MM
[INFO]    MMm-.hMMMMMMMMMdhMNo`+MMMMMMMMMMMMMMMMMMMM+`oNMhdMMMMMMMMMh.-mMM
[INFO]    MMMNo./hmNMMMMMNms--yMMMMMMMMMMMMMMMMMMMMMMy--smNMMMMMNmy/.oNMMM
[INFO]    MMMMMms:-:/+++/:-+hMMMMMMMMMMMMMMMMMMMMMMMMMNh+-:/+++/:-:smMMMMM
[INFO]    MMMMMMMMNdhhyhdmMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMmdhyhhmNMMMMMMMM
[INFO]    MMMMMMMMMMMMMMMNNNNNMMMMMMNNNNNNMMMMMMMMNNMMMMMMMNNMMMMMMMMMMMMM
[INFO]    MMMMMMMMMMMMMh/-...-+dMMMm......:+hMMMMs../MMMMMo..sMMMMMMMMMMMM
[INFO]    MMMMMMMMMMMM/  /yhy-  sMMm  -hhy/  :NMM+   oMMMy   /MMMMMMMMMMMM
[INFO]    MMMMMMMMMMMy  /MMMMN`  NMm  /MMMMo  +MM: .` yMd``` :MMMMMMMMMMMM
[INFO]    MMMMMMMMMMM+  sMMMMM:  hMm  /MMMMd  -MM- /s `h.`d- -MMMMMMMMMMMM
[INFO]    MMMMMMMMMMMs  +MMMMM.  mMm  /MMMMy  /MM. +M/   yM: `MMMMMMMMMMMM
[INFO]    MMMMMMMMMMMN-  smNm/  +MMm  :NNdo` .mMM` oMM+/yMM/  MMMMMMMMMMMM
[INFO]    MMMMMMMMMMMMNo-    `:yMMMm      `:sNMMM` sMMMMMMM+  NMMMMMMMMMMM
[INFO]    MMMMMMMMMMMMMMMNmmNMMMMMMMNmmmmNMMMMMMMNNMMMMMMMMMNNMMMMMMMMMMMM
[INFO]    ODM app finished - Mon Dec 08 22:13:26  2025
.100 - done.
(ASCII art is fun)
```

The output will be in `odm_orthophoto/odm_orthophoto.tif`

## Useful flags and other run options

- Store high-precision geotags from PPK in `geo.txt` in the project folder and ODM will pick them up automatically (see [here](https://docs.opendronemap.org/geo/))

```python
"""Convert WISPR PPK coords to OpenDroneMap"""

import argparse
import csv
from pathlib import Path

parser = argparse.ArgumentParser(description="Convert WISPR CSV to ODM geo.txt")
parser.add_argument("root_dir", help="Root directory to search for exif_image_list.csv")
parser.add_argument("output_txt", help="Output geo.txt file for ODM")
args = parser.parse_args()

csv_files = list(Path(args.root_dir).rglob("exif_image_list.csv"))
if len(csv_files) == 0:
    raise SystemExit("Error: No exif_image_list.csv found")
if len(csv_files) > 1:
    raise SystemExit(f"Error: Found multiple exif_image_list.csv files:\n" + "\n".join(str(f) for f in csv_files))

with open(csv_files[0]) as infile, open(args.output_txt, "w") as outfile:
    reader = csv.DictReader(infile)
    outfile.write("EPSG:4326\n")

    for row in reader:
        # ODM format: filename lon lat alt yaw pitch roll [h_acc v_acc]
        outfile.write(
            f"{row['image_name']} {row['longitude']} {row['latitude']} "
            f"{row['altitude']} {row['gimbal_yaw']} {row['gimbal_pitch']} "
            f"{row['gimbal_roll']} {row['x_accuracy']} {row['z_accuracy']}\n"
        )

print(f"Created {args.output_txt}")
```

- Set `--max-concurrency` to the number of CPUs assigned to the job
- `--optimize-disk-space` to clean up intermediate files at the cost of needing to re-run the job if it crashes (no resume)
- `--orthophoto-resolution 2` ortho resolution in centimeters. The lower this parameter, the larger the output ortho will be and the slower the processing. The default is `5` which runs fairly quickly and is a good sanity check.
- Use [GCPs](https://docs.opendronemap.org/gcp/)

## Array processing.

Here is an example job file that can run on an array of surveys. The list of input folders is provided in `folder_list.txt` (newline delineated).

```bash
#!/bin/bash
#SBATCH --job-name=odm-array
#SBATCH --nodes=1
#SBATCH --partition=hpg-turin
#SBATCH --cpus-per-task=16
#SBATCH --mem=64GB
#SBATCH --time=12:00:00
#SBATCH --gpus=1
#SBATCH --array=1-N%4
#SBATCH --output=./slurm_logs/%A_%a.out
#SBATCH --error=./slurm_logs/%A_%a.err

# Configuration
INPUT_FILE="folder_list.txt"  # Text file with one absolute path per line
WORKING_DIR= # Where you want to create the output folders
ODM_SIF=# Path to the SIF file

# Get the source folder for this array task
SOURCE_FOLDER=$(sed -n "${SLURM_ARRAY_TASK_ID}p" "$INPUT_FILE")

# Get the basename
BASENAME=$(basename "$SOURCE_FOLDER")

# Create target directory structure
TARGET_DIR="${WORKING_DIR}/${BASENAME}"

# Print environment for debugging
printenv | grep -i slurm | sort

mkdir -p "${TARGET_DIR}/code/images"

# Copy JPG files
echo "Copying images from ${SOURCE_FOLDER} to ${TARGET_DIR}/code/images"
rsync -av --include='*.JPG' --include='*.jpg' --exclude='*' "${SOURCE_FOLDER}/" "${TARGET_DIR}/code/images/" || \
echo "Warning: No JPG files found in ${SOURCE_FOLDER}"

# Perform PPK geotagging
python3 wispr_to_odm_ppk.py ${SOURCE_FOLDER} ${TARGET_DIR}/geo.txt || \
echo "Failed to find a PPK coordinate file. Processing will use EXIF GPS data only."

module load cuda

# Run ODM with the target directory as project path
echo "Running ODM on ${TARGET_DIR}"
srun apptainer run --nv --bind "${TARGET_DIR}:/project" \
    "$ODM_SIF" \
    --project-path /project \
    --max-concurrency 16 \
    --orthophoto-resolution 2 \
    --optimize-disk-space

# Clean up the images in the target folder
echo "Removing image folder ${TARGET_DIR}/code/images"
rm -rf "${TARGET_DIR}/code/images"

echo "Setting permissions on target foler"
bash group-permissions-update.sh ${TARGET_DIR}

echo "Completed processing ${BASENAME}"
```

- The param `#SBATCH --array=1-N%4` starts an array job with a maximum of 4 concurrent tasks.
- Note `module load cuda` to pass GPU to job

Submission script:

```bash
#!/bin/bash

INPUT_FILE="folder_list.txt"
N=$(wc -l < "$INPUT_FILE")

sbatch --array=1-$N job_array.slurm
```
