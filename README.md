# Epydemix Data

[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)
[![Read the Docs](https://readthedocs.org/projects/epydemix/badge/?version=latest)](https://epydemix.readthedocs.io/en/latest/?badge=latest)

![Alt text](https://github.com/epistorm/epydemix/blob/main/tutorials/img/epydemix-logo.png)

**[Documentation](https://epydemix.readthedocs.io/en/latest/)** | **[Website](https://www.epydemix.org/)** | **[Tutorials](https://github.com/epistorm/epydemix/tree/main/tutorials)**

This repository contains real-world population data and synthetic contact matrices used for epidemic modeling and simulation in the [**epydemix**](https://github.com/epistorm/epydemix/tree/main) python package. The data covers demographic distributions and various contact matrices for more than $400$ regions worldwide.

The contact matrices indicate interactions between individuals in different contexts (e.g., home, work, school, community) and are sourced from the following studies:
- [Inferring high-resolution human mixing patterns for disease modeling](https://www.nature.com/articles/s41467-020-20544-y) (`mistry_2021`)
- [Projecting contact matrices in 177 geographical regions: An update and comparison with empirical data for the COVID-19 era](https://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1009098) (`prem_2021`)
- [Projecting social contact matrices in 152 countries using contact surveys and demographic data](https://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1005697) (`prem_2017`)

**Note**: contact matrices from `mistry_2021` are 85×85 in dimension, with each row and column representing a single-year age group, and the last one corresponding to ages 84 and above. In contrast, contact matrices from `prem_2021` and `prem_2017` are 16×16, with each row and column representing a 5-year age group (0–4, 5–9, etc.), and the last group covering ages 75 and above.

When using the contact data provided by the **Epydemix** package please ensure that you cite the relevant research papers associated with each data source. 

## Supported Geographies

A comprehensive list of supported geographies can be found in the [locations.csv](https://github.com/epistorm/epydemix-data/blob/main/locations.csv) file. This file provides detailed information about the available contact matrices and population data for each location. A sample of the file is shown below:

| **location**    | **primary_contact_source** | **mistry_2021** | **prem_2021** | **prem_2017** | **population_source** |
|-----------------|----------------------------|-----------------|---------------|---------------|------------------------|
| Afghanistan     | prem_2021                   | False          | True          | False         | https://population.un.org/wpp/Download/Standard/CSV/ |
| Albania         | prem_2021                   | False          | True          | True          | https://population.un.org/wpp/Download/Standard/CSV/ |
| Algeria         | prem_2021                   | False          | True          | True          | https://population.un.org/wpp/Download/Standard/CSV/ |
| Andorra         | prem_2021                   | False          | False         | True          | https://population.un.org/wpp/Download/Standard/CSV/ |
| Angola          | prem_2021                   | False          | True          | False         | https://population.un.org/wpp/Download/Standard/CSV/ |
| ...             | ...                         | ...            | ...           | ...           | ...                    |


The file contains the following information:
- **Location Names**: The geographic regions for which contact and demographic data are available.
- **Primary Contact Source**: The default contact matrix source used by **Epydemix** for each location. If a specific contact source isn't specified, **Epydemix** will attempt to import the primary source listed. When available, Mistry 2021 is prioritized as the primary source, followed by Prem 2021, and then Prem 2017.
- **Availability of Contact Matrices**: The file indicates whether contact matrices are available from the Mistry 2021, Prem 2021, or Prem 2017 studies for each location. Some locations may have multiple sources, while others may only have one.
- **Population Data Source**: The file also provides the source of demographic data, primarily from the [United Nations World Population Prospects 2024](https://population.un.org/wpp/).


### Example Folder Structure: `United_States`

Each location folder is organized as follows:

```
United_States/
    ├── demographic/
    │   └── age_distribution.csv
    ├── contact_matrices/
        ├── mistry_2021/
        │   ├── contacts_matrix_all.csv
        │   ├── contacts_matrix_home.csv
        │   ├── contacts_matrix_work.csv
        │   ├── contacts_matrix_community.csv
        │   └── contacts_matrix_school.csv
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
  - **`prem_2017/`**: Contains similar contact matrix files from the Prem 2017 study, broken down by the same contexts (home, work, school, community, all).
  - **`prem_2021/`**: Contains updated contact matrices from the Prem 2021 study, also structured by context.

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
## Citation 
The preprint describing the development of Epydemix is available [here](https://www.medrxiv.org/content/10.1101/2025.05.07.25327151v1).
To reference our work, please use the following citation:
```
@article{gozzi2025epydemix,
	author = {Gozzi, Nicol{\`o} and Chinazzi, Matteo and Davis, Jessica T. and Gioannini, Corrado and Rossi, Luca and Ajelli, Marco and Perra, Nicola and Vespignani, Alessandro},
	title = {Epydemix: An open-source Python package for epidemic modeling with integrated approximate Bayesian calibration},
	elocation-id = {2025.05.07.25327151},
	year = {2025},
	doi = {10.1101/2025.05.07.25327151},
	publisher = {Cold Spring Harbor Laboratory Press},
	URL = {https://www.medrxiv.org/content/early/2025/05/08/2025.05.07.25327151},
	eprint = {https://www.medrxiv.org/content/early/2025/05/08/2025.05.07.25327151.full.pdf},
	journal = {medRxiv}
}
```

---
## License

This project is licensed under the GPL-3.0 License. See the [LICENSE](LICENSE) file for more details.

## Contact

For questions or issues, please open an issue on GitHub or contact the maintainers at `epydemix@isi.it`.

