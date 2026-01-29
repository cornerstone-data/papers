
# Methodology for Cornerstone U.S. National Model 

authors, TBD

This paper describes the methodological approach to building the detailed United States national environmentally-extended input-output (EEIO) model that is to be integrated into the Cornerstone global EEIO model, v1.0.
This methodology draws on approaches used previously in the the USEEI0 and CEDA-US models. 
The Cornerstone team first reviewed and identified all the differences between reference USEEIO and CEDA-US models, and the systematically assessed the differences both theoretically and quantitatively, as relevant. 
That work is captured in a discussions and related scripts found in the [cornerstone-data/methods](https://github.com/cornerstone-data/methods) repository. References to those discussions are provided herein.
This paper synthesizes those discussions into a consolidated and harmonious description of the selected methods.
The description of the methodology is split into three sections: (1) the economic data and steps used to forming the basis economic input-output table (IOT) matrices and associated economic datasets;
(2) the greenhouse gas emissions (GHG) and indicator datasets and the approach used to estimate industry GHG totals; and (3) the integration of the economic and environmental data to form model result matrices of direct and indirect GHG emissions per dollar. 

# Data Source and Procedures for Construction of IOTs

Data are requiring for building the economic IOTs as well as for linking the environmental data to build the EEIO extensions.

## Data Inputs on U.S. Industry
The data sources used are listed in [Table 1](#table-1.-economic-data-sources).

### Table 1. Data Sources Used in IOT Construction
| Name                                                       | Creator | Sources | DataYears |
|:-----------------------------------------------------------|:--------|:----------------------------------------------------------------------------------------------------------------------|:----------|
| Make and Use, Detail, After Redefinitions, Producer Price | BEA | [Bureau of Economic Analysis Industry Input-Output Accounts](https://www.bea.gov/industry/input-output-accounts-data) | 2017 |
| Gross Output by Industry                                   | BEA | [Gross Output By Industry](https://www.bea.gov/data/industries/gross-output-by-industry) | 2017, 2023 |
| Gross Output Chain Price Index                             | BEA | [Gross Output By Industry](https://www.bea.gov/data/industries/gross-output-by-industry) | 2012-2024 |
| Margins                                                    | BEA | [Bureau of Economic Analysis Industry Underlying Estimates](https://www.bea.gov/industry/industry-underlying-estimates) | 2017 |
| Import Matrix, After Redefinitions                         | BEA | [Bureau of Economic Analysis Industry Input-Output Accounts](https://www.bea.gov/industry/input-output-accounts-data) | 2017 |
| Summary Statistics for  Waste Sector (EC1756BASIC)                                              | Census Bureau | [2017 Administrative and Support and Waste Management and Remediation Services (NAICS Sector 56)](https://www.census.gov/data/tables/2017/econ/economic-census/naics-sector-56.html) | 2017 |
| Services Annual Survey (SAS)                                              | Census Bureau |  | 2017 |
| RCRA Online                                             | Census Bureau |  | 2012 |

The benchmark Make and Use matrices from the BEA, provided at 5 year intervals, were the fundamental US IOT inputs to USEEIO and CEDA models.
The 2017 tables are the most recent official release.
These tables are prepared in producer and purchaser price, as well as "before redefinitions" (BR) and "After Redefinitions" (AR).
We use the producer price version as was done previously both for USEEIO and CEDA. 
The purchaser price version provide less transparency in the input requirements because for each value in the Use table for an industries' use of a commodity includes not just the value of the commodity used but also the value of added wholesale, retail and transportation costs.
The producer price version is used to provide more transparency.

We use the AR version of the Make and Use tables, which were previously used in CEDA.
"Redefinitions" are defined by BEA in the [IO manual](https://www.bea.gov/resources/methodologies/concepts-methods-io-accounts) as "secondary products that are produced using input patterns that differ substantially from that of the primary product of the industry in which they are produced."
In the AR Make table, the BEA takes selected commodity outputs from industries that are producing it as a secondary product and moves it to the primary producing industry (the industry with the same code as the commodity since in the BEA tables industries use the same codes as commodities). The following list represents most of the redefinitions:
- Construction activities performed by other industries are redefined to construction.
- Manufacturing activities in non-manufacturing industries are redefined to manufacturing.
- Trade activities in non-trade industries are redefined to trade. Redefinitions
are not made between wholesale and retail trade.
- Rental activities in non-rental industries are redefined to real estate and rental industries.
- Service activities in non-service industries are redefined to services."

This shifts around industry output but commodity output stays the same.
In the AR Use table, uses of commodities are additionally moved away from industries where commodity output is removed, and to the industries where commodity output was added.
The result of the redefinitions are commodities with input structures that more closely match the associated primary industry input structure.

The BEA Gross Output by Industry is the authoritative dataset on both annual gross output and annual price index by detailed industry that most closely matches the Make and Use tables. 
The Import Matrix (AR) is an accompanying table to the selected Use table with values for uses of imported commodities by industries and final users.
The final three sources listed in Table 1 are used for the waste sector disaggregation.
All the uses of these tables are described below. 

## Selected IOT Schema
A complete list of commodities, industries, and final use types is given in the Appendix. 
For all selected industries, value added and final demand components we use the BEA's code and name. 
For all selected commodities we use the BEA code but assign a commodity-like name to differentiate them from industries as done in USEEIO v2.

We use the vast majority of the industries and commodities as given in the 2017 benchmark Make and Use tables as is.
However there are some cases in which industries or commodities are removed, combined with others, or split into multiple. 
Those cases are described here. 

### Combination of Industries
Government industries that produce that same commodities as private industries are aggregated.
_Federal electric utilities_ (BEA S00101) and _State and local government electric utilities_ (BEA S00202) are included in _Electric power generation, transmission, and distribution_ because all produce _Electricity_. 
_State and local government passenger transit_ (BEA S00201) is included in _Transit and ground passenger transportation_ (BEA 485000).
These aggregates are represented with the private industry name and code because this is also the code for the primary product they are producing. 
 See Discussions [#7](https://github.com/cornerstone-data/methods/discussions/7) and [#44](https://github.com/cornerstone-data/methods/discussions/44).


### Removal of Commodities

Some items are present in the Make and Use tables as commodities only for accounting/balancing purposes and do not represent tangible commodities. 
These include:
- _Customs duties_ (BEA 4200ID)
- _Noncomparable imports_ (BEA S00300)
- _Rest of the world adjustment_ (BEA S00900)

_Customs duties_ are tariffs or taxes collected on imports. BEA defines _Rest of the world adjustment_ as "values for exports and imports that have offsetting adjustments to personal consumption expenditures (PCE) and government". _Noncomparable imports_ are "(1) Services that are produced and consumed abroad, such as airport expenditures by U.S. airlines in foreign countries; (2) services imports that are unique, such as payments for the rights to patents, copyrights, or industrial processes; and (3) services imports that cannot be identified by type, such as payments by U.S. companies to their foreign affiliates for an undefined “basket” of services". [BEA IO Manual](https://www.bea.gov/resources/methodologies/concepts-methods-io-accounts).

These commodities are removed from the model as they are not intended to be use to characterize environmental impacts. See discussion [#1](https://github.com/cornerstone-data/methods/discussions/1). Removal details are specified below.

The commodity _Scrap_ (BEA S00401) represents discarded materials but is not specific to any material or product and not determined to be useful for environmental characterization. See discussion [#5](https://github.com/cornerstone-data/methods/discussions/5). It is removed as described below.  

### Addition of Industries and Commodities - Waste Sector

The _Waste and Remediation_ (BEA 562000) industry and associated commodity is removed and and replaced with more specific industries and commodities. 
For each new industry, new commodity is created. 

| Industry/Commodity Name                           |  USEEIO Code |
|---------------------------------------------------|-------------|
| Solid waste collection                            | 562111      |
| Hazardous waste collection treatment and disposal | 562HAZ      |
| Solid waste landfilling                           | 562212      |
| Solid waste combustors and incinerators           | 562213      |
| Remediation services                              | 562910      |
| Material separation/recovery facilities           | 562920      |
| Other waste collection and treatment services     | 562OTH      |

These are the same waste commodities and industries present in [USEEIO v2.0](https://www.nature.com/articles/s41597-022-01293-7).
This results in a net addition of 6 industries and 6 commodities. See discussion [#8](https://github.com/cornerstone-data/methods/discussions/8).

## Treatment of Economic Data and IOT Construction


# GHG Emissions Model and Indicators

## Data Sources
### Table 2. Data Sources Used in GHG Emission Model
| Name                                                       | Creator | Sources | DataYears |
|:-----------------------------------------------------------|:--------|:----------------------------------------------------------------------------------------------------------------------|:----------|
| GHG Inventory | EPA | [ Inventory of U.S. Greenhouse Gas Emissions and Sinks](https://www.bea.gov/industry/input-output-accounts-data) | 2017 |
| Make and Use, Detail, After Redefinitions, Producer Price | BEA | [Bureau of Economic Analysis Industry Input-Output Accounts](https://www.bea.gov/industry/input-output-accounts-data) | 2017 |

| MECS Tables 2.3  and Use, Detail, After Redefinitions, Producer Price | BEA | [Bureau of Economic Analysis Industry Input-Output Accounts](https://www.bea.gov/industry/input-output-accounts-data) | 2017 |



## Sector Attribution Model








# EEIO model







