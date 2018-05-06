---
nav_include: 1
title: Python Segmentation Package
---

## Contents
{:.no_toc}
*  
{: toc}

## Introduction & Package Structure
The Massachusetts Bay Transportation Authority (MBTA) is the largest public transportation agency in New England, delivering a complex system of subway, bus, commuter rail, light rail, and ferry services to riders in the dynamic economy of the Greater Boston Area. It is estimated that MBTA provides over 1.3 million trips on an average weekday. While MBTA collects a wealth of trip transaction data on a daily basis, a persistent limitation has been the organization’s lack of knowledge around rider groups and their respective ridership habits. Understanding rider segmentation in the context of pattern-of-use has significant implications in developing new policies to improve its service planning and potentially changing its fare structure.

The high-level functionality of this rider segmentation package is to group individual MBTA riders according to pattern-of-use dimensions. The Python segmentation package has the following structure:

```sh
Rider-Segmentation-Full-App/
    MBTAdashboard/
    MBTAriderSegmentation/
        data/
            cached_clusters/
                hierarchical/
                non-hierarchical/
            cached_features/
            cached_profiles/
            input/
                census/
                geojson/
            report_models/
                report_cnn.h5
        __init__.py
        config.py
        features.py
        profile.py
        report.py
        segmentation.py
        visualization.py
```

A brief description of each item:

- **`data/` directory** contains the input data files (i.e., US census data, geojson data which is used to draw maps) and output result files (i.e. extracted feature in `cached_features`, segmentation results in `cached_clusters`, profiled cluster summaries in `cached_profiles` and a trained CNN model to classify temporal patterns in `report_models`) .

- **`config.py` module** defines global constants (e.g., data path, file prefix, rider type dictionary) that are used in the other modules.

- **`feature.py` module** extracts different sets of rider-level features ([details](https://ac297r-mbta-2018.github.io/Final-Report/feature.html)) from transaction-level data and save the extracted features in the `data/cached_features/` directory.

- **`segmentation.py` module** implements the K-means and LDA clustering algorithm and save the rider features with its cluster assignment in the `data/cached_clusters/` directory.

- **`profile.py` module** aggregated the rider features by cluster assignments and generates a text summary that describes the cluster size, average number of trips, predicted rider type (by using the report.py module), weekday / weekend hours and the zip code associated with the most traffic for each cluster. The profiled clusters are saved in the `data/cached_profiles/` directory.

- **`report.py` module** is called by `profile.py` to format the generated cluster description for each cluster and append this column of text to the profiled cluster data frame.

- **`visualization.py` module** implements all types of visualization graphs in python.

## Installation & Setup

- Step 1: Download code from our [github](https://github.com/AC297r-MBTA-2018/Rider-Segmentation-Full-App)
> ```
> git clone https://github.com/AC297r-MBTA-2018/Rider-Segmentation-Full-App.git
> ```

- Step 2: Obtain AFC, ODX, stops and fare product data from MBTA
> **Note**: We did not publish this data for security reasons.

- Step 3: Preprocess data to form clean transaction records
> - Merge AFC, ODX and fare product data
> - Map each stop's (longitude, latitude) to zip code using Google Geoencoding API
> - Filter out invalid transaction records (associated with MBTA employers or voided)

> **Note**: We did not propose a fixed pipeline with specific codes for this step as these procedures need to be adapted depending on the obtained data's condition.


## Usage & Examples

### Feature Extraction

In the same directory level as MBTAriderSegmentation, run the following code:

```python
import time
from MBTAriderSegmentation.config import *
from MBTAriderSegmentation.features import FeatureExtractor

import time
t0 = time.time()
start_month='1710'
duration=1
extractor = FeatureExtractor(start_month=start_month, duration=duration).extract_features()
print("feature extraction time: ", time.time() - t0)
```
### Rider Segmentation

In the same directory level as MBTAriderSegmentation, run the following code:

```python
import time
from MBTAriderSegmentation.config import *
from MBTAriderSegmentation.segmentation import Segmentation

# Hierarchical pipeline
import time
t0 = time.time()
start_month='1710'
duration=1
segmentation = Segmentation(start_month=start_month, duration=duration)
segmentation.get_rider_segmentation(hierarchical=True)
print("Hierarchical clustering time: ", time.time() - t0)

# Non-hierarchical pipeline
t0 = time.time()
start_month='1710'
duration=1
segmentation = Segmentation(start_month=start_month, duration=duration)
segmentation.get_rider_segmentation(hierarchical=False)
print("Non-hierarchical clustering time: ", time.time() - t0)
```
### Cluster Inference

In the same directory level as MBTAriderSegmentation, run the following code:

```python
import time
from MBTAriderSegmentation.config import *
from MBTAriderSegmentation.profile import ClusterProfiler

# by_cluster = True
start_month = '1710'
duration=1
hier_flags = [True, False]
algo_types = ['kmeans', 'lda']
# by_cluster = True
for hier_flag in hier_flags:
    for algo_type in algo_types:
        t0 = time.time()
        profiler = ClusterProfiler(start_month=start_month, duration=duration, hierarchical=hier_flag)
        profiler.extract_profile(algorithm=algo_type, by_cluster=True)
        print("[by_cluster = True] Profile time: ", time.time() - t0)

# by_cluster = False
t0 = time.time()
profiler = ClusterProfiler(start_month=start_month, duration=duration, hierarchical=True)
profiler.extract_profile(algorithm='kmeans', by_cluster=False)
print("[by_cluster = False] Profile time: ", time.time() - t0)

```

### Visualization

In the same directory level as MBTAriderSegmentation, run the following code:

```python
import time
from MBTAriderSegmentation.config import *
from MBTAriderSegmentation.visualization import Visualization

# load data
start_month='1710'
duration=1
viz = Visualization(start_month=start_month, duration=duration)
viz.load_data(by_cluster=True, hierarchical=True, w_time=None, algorithm='lda')

# plot cluster_size, avg_num_trips
viz.plot_cluster_size()
viz.plot_avg_num_trips()

# PCA plot
viz.visualize_clusters_2d()

# plot temporal patterns
viz.plot_all_hourly_patterns()

# generate geographical patterns as html files
unique_clusters = list(viz.df['cluster'].unique())
map_urls = []
for cluster in unique_clusters:
    map_urls.append(viz.plot_cluster_geo_pattern(cluster))

# plot demographics distribution
viz.plot_demographics(grp='race', stacked=True)
viz.plot_demographics(grp='edu', stacked=False)

```
