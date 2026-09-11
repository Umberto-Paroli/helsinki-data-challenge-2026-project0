# Helsinki Data Challenge 2026 - Team UNIMORE - Solution 0

## Installation
It is suggested the use of a virtual environment or a container to manage project dependencies.
```bash
pip install --upgrade pip
pip install -r requirements.txt
```
## Data
Before run the code add asteroids lightcurves and .stl of the public objects in a **./Data** directory. Real videos and blender renderings aren't required. 

## Usage
Running [search.py](./search.py) find a best hyperparameter configuration for the three public objects available from the data challenge. \
The resulting values has to be replicated in configuration files for each unknown objects but changing the light curve file path adn the cylinder_radius which are different for every objects. \
The configuration files present in the repository are obtained by running:
```bash
python search.py --phase convex --hours 24
```
Note: search.py is shared two solutions, in this repository option "--phase refine" doesn't work, don't use it.

If data directory is different it also has to be chaged in configuration files
```YAML
  data:
    intensity_file: <LC_INTENSITY_PATH>
    binary_file: <LC_INTENSITY_BINARY>
    intensity_file_blender: <LC_BLENDER_INTENSITY_PATH> # only if this object has published Blender data
    binary_file_blender: <LC_BLENDER_BINARY_PATH>
```
Change also output directory if required
```YAML
  output:
    base_dir: ./runs_unknown/[N] #if desired
```

When the configuration files are ready we can run the two stage of the process in sequence:
```bash
python ./run_unknown_object.py
```

## Saved Configuratio files
To simplify the work and avoid running the search of hyperparameters we include our configuration files for each unknown objects in [configs_unknown directory](./configs_unknown/).

## Project summary
This project performs a **convex reconstruction** where an initial mash (an ellipsoid as default) is adapted to the lightcurves of an unknown object trought a forward operator that emulates the challenge experimental setup.

For more details on model architectures and training process see [train_convex.py](./train_convex.py). and [train.py](./train.py) \
**Note:** The code use pytorch with device set to "cuda:0" and it may cause a crash. Change to "cuda:1" to allow the usage of GPU, if available.

