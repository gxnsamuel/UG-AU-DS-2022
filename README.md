# Uganda Administrative Units Dataset 2022 (UG-AU-DS-2022)

[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)
[![JSON](https://img.shields.io/badge/Format-JSON-green.svg)](https://www.json.org/)

## Overview

The **Uganda Administrative Units Dataset 2022** is a comprehensive, hierarchical dataset containing detailed information about Uganda's administrative structure as of July 2022. This dataset provides a complete mapping of Uganda's administrative divisions from the national level down to individual villages, making it an invaluable resource for researchers, developers, government agencies, and organizations working with Ugandan geographical and administrative data.

## Table of Contents

- [Dataset Statistics](#dataset-statistics)
- [Data Structure](#data-structure)
- [File Descriptions](#file-descriptions)
- [Usage Examples](#usage-examples)
- [Data Hierarchy](#data-hierarchy)
- [Use Cases](#use-cases)
- [Data Format](#data-format)
- [Contributing](#contributing)
- [License](#license)
- [Acknowledgments](#acknowledgments)

## Dataset Statistics

The dataset contains comprehensive administrative data with the following statistics:

| **Administrative Level** | **Count** |
|--------------------------|-----------|
| Country                  | 1         |
| Districts                | 145       |
| Constituencies           | 353       |
| Sub-counties             | 2,198     |
| Parishes                 | 9,251     |
| Villages                 | 77,027    |

This makes it one of the most complete and detailed datasets of Uganda's administrative structure publicly available.

## Data Structure

The dataset follows a hierarchical structure representing Uganda's administrative divisions:

```
Country (Uganda)
└── Districts (145)
    └── Constituencies (353)
        └── Sub-counties (2,198)
            └── Parishes (9,251)
                └── Villages (77,027)
```

Each level contains detailed information about the administrative unit, including names and counts of subordinate units.

## File Descriptions

### `dataset.json`
**Size:** ~6.9 MB  
**Format:** JSON  
**Description:** The main dataset file containing the complete hierarchical structure of Uganda's administrative units. The data is structured as nested JSON objects, making it easy to parse and navigate programmatically.

**Structure:**
```json
{
  "country": "UGANDA",
  "districts": [
    {
      "district": "DISTRICT_NAME",
      "data": [
        {
          "constituency": "CONSTITUENCY_NAME",
          "data": [
            {
              "subcounty": "SUBCOUNTY_NAME",
              "number_of_subcounties": NUMBER,
              "data": [
                {
                  "parish": "PARISH_NAME",
                  "number_of_villages": NUMBER,
                  "villages": ["VILLAGE1", "VILLAGE2", ...]
                }
              ]
            }
          ]
        }
      ]
    }
  ]
}
```

### `ADMINISTRATIVE UNITS IN UGANDA_JULY 2022.pdf`
**Size:** ~10.3 MB  
**Format:** PDF  
**Description:** Official reference document containing the administrative units of Uganda as of July 2022. This serves as the authoritative source document from which the JSON dataset was compiled.

### `LICENSE`
**Format:** Text  
**Description:** GNU General Public License v3.0 - specifies the terms under which this dataset can be used, modified, and distributed.

## Usage Examples

### Python

#### Loading the Dataset
```python
import json

# Load the dataset
with open('dataset.json', 'r', encoding='utf-8') as f:
    uganda_data = json.load(f)

# Access country name
country = uganda_data['country']
print(f"Country: {country}")

# Get number of districts
num_districts = len(uganda_data['districts'])
print(f"Number of districts: {num_districts}")
```

#### Finding a Specific District
```python
def find_district(data, district_name):
    for district in data['districts']:
        if district['district'].upper() == district_name.upper():
            return district
    return None

# Find Hoima district
hoima = find_district(uganda_data, "HOIMA")
if hoima:
    print(f"District: {hoima['district']}")
    print(f"Number of constituencies: {len(hoima['data'])}")
```

#### Counting Villages in a District
```python
def count_villages_in_district(district):
    total_villages = 0
    for constituency in district['data']:
        for subcounty in constituency['data']:
            for parish in subcounty['data']:
                total_villages += len(parish['villages'])
    return total_villages

# Count villages in a district
if hoima:
    village_count = count_villages_in_district(hoima)
    print(f"Total villages in {hoima['district']}: {village_count}")
```

#### Searching for a Village
```python
def find_village(data, village_name):
    results = []
    for district in data['districts']:
        for constituency in district['data']:
            for subcounty in constituency['data']:
                for parish in subcounty['data']:
                    if village_name.upper() in [v.upper() for v in parish['villages']]:
                        results.append({
                            'district': district['district'],
                            'constituency': constituency['constituency'],
                            'subcounty': subcounty['subcounty'],
                            'parish': parish['parish'],
                            'village': village_name
                        })
    return results

# Search for a village
results = find_village(uganda_data, "KATEREIGA I")
for result in results:
    print(f"Found in: {result['district']} > {result['constituency']} > "
          f"{result['subcounty']} > {result['parish']}")
```

### JavaScript (Node.js)

```javascript
const fs = require('fs');

// Load the dataset
const ugandaData = JSON.parse(fs.readFileSync('dataset.json', 'utf8'));

// Get all district names
const districts = ugandaData.districts.map(d => d.district);
console.log('Districts:', districts);

// Count total villages
let totalVillages = 0;
ugandaData.districts.forEach(district => {
    district.data.forEach(constituency => {
        constituency.data.forEach(subcounty => {
            subcounty.data.forEach(parish => {
                totalVillages += parish.villages.length;
            });
        });
    });
});
console.log('Total villages:', totalVillages);
```

### R

```r
library(jsonlite)

# Load the dataset
uganda_data <- fromJSON("dataset.json")

# Get district names
districts <- uganda_data$districts$district
print(paste("Number of districts:", length(districts)))

# Display first 10 districts
print(head(districts, 10))
```

## Data Hierarchy

### Level 1: Country
- **Uganda** - The sovereign nation

### Level 2: Districts (145)
Administrative regions that serve as the primary local government units. Examples include:
- Hoima
- Kampala
- Wakiso
- Gulu
- Mbarara

### Level 3: Constituencies (353)
Electoral divisions within districts, typically represented in parliament.

### Level 4: Sub-counties (2,198)
Administrative subdivisions of districts, serving as the lowest level of local government with administrative functions.

### Level 5: Parishes (9,251)
Subdivisions of sub-counties, often serving as the primary level for community organization and service delivery.

### Level 6: Villages (77,027)
The smallest administrative units, representing local communities.

## Use Cases

This dataset can be used for various applications:

1. **Geographic Information Systems (GIS)**
   - Mapping applications
   - Spatial analysis
   - Location-based services

2. **Government and NGO Operations**
   - Administrative planning
   - Resource allocation
   - Census and survey planning
   - Service delivery mapping

3. **Research and Analytics**
   - Demographic studies
   - Socioeconomic research
   - Development indicators tracking
   - Statistical analysis

4. **Application Development**
   - Address validation systems
   - Location selectors in forms
   - Regional filtering in applications
   - Administrative boundary reference

5. **Business Intelligence**
   - Market analysis
   - Distribution planning
   - Sales territory definition
   - Branch location planning

6. **Education and Training**
   - Learning about Uganda's administrative structure
   - Geography education
   - Data science projects

## Data Format

### JSON Structure Details

The dataset uses a nested JSON structure with the following key characteristics:

- **Encoding:** UTF-8
- **Format:** Valid JSON (RFC 8259 compliant)
- **File Size:** ~6.9 MB
- **Lines:** ~148,215
- **Indentation:** 4 spaces (formatted for readability)

### Data Quality Notes

- All administrative unit names are in UPPERCASE
- Hierarchical relationships are maintained through nested arrays
- Each level includes count information for subordinate units
- No missing or null values in the hierarchical structure
- Village names may contain Roman numerals (I, II, etc.) to distinguish similar names

## Contributing

We welcome contributions to improve and maintain this dataset! Here's how you can help:

### Reporting Issues
If you find any errors or inconsistencies in the data:
1. Open an issue describing the problem
2. Include the specific administrative unit(s) affected
3. Provide the correct information if known
4. Reference any official sources

### Suggesting Improvements
- Propose additional data fields
- Suggest better data structures
- Recommend new features or formats

### Updating Data
If you have updated information:
1. Fork the repository
2. Make your changes to the dataset
3. Provide references to official sources
4. Submit a pull request with a clear description

### Data Validation
Help validate the existing data by:
- Cross-referencing with official government sources
- Verifying administrative unit names and hierarchies
- Checking for duplicates or inconsistencies

## License

This dataset is licensed under the **GNU General Public License v3.0 (GPL-3.0)**.

This means you are free to:
- ✅ Use the dataset for any purpose (commercial or non-commercial)
- ✅ Modify and adapt the dataset
- ✅ Distribute the dataset
- ✅ Distribute modified versions

Under the following conditions:
- 📋 You must disclose the source code/data of any modifications
- 📋 You must license any derivative works under GPL-3.0
- 📋 You must include the original copyright notice and license
- 📋 You must state significant changes made to the dataset

See the [LICENSE](LICENSE) file for the complete license text.

## Acknowledgments

### Data Source
This dataset is compiled from official administrative data of Uganda as documented in the reference PDF "ADMINISTRATIVE UNITS IN UGANDA_JULY 2022.pdf".

### Credits
- **Original Compilation:** Government of Uganda administrative records
- **Dataset Creation:** July 2022
- **Format Conversion:** JSON structure for programmatic access

### References
- Uganda Bureau of Statistics (UBOS)
- Local Government administrative records
- Official government publications

## Citation

If you use this dataset in your research or project, please cite it as:

```
Uganda Administrative Units Dataset 2022 (UG-AU-DS-2022)
Available at: https://github.com/gxnsamuel/UG-AU-DS-2022
Accessed: [Your Access Date]
```

## Contact and Support

For questions, suggestions, or support:
- **Issues:** Use the GitHub Issues tab
- **Repository:** [https://github.com/gxnsamuel/UG-AU-DS-2022](https://github.com/gxnsamuel/UG-AU-DS-2022)

---

**Last Updated:** July 2022  
**Version:** 1.0  
**Maintained by:** gxnsamuel
