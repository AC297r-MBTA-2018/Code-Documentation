---
title: MBTA Rider Segmentation
---

## Contents
{:.no_toc}
*  
{: toc}

## Team
Harvard 2018 Spring AC297r Capstone Project: Chia Chi (Michelle) Ho, Yijun Shen, Jiawen Tong, Anthony Hou

## Project Deliverables
This repo contains code documentation for both of our deliverables:
- Python Rider Segmentation Package
- Web-based Visualization Exploration Tool

| <img src="img/project_deliverables.png" width="1000">|
|:--:|
| ***Figure 1: Project Deliverables*** |

## Code Package Structure

The full package has the following structure:

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
