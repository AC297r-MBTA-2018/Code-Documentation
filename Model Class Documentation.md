---
nav_include: 1
title: Module Class Documentation
---

## Contents
{:.no_toc}
*  
{: toc}

## Feature Extraction

The ```feature.py``` module allows you to extract rider-level pattern-of-use features from the MBTA transaction-level data.

### Class ```DataLoader```

A ```DataLoader``` object first merges the joint AFC_ODX table, the stops table and fare product tables to form transaction records. The preprocessed transaction records are then passed to a ```FeatureExtractor``` object to extract the rider-level pattern-of-use features.

Note: A ```DataLoader``` object is initialized by a ```FeatureExtractor``` object and not explicited used elsewhere in our project.

- **Attributes**:
  - ```start_month```: a string representing the start month in the format of YYMM, e.g. '1710'
  - ```duration```: an integer representing the length of duration
  - ```afc_odx_fields```: a list of fields used to read the joint AFC_ODX table, ['deviceclassid', 'trxtime', 'tickettypeid', 'card', 'origin', 'movementtype']
  - ```fp_field```: a list of fields used to read the fare product table, ['tariff', 'servicebrand', 'usertype', 'tickettypeid', 'zonecr']
  - ```stops_fields```: a list of fields used to read the stops table, ['stop_id', 'zipcode']
  - ```fareprod```: a DataFrame of fare product records
  - ```stops```: a DataFrame of stop records
  - ```station_deviceclassid```: a list of interested device class ids, [411, 412, 441, 442, 443, 501, 503]
  - ```validation_movementtype```: a list of interested validation movement types, [7, 20]
  - ```df```: a DataFrame of preprocessed transaction records

- **Methods**:
  - ```__init__(self, start_month, duration)```:
    - initialize the attributes
    - read in the stops and the fare product table as DataFrame

  - ```load(self)```:
    - read in the joint AFC_ODX table with the specified ```start_month``` and ```duration``` as a DataFrame
    - merge joint AFC_ODX, stops and fare product table and select rows with the interested ```station_deviceclassid``` and ```validation_movementtype```
    - save the preprocessed transaction records as ```self.df```

### Class ```FeatureExtractor```

A ```FeatureExtractor``` object extracts the rider-level temporal, geographical, and ticket-purchasing features based on the preprocessed transaction records returned by the ```DataLoader```
  Label riders by their total number of trips, and whether they use commuter rail expect for zone 1a
  The second step is for further filtering in segmentaion model.

- **Attributes**:
  - start_month: a string representing the start month in the format of YYMM, e.g. '1710'
  - duration: an integer representing the length of duration
  - df_transaction: a DataFrame of preprocessed transaction records returned by a ```DataLoader``` object
  - purchase_features: a list of ticket-purchasing types, ['tariff', 'usertype', 'servicebrand', 'zonecr']

- **Methods**:
  - ```__init__(self, start_month='1701', duration=1)```:
    - initialize the attributes
    - get the preprocessed transaction DataFrame by initializing a ```DataLoader``` object

  - ```_extract_temporal_patterns(self)```: For each ```riderID```,
    - extract the [168 hourly temporal patterns](https://ac297r-mbta-2018.github.io/Final-Report/feature.html#feature-set-1a-168-hourly-version), each with a column name prefix 'hr_'
    - extract the [weekend-vs-weekday trip counts](https://ac297r-mbta-2018.github.io/Final-Report/feature.html#feature-set-2-weekday-vs-weekend-total-counts), with column name 'weekday' and 'weekend'
    - extract the [48 weekday hourly vs. weekend hourly temporal patterns](https://ac297r-mbta-2018.github.io/Final-Report/feature.html#feature-set-1b-48-weekday-hourly-vs-weekend-hourly-version), each with a column name prefix 'wkday_24_'
    - extract the [time flexibility score](https://ac297r-mbta-2018.github.io/Final-Report/feature.html#feature-set-3-time-flexibility-score), with column name 'flex_wkday_24' and 'flex_wkend_24'
    - extract the [most frequent trip hours](https://ac297r-mbta-2018.github.io/Final-Report/feature.html#feature-set-4-most-frequent-trip-hours), with column name 'max_wkday_24_1', 'max_wkday_24_2', 'max_wkend_24_1'
    - return all sets of temporal features as a DataFrame

  - ```_extract_geographical_patterns(self)```:
    - extract the [# trips originated in each zip code](https://ac297r-mbta-2018.github.io/Final-Report/feature.html#geographical-patterns)
    - return the geographical features as a DataFrame

  - ```_get_one_purchase_feature(self, feature)```:
    - argument ```feature```: one item from the ```purchase_features``` list
    - extract the [# trips associated with the ticket purchasing dimension specified in ```feature```](https://ac297r-mbta-2018.github.io/Final-Report/feature.html#ticket-purchasing-pattern)
    - return one type of ticket purchasing features as a DataFrame

  - ```_extract_ticket_purchasing_patterns(self)```:
    - call ```_get_one_purchase_feature(self, feature)``` to extract the [number of trips associated with all ticket purchasing dimensions specified in ```purchase_features```](https://ac297r-mbta-2018.github.io/Final-Report/feature.html#ticket-purchasing-pattern)
    - return the ticket-purchasing features as a DataFrame

  - ```_label_rider_by_trip_frequency(self, rider)```:
    - argument ```rider```: a row in the rider feature DataFrame
    - label riders by the their riding frequency specified in the 'total_num_trips' column
      - 0: infrequent riders
      - 1: less than or equal to 20-time/month riders:
      - 2: more than 20-time/month rider
      - -1: others
    - return label

  - ```_label_commuter_rail_rider(self, rider)```:
    - argument ```rider```: a row in the rider feature DataFrame
    - label commuter rail riders except for those coming from zone 1A as 'CR except zone 1A' and 'others' otherwise
    - return label

  - ```extract_features(self)```:
    - call ```_extract_temporal_patterns()```, ```_extract_geographical_patterns()```, ```_extract_ticket_purchasing_patterns()``` to extract each group of features
    - drop infrequent riders and commuter rail riders from zones other than 1A
    - column-wise concatenate features to and save as a `.csv` file to the features cache path
    - return the concatenated rider features DataFrame

## Rider Segmentation

## Cluster Inference

## Visualization

## Auto Report Generator
