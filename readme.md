## FengQing

FengQing is a data-driven model for medium-range weather forecasting jointly developed by the China Meteorological Administration and School of Software at Tsinghua University. It leverages advanced machine-learning techniques to enhance weather forecasting and analysis for over 10 days. The runnable model and inference code are fully open-sourced, allowing users to set up and perform their weather predictions efficiently.

## Data and Model Download

To begin working on this project, you need to download the sample input data and pre-trained models from this link, which is organized according to the following directory structure:

```yaml
project_root/
├── data/
│   ├── YYYY
│   	├── MMDD
│   		├── pressure_060000.npy
│   		├── pressure_120000.npy
│   		├── surface_060000.npy
│   		├── surface_120000.npy
├── mean_std/
│   ├── upper_mean.npy
│   ├── surface_mean.npy
│   ├── upper_std.npy
│   ├── surface_std.npy
│   ├── res_upper_std.npy
│   ├── res_surface_std.npy
├── utils/
│   ├── constant_masks.npy
│   ├── cfg_15days.pt
├── onnx/
│   ├── weights.pb
│   ├── fengqing.onnx
```

You can customize the input data by modifying functions in `get_data.py`. To ensure compatibility, please keep the exact input variable order and units as specified in the [details of data](#Details of Data).

## Quick Start

Once downloading the model and sample data, follow these steps to install the necessary environment and run the inference script.

1. Install dependencies

   ```python
   pip install -r requirements.txt
   ```

2. Run Inference (start time corresponds to the last input frame)

    ```bash
    python inference_fengqing.py --dataset-path /project_root/data --datetime start_time --output_dir /path/to/output
    ```

## Details of Data

The model takes in two main data types: upper-level and surface data. Each has its own shape and order, and any customized data should be preprocessed in the following format.

* upper-level data: [variable, level, lat, lon] rearrange to [variable * level, lat, lon]

|   upper variable    | abbreviation |            unit             |
| :-----------------: | :----------: | :-------------------------: |
|    Geopotential     |      Z       |   $\text{m}^2/\text{s}^2$   |
|  Specific humidity  |      Q       | $\text{kg}\ \text{kg}^{-1}$ |
|     Temperature     |      T       |         $\text{K}$          |
| U component of wind |      U       |        $\text{m/s}$         |
| V component of wind |      V       |        $\text{m/s}$         |

The pressure levels are arranged in the following sequence:

1000 hPa, 925 hPa, 850 hPa, 700 hPa, 600 hPa, 500 hPa, 400 hPa, 300 hPa, 250 hPa, 200 hPa, 150 hPa, 100 hPa, 50 hPa.

* Surface data: [variable, lat, lon]

|     surface variable      | abbreviation |     unit     |
| :-----------------------: | :----------: | :----------: |
|  Mean sea level pressure  |     MSLP     | $\text{Pa}$  |
| 10 metre U wind component |     U10      | $\text{m/s}$ |
| 10 metre V wind component |     V10      | $\text{m/s}$ |
|    2 metre temperature    |     T2M      |  $\text{K}$  |
