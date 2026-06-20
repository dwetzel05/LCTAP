# LCTAP — Load Carrier to Truck Assignment Problem

This repository contains the real-world benchmark instances used in the paper
*"Industrial-scale load carrier to truck assignment with implicit truck loading
constraints"* by Wetzel, Schulte, Khatouf, Römer, and Tierney.

The instances were provided by Renault and cover supply chains across Europe. They
share similarities with the [ROADEF 2022 Challenge](https://roadef.org/challenge/2022/en/).

## Overview

The Load Carrier to Truck Assignment Problem (LCTAP) assigns load carriers (items)
to a fixed fleet of heterogeneous trucks on predefined routes, minimizing total
supply chain cost (truck fixed costs plus item inventory costs) while respecting
truck loading constraints such as stacking rules, weight limits, and density limits.

## Dataset structure

The instances are grouped into four datasets by size:

| Dataset | Description | Instances | Trucks (min–max) | Items (min–max) |
|---------|-------------|-----------|------------------|-----------------|
| S       | Small       | 23        | 41 – 976         | 123 – 1,672     |
| M       | Medium      | 83        | 246 – 3,777      | 1,469 – 17,153  |
| L       | Large       | 94        | 1,344 – 9,286    | 5,971 – 50,750  |
| XL      | Very large  | 23        | 1,845 – 14,250   | 17,035 – 65,274 |

In total the repository contains **223 instances**, with up to 15,000 trucks and
210,000 items per instance.

## File organization

Each instance consists of **two CSV files**: one describing the items (load carriers)
and one describing the available trucks.

The files are organized into one subfolder per dataset (`S`, `M`, `L`, `XL`):

```
data/
├── S/
│   ├── T135_I2881_1_Items.csv
│   ├── T135_I2881_1_Trucks.csv
│   └── ...
├── M/
│   ├── T1010_I19325_1_Items.csv
│   ├── T1010_I19325_1_Trucks.csv
│   └── ...
├── L/
│   ├── T1344_I29360_1_Items.csv
│   ├── T1344_I29360_1_Trucks.csv
│   └── ...
└── XL/
    ├── T1845_I38000_1_Items.csv
    ├── T1845_I38000_1_Trucks.csv
    └── ...
```

### Naming convention

Each file follows the pattern:

```
T<numTrucks>_I<numItems>_<id>_<type>.csv
```

| Part           | Meaning                                                                              |
|----------------|--------------------------------------------------------------------------------------|
| `T<numTrucks>` | Number of trucks in the instance (e.g. `T135`)                                       |
| `I<numItems>`  | Total item demand of the instance (e.g. `I2881`)                                     |
| `<id>`         | Unique identifier distinguishing instances that share the same truck and item counts |
| `<type>`       | Either `Items` or `Trucks`                                                           |

For example, `T135_I2881_1_Items.csv` is the items file of the first instance with
135 trucks and a total item demand of 2,881, and `T135_I2881_1_Trucks.csv` is the
matching trucks file.

## File format

All files are **semicolon-separated** (`;`) and contain a header row. Dimensions
(length, width, height, nested height) are given in **millimeters**, weight in
**kilograms**, and density as the maximum weight per square meter.

Several columns hold **hyphen-separated lists** (`-`) that encode the relationships
between items and trucks. In the items file, the *n*-th entry of `ListOfTrucks`
corresponds to the *n*-th entry of `ListOfInventoryCosts`.

### `*_Items.csv`

Each row describes one item (load carrier) type.

| # | Column                | Description                                                                                 |
|---|-----------------------|---------------------------------------------------------------------------------------------|
| 1 | `ID`                  | Unique item identifier                                                                      |
| 2 | `Nbr`                 | Demand: number of units of this item to be delivered                                        |
| 3 | `Length`              | Item length (mm)                                                                            |
| 4 | `Width`               | Item width (mm)                                                                             |
| 5 | `Height`              | Item height (mm)                                                                            |
| 6 | `NestedHeight`        | Height that can be nested within another item when stacking (mm; `0` if not nestable)       |
| 7 | `Weight`              | Item weight (kg)                                                                            |
| 8 | `SC`                  | Stackability code: items must share an identical code to be placed in the same stack        |
| 9 | `MaxStackability`     | Maximum number of times the item may be stacked together with other items                   |
| 10 | `Orientation`        | Allowed orientation: `lengthwise`, `widthwise`, `free`, or `none`                           |
| 11 | `ListOfTrucks`        | Hyphen-separated list of truck IDs that can transport this item                             |
| 12 | `ListOfInventoryCosts`| Hyphen-separated list of inventory costs, aligned position-by-position with `ListOfTrucks`  |

### `*_Trucks.csv`

Each row describes one available truck.

| # | Column         | Description                                                              |
|---|----------------|--------------------------------------------------------------------------|
| 1 | `ID`           | Unique truck identifier                                                  |
| 2 | `Length`       | Truck loading length (mm)                                                |
| 3 | `Width`        | Truck loading width (mm)                                                 |
| 4 | `Height`       | Truck loading height (mm)                                                |
| 5 | `Weight`       | Maximum transportable weight (kg)                                        |
| 6 | `Density`      | Maximum density the truck can carry (max. weight per square meter)       |
| 7 | `ListOfItems`  | Hyphen-separated list of item IDs that this truck can carry              |
| 8 | `TruckCost`    | Fixed cost of deploying the truck on its assigned route                  |
