---
nav_include: 2
title: Model Class Documentation
---

## Contents
{:.no_toc}
*  
{: toc}

## Feature Extraction

The ```feature.py``` module allows you to extract rider-level pattern-of-use features from the MBTA transaction-level data.

### Class ```DataLoader```

A ```DataLoader``` object first merges the joint AFC_ODX table, the stops table and fare product tables to form transaction records. The preprocessed transaction records are then passed to a ```FeatureExtractor``` object to extract the rider-level pattern-of-use features.

- **Attributes**:
  - start_month: a string representing the start month in the format of YYMM, e.g. '1710'
  - duration: an integer representing the length of duration
  - afc_odx_fields: a list of fields used to read the joint AFC_ODX table, ['deviceclassid', 'trxtime', 'tickettypeid', 'card', 'origin', 'movementtype']
  - fp_field: a list of fields used to read the fare product table, ['tariff', 'servicebrand', 'usertype', 'tickettypeid', 'zonecr']
  - stops_fields: a list of fields used to read the stops table, ['stop_id', 'zipcode']
  - fareprod: a DataFrame of fare product records

  - stops: a DataFrame of stop records
  - station_deviceclassid: a list of interested device class ids, [411, 412, 441, 442, 443, 501, 503]
  - validation_movementtype: a list of validation movement types, [7, 20]

- **Methods**:
  - __init__(self, start_month, duration):
  - load(self):

### Class ```FeatureExtractor```

## Rider Segmentation

## Cluster Inference

## Visualization

## Auto Report Generator
