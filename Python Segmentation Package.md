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
                censue/
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

## Installation & Setup

## Usage & Examples
