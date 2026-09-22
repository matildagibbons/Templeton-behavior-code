# Templeton Behavior Code

Code for running and analysing a head-fixed mouse behavioural task in which mice learn to respond to a rewarded **visual** or **auditory** stimulus. It includes:

- **Task GUI**: a point-and-click launcher for training sessions
- **Analysis GUI**: plots a mouse's performance from its latest session and across all its sessions
- **RF mapping GUI**: runs receptive-field mapping
- **Running speed notebook**: looks at wheel running around trials

Sessions are run with [PsychoPy](https://www.psychopy.org/). Hardware (water valve, rotary encoder) is controlled through a National Instruments DAQ, and each session is saved as an `.hdf5` file.

> The task code (`DynamicRouting1.py`, `TaskControl.py`, `TaskUtils.py`, `RFMapping.py`) is adapted from the Allen Institute's Dynamic Routing task code by Sam Gale. The Templeton training stages, GUIs and analysis were added for this project.

---

## Repository structure

```
Templeton-behavior-code/
├── Task GUI/
│   ├── Behavior_task_GUI.py        # Launcher: pick mouse, modality and stage
│   ├── DynamicRouting1.py          # Task logic, including the Templeton stages
│   ├── TaskControl.py              # Base class: display, sound, DAQ, saving
│   ├── TaskUtils.py                # Helper functions
│   └── taskParams_templeton_stage_*_{vis,aud}.json   # One parameter file per stage
├── Analysis GUI/
│   ├── Behavior_analysis_GUI.py    # Pick a mouse, run analysis, show plots
│   └── Behavior_analysis.py        # Loads newest session and makes plots
├── RFmapping GUI/
│   ├── RFMapping_GUI.py            # Set subject/trials and start RF mapping
│   └── RFMapping.py                # RF mapping task
└── Running speed notebook.ipynb    # Wheel running speed aligned to trials
```

---

## Training stages

Each stage exists in a **visual** (`vis`) and an **auditory** (`aud`) version. The rewarded stimulus is `vis1` for visual mice and `sound1` for auditory mice.

| Stage | Stimuli | What's new |
|-------|---------|------------|
| **0** | Target and non-target in one modality | Every go trial is auto-rewarded; 150 trials; no catch trials |
| **1** | Same as stage 0 | Rewards must be earned; wrong responses get a 3 s timeout (plus a noise burst for auditory) and the trial is repeated up to 3 times |
| **2** | Stimuli from **both** modalities | Only the trained modality's target is rewarded; stimulus durations and pre-stimulus delays vary |

Stage settings are defined under `templeton` in `Task GUI/DynamicRouting1.py`.

---

## Setup

The rig code is written for **Windows** with NI-DAQmx drivers installed.

1. **Create the environment** (Python 3.10 recommended for PsychoPy):

```bash
   conda create -p ./.conda python=3.10
   conda activate ./.conda
   pip install psychopy psychtoolbox nidaqmx pyserial h5py numpy scipy pandas matplotlib pillow freetype-py
```
NB: Or use 'Behaviorenvironment' if on teenspirit

2. **Update file paths.** Several paths are hard-coded to the rig computer (`C:\Users\teenspirit\Desktop\Behavior\Tilda\...`). If you move the code, update them in:
   - `Task GUI/Behavior_task_GUI.py`: `script_path`, `params_file`, `save_dir`, and the folder in `get_task_versions`
   - `Analysis GUI/Behavior_analysis_GUI.py`: data folder and `Saved graphs` folder
   - `RFmapping GUI/RFMapping_GUI.py`: `save_path`

---

## Usage

### Running a session (see lab notebook for details)

```bash
python "Task GUI/Behavior_task_GUI.py"
```

1. Enter the **mouse number**.
2. Click **Visual** or **Auditory**, then pick a stage.
3. Use **Give water droplet** to deliver a manual reward at any time.

Data is saved to `...\Tilda\Data\<mouse>\<mouse>_<Vis|Aud>_<stage>_<YYYYMMDD_HHMMSS>.hdf5`.

### Analysing a session

```bash
python "Analysis GUI/Behavior_analysis_GUI.py"
```

1. Enter the **mouse name**.
2. Click **Run Analysis**. This finds the mouse's newest `.hdf5` file and generates the plots.
3. Click **Display Plots** to view them. They're also saved to `...\Tilda\Data\Saved graphs\<mouse>\`.

### RF mapping

```bash
python "RFmapping GUI/RFMapping_GUI.py"
```

Set the subject name, max frames/trials/blocks, then click **Start Task**.


---

## Author

Matilda Gibbons
