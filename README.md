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

In each configuration file one must set:
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

We can also manually call each script, but doing so requires to specify the checkpoint for the refinement step in the configuration files
```bash
python main_convex.py ./config_calibrate_unknown.yaml
python main_convex.py ./config_convex_unknown.yaml
```

## Saved Configuratio files
To simplify the work and avoid running the search of hyperparameters we include our configuration files for each unknown objects in [configs_unknown directory](./configs_unknown/).

## Project summary
This project performs a **convex reconstruction** where an initial mash (an ellipsoid as default) is adapted to the lightcurves of an object unknown object trought a forward operator that emulates challenge experimental setup.


[INSERT HERE SOME LINES ABOUT THE MODEL]

For more details on model architectures and training process see [train_convex.py](./train_convex.py). \
**Note:** The code use pytorch with device set to "cuda:0". Change to "cuda:1" to allow the usage of GPU, if available.

