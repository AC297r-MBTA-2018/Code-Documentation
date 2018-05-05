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

The high-level functionality of this rider segmentation package is to group individual MBTA riders according to pattern-of-use dimensions. The full package has the following structure:

```sh
Rider-Segmentation-Full-App/
    MBTAdashboard/
        __init__.py
        app.py
        src/
            __init__.py
            json_generator_driver.py
            util.py
        static/
            css/
                'custom css files for D3 visualization'
            img/
                'MBTA icon images'
            js/
                'custom javascript files for D3 visualization'
            lib/
                'library files'
        templates/
                'html files'
    MBTAriderSegmentation/
        data/
            cached_clusters/
                hierarchical/
                    'hierarchical clustering .csv results and .json scores'
                non-hierarchical/
                    'non-hierarchical clustering .csv results and .json scores'
            cached_features/
                '.csv rider features by start month'
            cached_profiles/
                '.csv cluster profiles by start month'
            input/
                censue/
                    'census data'
                geojson/
                    'data to draw the maps'
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


Therefore, we developed a flexible, reusable rider segmentation model on MBTA’s local buses and subway system that can group individuals according to their ridership patterns.

## Installation

## Usage
