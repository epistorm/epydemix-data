# Epydemix Data

[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)
[![Read the Docs](https://readthedocs.org/projects/epydemix/badge/?version=latest)](https://epydemix.readthedocs.io/en/latest/?badge=latest)

![Alt text](https://github.com/epistorm/epydemix/blob/main/tutorials/img/epydemix-logo.png)

**[Documentation](https://epydemix.readthedocs.io/en/latest/)** | **[Website](https://www.epydemix.org/)** | **[Tutorials](https://github.com/epistorm/epydemix/tree/main/tutorials)**

This repository contains real-world population data and contact matrices used for epidemic modeling and simulation in the [**epydemix**](https://github.com/epistorm/epydemix/tree/main) python package. The data covers demographic distributions and various contact matrices for more than $400$ regions worldwide.

The contact matrices indicate interactions between individuals in different contexts (e.g., home, work, school, community) and are sourced from the following studies:
- [Inferring high-resolution human mixing patterns for disease modeling](https://www.nature.com/articles/s41467-020-20544-y) (`mistry_2021`)
- [Projecting contact matrices in 177 geographical regions: An update and comparison with empirical data for the COVID-19 era](https://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1009098) (`prem_2021`)
- [Projecting social contact matrices in 152 countries using contact surveys and demographic data](https://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1005697) (`prem_2017`)
- [Epistorm-Mix: Mapping Social Contact Patterns for Respiratory Pathogen Spread in the Post-Pandemic United States](https://www.medrxiv.org/content/10.1101/2025.11.20.25340662v1) (`litvinova_2025`)

**Note**: contact matrices from `mistry_2021` are 85×85 in dimension, with each row and column representing a single-year age group, and the last one corresponding to ages 84 and above. Contact matrices from `prem_2021`, `prem_2017`, and `litvinova_2025` are 16×16, with each row and column representing a 5-year age group (0–4, 5–9, etc.), and the last group covering ages 75 and above.

When using the contact data provided by the **Epydemix** package please ensure that you cite the relevant research papers associated with each data source.

## Supported Geographies

A comprehensive list of supported geographies can be found in the [locations.csv](https://github.com/epistorm/epydemix-data/blob/main/locations.csv) file. This file provides detailed information about the available contact matrices and population data for each location. A sample of the file is shown below:

| **location**    | **primary_contact_source** | **contact_sources**       | **population_source** |
|-----------------|----------------------------|---------------------------|-----------------------|
| Afghanistan     | prem_2021                  | prem_2021                 | https://population.un.org/wpp/ |
| Albania         | prem_2021                  | prem_2021\|prem_2017      | https://population.un.org/wpp/ |
| Algeria         | prem_2021                  | prem_2021\|prem_2017      | https://population.un.org/wpp/ |
| Andorra         | prem_2017                  | prem_2017                 | https://population.un.org/wpp/ |
| Angola          | prem_2021                  | prem_2021                 | https://population.un.org/wpp/ |
| ...             | ...                        | ...                       | ...                   |

The file contains the following information:
- **Location Names**: The geographic regions for which contact and demographic data are available.
- **Primary Contact Source**: The default contact matrix source used by **Epydemix** for each location. If a specific contact source isn't specified, **Epydemix** will attempt to import the primary source listed. When available, Mistry 2021 is prioritized as the primary source, followed by Prem 2021, and then Prem 2017.
- **Contact Sources**: A pipe-separated list of all available contact matrix sources for each location (e.g., `prem_2021|prem_2017`). Some locations may have multiple sources, while others may only have one.
- **Population Data Source**: The source of demographic data, such as the [United Nations World Population Prospects 2024](https://population.un.org/wpp/) and the [US Census Bureau](https://api.census.gov/data/2023/pep/charv) (2023).


### Example Folder Structure: `United_States`

Each location folder is organized as follows:

```
United_States/
    ├── demographic/
    │   └── age_distribution.csv
    └── contact_matrices/
        ├── mistry_2021/
        │   ├── contacts_matrix_all.csv
        │   ├── contacts_matrix_home.csv
        │   ├── contacts_matrix_work.csv
        │   ├── contacts_matrix_community.csv
        │   └── contacts_matrix_school.csv
        ├── litvinova_2025/
        │   ├── ...
        ├── prem_2017/
        │   ├── ...
        └── prem_2021/
            ├── ...
```
Where:
- **`demographic/`**: Contains demographic data for the population of the United States.
  - **`age_distribution.csv`**: A CSV file detailing the population distribution by age group.

- **`contact_matrices/`**: This directory contains the contact matrices, which describe interaction patterns between different age groups in various contexts.
  - **`mistry_2021/`**: Contains contact matrices from the Mistry 2021 study, with data separated by context (e.g., home, work, school, community).
    - **`contacts_matrix_all.csv`**: Aggregated contact matrix across all contexts.
    - **`contacts_matrix_home.csv`**: Contact matrix for interactions within households.
    - **`contacts_matrix_work.csv`**: Contact matrix for workplace interactions.
    - **`contacts_matrix_community.csv`**: Contact matrix for community-based interactions.
    - **`contacts_matrix_school.csv`**: Contact matrix for interactions in schools.
  - **`litvinova_2025/`**: Contains contact matrices from Litvinova et al. 2025, also structured by context.
  - **`prem_2017/`**: Contains similar contact matrix files from the Prem 2017 study, broken down by the same contexts (home, work, school, community, all).
  - **`prem_2021/`**: Contains updated contact matrices from the Prem 2021 study, also structured by context.

---

## Additional Demographic Attributes (United States only)

For the United States, additional contact matrices and population data stratified by **sex** and **race/ethnicity** are available under `data/other_attributes/`. These are sourced from [Epistorm-Mix](https://www.medrxiv.org/content/10.1101/2025.11.20.25340662v1) (`litvinova_2025`) and organized as follows:

- `data/other_attributes/sex/` — 2×2 contact matrices (Female, Male); locations listed in [data/other_attributes/sex/locations.csv](https://github.com/epistorm/epydemix-data/blob/main/data/other_attributes/sex/locations.csv)
- `data/other_attributes/race_ethnicity/` — 5×5 contact matrices (Asian Non-Hispanic, Black Non-Hispanic, Hispanic/Latino, Other Non-Hispanic, White Non-Hispanic); locations listed in [data/other_attributes/race_ethnicity/locations.csv](https://github.com/epistorm/epydemix-data/blob/main/data/other_attributes/race_ethnicity/locations.csv)

Each attribute follows the same folder structure as the age data, with `population.csv` in place of `age_distribution.csv` for the demographic file.

**Note**: US population division between sexes and race/ethnicities is sourced from the dataset *Decennial Census, DEC Demographic Profile, Table DP1*, while total population counts and age division from the *Annual Resident Population Estimates by Age, Sex, Race, and Hispanic Origin*, both released from the U.S. Census Bureau.

---

### Using Data with the **Epydemix** Package

The **Epydemix** package provides flexibility in loading demographic data and contact matrices for various regions. The `epydemix.population.load_epydemix_population` function allows you to load this data either via **online import** (fetching from this repository) or **offline import** (loading from a local directory).

- **Online Import**: If `path_to_data` is not provided, **Epydemix** will attempt to import the data directly from this GitHub repository.
- **Offline Import**: If `path_to_data` is provided, **Epydemix** will attempt to load the data from a local directory. For this option, users must first download the corresponding data folder hosted on this GitHub repository.

### Example: Loading Population Data for the United States

Below is an example of how to load population data for the United States, specifying both online and offline import options:

```python
from epydemix.population import load_epydemix_population
from epydemix.model import load_predefined_model

# Example 1: Online import (data will be fetched from GitHub)
population = load_epydemix_population(
    population_name="United_States",
    # Specify the preferred contact data source (needed only if you want to override the default primary source)
    contacts_source="mistry_2021",
    layers=["home", "work", "school", "community"]  # Load contact layers (by default all layers are imported)
)

# Example 2: Offline import (data will be loaded from a local directory)
# Ensure that the folder is downloaded locally before running this
population = load_epydemix_population(
    population_name="United_States",
    path_to_data="path/to/local/epydemix_data/",  # Path to the local data folder
    # Specify the preferred contact data source (needed only if you want to override the default primary source)
    contacts_source="mistry_2021",
    layers=["home", "work", "school", "community"]  # Load contact layers (by default all layers are imported)
)

# Create model
model = load_predefined_model(model_name="SIR")

# Use the population in your epidemic model
model.set_population(population=population)
```

**Note**: By default, contact matrices and population data are imported using five age groups: 0–4, 5–19, 20–49, 50–64, and 65+. Custom age groupings can be specified during the import step. For more details, see this [tutorial](https://github.com/epistorm/epydemix/blob/main/tutorials/2_Modeling_with_Population_Data.ipynb).

---

## Versioning

This repository is versioned using git tags. Each release (e.g., `v1.0.0`) represents a snapshot of the data that can be referenced for reproducibility. The `load_epydemix_population` function in the **epydemix** package points to a specific version of this repository by default, ensuring consistent results across runs. Users can override the version to use a different data release.

---
## Citation
The paper describing the development of Epydemix is available [here](https://doi.org/10.1371/journal.pcbi.1013735).
To reference our work, please use the following citation:
```
@article{gozzi2025epydemix,
  title={Epydemix: An open-source Python package for epidemic modeling with integrated approximate Bayesian calibration},
  author={Gozzi, Nicol{\'o} and Chinazzi, Matteo and Davis, Jessica T and Gioannini, Corrado and Rossi, Luca and Ajelli, Marco and Perra, Nicola and Vespignani, Alessandro},
  journal={PLOS Computational Biology},
  volume={21},
  number={11},
  pages={e1013735},
  year={2025},
  publisher={Public Library of Science San Francisco, CA USA}
}
```

---
## License

This project is licensed under the GPL-3.0 License. See the [LICENSE](LICENSE) file for more details.

## Contact

For questions or issues, please open an issue on GitHub or contact the maintainers at `epydemix@isi.it`.
