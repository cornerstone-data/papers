
# Methodology for Cornerstone U.S. National Model 

Wesley Ingwersen, Ben Young, Mo Li, Jorge Vendries, Catherine Birney, Brian Tobin

This paper describes the methodological approach to building the detailed United States national environmentally-extended input-output (EEIO) model that is to be integrated into the Cornerstone (CS) global EEIO model, v1.0.
This U.S. model is intended to meet the model requirements for the US national component described in the [v1 model requirements](https://github.com/cornerstone-data/methods/blob/main/ModelRequirements.md).
This methodology draws on approaches used previously in the the USEEI0 and CEDA-US models. 
The Cornerstone team first reviewed and identified all the differences between reference USEEIO and CEDA-US models, and the systematically assessed the differences both theoretically and quantitatively, as relevant. 
That work is captured in a discussions and related scripts found in the [cornerstone-data/methods](https://github.com/cornerstone-data/methods) repository. References to those discussions are provided herein.

The description of the methodology is split into three sections: (1) the economic data and steps used to forming the basis economic input-output table (IOT) matrices and associated economic datasets;
(2) the greenhouse gas emissions (GHG) model and the approach used to estimate industry GHG totals; and (3) the integration of the economic and environmental data to form model result matrices of direct GHG emissions per dollar. 

Additional steps in the preparation of the EEIO model occur in concert with the development of the model for other countries and regions and their integration into the global model and are not described here.

# Data Source and Procedures for Construction of IOTs

Data are requiring for building the economic IOTs as well as for linking the environmental data to build the EEIO extensions.

## Data Inputs to IOTs
The data sources used are listed in [Table 1](#table-1.-economic-data-sources).

### Table 1. Data Sources Used in IOT Construction
| Name                                                       | Creator | Sources | DataYears |
|:-----------------------------------------------------------|:--------|:----------------------------------------------------------------------------------------------------------------------|:----------|
| Make and Use, Detail, After Redefinitions, Producer Price | BEA | [Bureau of Economic Analysis Industry Input-Output Accounts](https://www.bea.gov/industry/input-output-accounts-data) | 2017 |
| Gross Output by Industry                                   | BEA | [Gross Output By Industry](https://www.bea.gov/data/industries/gross-output-by-industry) | 2017, 2023 |
| Gross Output Chain Price Index                             | BEA | [Gross Output By Industry](https://www.bea.gov/data/industries/gross-output-by-industry) | 2012-2024 |
| Make, Detail, Before Redefinitions, Producer Price         | BEA | [Bureau of Economic Analysis Industry Input-Output Accounts](https://www.bea.gov/industry/input-output-accounts-data) | 2017 |
| Margins                                                    | BEA | [Bureau of Economic Analysis Industry Underlying Estimates](https://www.bea.gov/industry/industry-underlying-estimates) | 2017 |
| Import Matrix, After Redefinitions                         | BEA | [Bureau of Economic Analysis Industry Input-Output Accounts](https://www.bea.gov/industry/input-output-accounts-data) | 2017 |
| Summary Statistics for  Waste Sector (EC1756BASIC)                                              | Census Bureau | [2017 Administrative and Support and Waste Management and Remediation Services (NAICS Sector 56)](https://www.census.gov/data/tables/2017/econ/economic-census/naics-sector-56.html) | 2017 |
| Services Annual Survey                                              | Census Bureau | [Services Annual Survey (SAS)](https://www2.census.gov/programs-surveys/sas/tables/time-series/sas-latest/sas-22.xlsx) | 2017 |
|  Biennial Report                                           |  EPA | [RCRAInfo Hazardous Waste Information Platform](https://rcrapublic.epa.gov/rcra-hwip/) | 2012 |

The benchmark Make and Use matrices from the BEA, provided at 5 year intervals, were the fundamental US IOT inputs to USEEIO and CEDA models.
The 2017 tables are the most recent official release.
These tables are prepared in producer and purchaser price, as well as "Before redefinitions" (BR) and "After Redefinitions" (AR).
We use the producer price version as was done previously both for USEEIO and CEDA. 
The purchaser price version provide less transparency in the input requirements because for each value in the Use table for an industries' use of a commodity includes not just the value of the commodity used but also the value of added wholesale, retail and transportation costs.
The producer price version is used to provide more transparency.

We use the AR version of the Make and Use tables, which were previously used in CEDA. 
See [discussion #3](https://github.com/cornerstone-data/methods/discussions/3) on the choice of AR tables.
"Redefinitions" are defined by BEA in the [IO manual](https://www.bea.gov/resources/methodologies/concepts-methods-io-accounts) as "secondary products that are produced using input patterns that differ substantially from that of the primary product of the industry in which they are produced."
In the AR Make table, the BEA takes selected commodity outputs from industries that are producing it as a secondary product and moves it to the primary producing industry (the industry with the same code as the commodity since in the BEA tables industries use the same codes as commodities). 
The following list represents most of the redefinitions:
- Construction activities performed by other industries are redefined to construction.
- Manufacturing activities in non-manufacturing industries are redefined to manufacturing.
- Trade activities in non-trade industries are redefined to trade. Redefinitions
are not made between wholesale and retail trade.
- Rental activities in non-rental industries are redefined to real estate and rental industries.
- Service activities in non-service industries are redefined to services."

The redefinitions shift around industry output but commodity output stays the same.
In the AR Use table, uses of commodities are additionally moved away from industries where commodity output is removed, and to the industries where commodity output was added.
The result of the redefinitions are commodities with input structures that more closely match the associated primary industry input structure.

The BEA Gross Output by Industry is the authoritative dataset on both annual gross output and annual price index by detailed industry that most closely matches the Make and Use tables. 
The Import Matrix (AR) is an accompanying table to the selected Use table with values for uses of imported commodities by industries and final users.
The Margins table contain data on cost of transportation, wholesale and retail to move each commodity to each user corresponding with the benchmark Use table.
The final three sources listed in Table 1 are used for the waste sector disaggregation.
The Biennial Report is a collection of has data on the generation, handling and ultimate disposition of wastes that are according the U.S. [Resource Conservation and Recovery Act (RCRA)](https://www.epa.gov/laws-regulations/summary-resource-conservation-and-recovery-act) declared as hazardous waste in the U.S. 
It includes data on specific waste generators, waste type generated, quantity generated, and disposition of the waste. 
The EC1756BASIC is a set of summary statistics derived from the Economic Census of the Waste sector, reporting data such as costs, revenue, and number of establishments.
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

### Table 2. Waste Sectors

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

We use matrix algebra to describe the treatment of the economic data to construct the US IOT. 
Variables are defined in Table 3. 

### Table 3. Variables.

| Variable        | Contents                                                                               | Shape                 |
|-----------------|----------------------------------------------------------------------------------------|-----------------------|
| **A**           | direct requirements matrix                                                             | commodity x commodity |
| **$A_d$**       | domestic direct requirements matrix                                                    | commodity x commodity |
| **B**           | direct environmental flows per commodity output matrix                                 | flow x commodity      |
| **C**           | characterization factor matrix                                                         | indicator x flow      |
| c               | subscript for commodity                                                                |                       |    
| d               | subscript for domestic                                                                 |                       |    
| **$\chi$**      | non-scrap output ratio vector                                                          | industry              |    
| **i**           | vector of 1s                                                                           | varies                |
| i               | subscript for industry                                                                 |                       |
| **L**           | Leontief matrix                                                                        | commodity x commodity |
| **$L_d$**       | Leontief matrix only including total domestic requirements                             | commodity x commodity |
| **M**           | direct + indirect flows matrix, aka multipliers                                        | flow x commodity      |
| m               | subscript for imports                                                                  |                       |
| **O**           | correspondence matrix                                                                  | varies                |
| **q** or **Q**  | commodity output vector or matrix                                                      | commodities or commodities x year          |
| **$\Pi$**       | Price index matrix                                                                     | industry x year       |
| **R**           | Redefinitions co-product ratios matrix                                                 | industry x commodity  |
| **s**           | scrap production vector                                                                | industries            |
| **$\rho$**      | inflation adjustment factor vector                                                     | industries            |
| **T**           | Commodity mix matrix                                                                   | commodity x industry  |
| **U**           | Use table industry transactions                                                        | commodity x industry  |
| **V^**           | Make table                                                                            | industry x commodity  |
| **W**           | Transformation matrix                                                                  | industry x commodity  |
| **x** or **X**  | industry output vector or matrix                                                       | industries or industries x year           |
| **y**           | total final demand vector                                                              | commodities           |
| y               | subscript for year                                                                     |                       |
| ^               | symbol that indicates diagonalized form (matrix form) of a vector                      |                       |
| '               | symbol that indicates the transposition of a matrix of vector                          |                       |
| -               | bar over to represent a variable before transformation into CS conventions             |                       |

We start with the Make table from BEA, $\bar{V}$, and the Use table from BEA, $\bar{U}$.  
To combine the government industries we use an industry x industry correspondence matrices, $O_i$, and a commodity by commodity correspondence matrix, $O_c$, to aggregate the sector in the Make (the rows) and Use. 
In these correspondence matrices the CS sectors are on the rows and the BEA sectors on the columns. 
This removes the rows and columns of special accounting industries and commodities from the matrices, and aggregates the government utilities. 
Note that these correspondence matrices do not account for the additions of the waste sectors.

$$ V = O_i \bar{V} O_c' $$

$$ U = O_c \bar{U} O_i' $$

Following this initial mapping to the CS schema, the _Waste and Remediation Services_ (562000) industry and commodity are disaggregated in the $U$ and $V$ tables to match those sectors give in Table 2. 
 In the Use table, the Biennial Report data on waste shipped to and from the waste sectors is used to allocate the new waste management commodities across the new waste waste management industries based on the relative shares of the total waste shipped.
 The new waste industries uses of non-waste commodities are estimated based on the detailed expenses from the SAS of the waste industries.
 The uses of the new waste commodities by non-waste industries was estimated using the Economic Census data.
 In the Make table, we assume that each new waste industry only produces its primary waste commodity.
 Further details on this disaggregation procedure can be found in the the Disaggregation of the waste and remediation services sector section of the [USEEIO v2 documentation](https://doi.org/10.1038/s41597-022-01293-7).

The total commodity and industry output, $q$ and $x$, is derived from the Make table, where $q$ is the columns sums and $x$ is the row sums.
Although the commodity, _Scrap_, is not part of the final sector schema, it does need to be included in order to adjust the IOT.  
The scrap amounts produced by each industry, $s$, must be added to get the industry output.   

$$ q = iV $$

$$ x = Vi + s $$

We calculate the non-scrap portion of industry output for each industry in the vector, $\chi$. 

$$ \chi = (x-s){x}^-1 $$

This vector is used to modify the commodity output normalized form of the Make matrix, also knows as the market shares matrix, to reflect an increased input of non-scrap commodities needed to fill the gap left from scrap removal in industry output. This new matrix is $W$.

$$ W = \hat{\chi}^{-1} V\hat{q}^{-1} $$  

The Use table is normalized by industry output and then post-multiplied by $W$ to get $A$ in commodity by commodity format.
This method for creating the $A$ matrix is based on the _industry- technology_ assumption, wherein the manufacture of the primary and any secondary commodities by an industry uses the same production requirements, and the commodity requirements are based therefore on the mix of industries that produce that commodity, weighted by their relative share of total commodity output. See [Input Output Analysis by R. Miller and P. Blair 2022](https://doi.org/10.1017/9781108676212).

$$ A = U\hat{x}^{-1}W $$

$L$, the Leontief inverse, or the total requirements matrix, is obtained from $A$.
$L$ is in commodity x commodity form and represents the total upstream inputs of commodities (rows) used to make a commodity (columns).

$$ L = (I - A)^{-1} $$ 

We also prepare domestic versions of the $A$ matrix.  This starts with using a version of the industry transactions in the Use table only with uses of domestic commodities, also known as the import matrix, $U_m$. 
This has to be conformed to the CS model schema through an aggregation matrix.

$$ U_m = O_c \bar{U_m} O_i' $$

Then we subtracting it from from the Use matrix to estimate a domestic Use table, $U_d$.

$$ U_d = U - U_m $$

Then it can be used to derive the domestic version of the direct requirements, $A_d$.

$$ A_d= U_d\hat{x}^{-1}W $$

We also develop price adjustment factors to adjust for inflation, $\rho$.

The factor are specific to each commodity and year. 

$\rho_y$ is an inflation adjustment factor for commodities in the form of IO based year per given currency year (the columns of the matrix).

$\rho$ is the ratio of the commodity gross output chain price index in IO base year, $by$, to that of year $y$.  

$$ \rho_{y} = \frac{\Pi_{by}}{\Pi_{y}} $$

where $\rho_{y}$ is a vector of inflation factors for every commodity.

The price indices are derived from the BEA Gross Output Chain Price Index, which is a industry by year matrix, $\Pi_i$, of price indices reflecting the annual change in the price of industry gross output relative to the IO year, which is set to 100. 
The commodity price indices are derived from $\Pi_i$ , using the $W$ transformation matrix after $\Pi$ has been conformed to the model schema.

$$ \Pi_i = O_i \bar{\Pi_i} $$

$$ \Pi_i = \Pi_i W $$

The industry and commodity gross output are also estimated for non-IOT years. 
The BEA provides a time-series of industry output that reflects output before redefinitions, \{bar}x.
However, during redefinitions procedure some of industry output, reflecting co-production, is reallocated to the primary industry.
We estimate this reallocation and thus non-IOT years using the following steps.

Differences in co-product output before and after redefinitions are derived as:

$$ V^*_{i,c} = (\bar{V_{i,c}} - V^b_{i,c}) : i \neq c $$

$$ V^*_{i,c} = 0 : i=c $$

where $\bar{V^b}$ is the Make table before redefinitions

We create a matrix $R$ of redefinition ratios

$$ R = \hat{\bar{x}}^-1 V^* $$

Now if we have industry output data for a new year, $\bar{x_y}, we can adjust this for redefinitions using the redefinition ratios.

We first estimate the equivalent of the Make difference in co-product output

$$ V^*_y = \hat{\bar{x_y}} R$$

Then the rows are summed and subtracted which represented removal of the co-product output from the producing industry.

$$ x_y = \bar{x_y} - (V^*_y  i) $$

The the columns are summed to add the new redefined co-product output to the primary industry

$$ x_y = x_y + (i V^*_y) $$

Note the final equation hold when commodity and industry indices of the Make table are identical as the total of the column sum is a commodity total and thus
has to be added to the primary industry.

Commodity output from each non IO year is estimated using the commodity mix matrix, $T$, to get the mix of commodities produced by each industry.

$$ T = \hat{\chi}^{-1} V' \hat{x}^{-1} $$ 

$$ q = T x $$

The estimated industry output for all years is assembled into a $X$ matrix of industry output by year and commodity output into a $Q$ matrix.

# GHG Emissions Model and Indicators

The GHG emissions model is largely based on previously GHG attribution models led by EPA [cite Young and Ingwersen] using the FLOWSA python package [cite Birney].

## Data Sources

Table 4 has a list of the data sources used in the emissions model.

### Table 4. Data Sources Used in GHG Emission Model

| Name                                                       | Creator | Sources | DataYears |
|:-----------------------------------------------------------|:--------|:----------------------------------------------------------------------------------------------------------------------|:----------|
| GHG Inventory | EPA | [ Environmental Protection Agency Inventory of U.S. Greenhouse Gas Emissions and Sinks](https://www.epa.gov/ghgemissions/inventory-us-greenhouse-gas-emissions-and-sinks) | 2023 |
| COA Cropland   | USDA | [Department of Agriculture Census of Agriculture](https://www.eia.gov/consumption/manufacturing/) | 2022 |
| MECS  | EIA | [Energy Information Administration Manufacturing Energy Consumption Survey](https://www.eia.gov/consumption/manufacturing/) | 2022 |

The US GHG Inventory (GHGI) is an authoritative estimate of national GHG emissions and sinks for the U.S. that includes estimates of emissions by broad sectors as specific activities. 

The GHGI is the primary source of all GHG emissions data.
The specific tables from which data are used are included in Table 5. 
Data from these tables were selected because they were determined to be the most relevant to estimate emissions by detailed sector corresponding with the industry data.

### Table 5. GHG Inventory Tables Used
No. | Name
-- | --
2-1 | Recent Trends in U.S. Greenhouse Gas Emissions and Sinks (MMT $CO_2$  Eq.)
3-8 | $CH_4$  Emissions from Stationary Combustion (MMT $CO_2$  Eq.)
3-9 | $N_2O$ Emissions from Stationary Combustion (MMT $CO_2$  Eq.)
3-13 | $CO_2$  Emissions from Fossil Fuel Combustion in Transportation End-Use Sector
3-14 | $CH_4$  Emissions from Mobile Combustion (MMT $CO_2$  Eq.)
3-15 | $N_2O$ Emissions from Mobile Combustion (MMT $CO_2$  Eq.)
3-25 | 2023 Adjusted Non-Energy Use Fossil Fuel Consumption, Storage, and Emissions
3-45 | $CH_4$  Emissions from Petroleum Systems (MMT $CO_2$  Eq.)
3-47 | $CO_2$  Emissions from Petroleum Systems (MMT $CO_2$ )
3-49 | $N_2O$ Emissions from Petroleum Systems (Metric Tons $CO_2$  Eq.)
3-64 | $CH_4$  Emissions from Natural Gas Systems (MMT $CO_2$  Eq.)
3-66 | $CO_2$  Emissions from Natural Gas Systems (MMT)
3-68 | $N_2O$ Emissions from Natural Gas Systems (Metric Tons $CO_2$  Eq.)
4-55 | $CO_2$  and $CH_4$  Emissions from Petrochemical Production (MMT $CO_2$  Eq.)
4-59 | HFC-23 Emissions from HCFC-22 Production (MMT $CO_2$  Eq.)
4-63 | Emissions of HFCs, PFCs, $SF_6$, and NF3 from production of Fluorochemicals Other Than HCFC-22 (MMT $CO_2$  Eq.)
4-64 | Emissions of HFCs, PFCs, $SF_6$, and NF3 from production of Fluorochemicals Other Than HCFC-22 (Metric Tons)
4-106 | $SF_6$, HFC-134a, FK 5-1-12 and $CO_2$  Emissions from Magnesium Production and Processing (MMT $CO_2$  Eq.)
4-100 | PFC Emissions from Aluminum Production (MMT $CO_2$  Eq.)
4-118 | PFC, HFC, $SF_6$, NF3, and $N_2O$ Emissions from Electronics Manufacture (MMT $CO_2$  Eq.)
4-124 | Emissions of HFCs, PFCs, and $CO_2$  from ODS Substitutes (MMT $CO_2$  Eq.) by Sector
4-132 | $SF_6$ and PFC Emissions from Other Product Use (MMT $CO_2$  Eq.)
5-3 | $CH_4$  Emissions from Enteric Fermentation (MMT $CO_2$  Eq.)
5-7 | $CH_4$  and $N_2O$ Emissions from Manure Management (MMT $CO_2$  Eq.)
5-18 | Direct $N_2O$ Emissions from Agricultural Soils by Land Use Type and Nitrogen Input Type (MMT $CO_2$  Eq.)
5-19 | Indirect $N_2O$ Emissions from Agricultural Soils (MMT $CO_2$  Eq.)
5-29 | $CH_4$  and $N_2O$ Emissions from Field Burning of Agricultural Residues (MMT $CO_2$  Eq.)
A-5 | 2023 Energy Consumption Data and $CO_2$  Emissions from Fossil Fuel Combustion by Fuel Type
A-90 | HFC Emissions from Transportation Sources (MMT $CO_2$  Eq.)

## Sector Attribution Model

In the GHGI, U.S. GHG emissions are typically reported by emitting activity and gas.
In some cases, those reporting activities align directly with an industry included in the model.
For example, emissions of $CH_4$ from Landfills (Table 2-1) are assigned directly to 562212, while emissions of $CO_2$ from “Refining” in petroleum systems (Table 3-45) are assigned directly to 324110.
However, in many cases, the reported emissions require a secondary dataset to accurately attribute them to an economic sector.
For example, mobile emissions of $CH_4$ from agricultural equipment (Table 3-14) likely includes emissions from several economic sectors.
In these cases, the potential target sectors are identified (i.e., sectors likely to have agricultural equipment use, 1111A0, 1111B0, 111200, 111300, 111400).
Next a secondary dataset is identified to proportionally attribute the emissions across those sectors in an appropriate way.
For example, purchases made from the "Farm machinery and equipment manufacturing" (333111) sector, as identified in the Use table, are used to attribute emissions from agricultural equipment.
The implicit assumption here is that those sectors that purchase a higher share of agricultural equipment are likely responsible for a higher share of mobile emissions from agricultural equipment.
For consistency with the economic modeling, the After Redefinitions versions of the Make and Use are used for attribution, see discussion [#45](https://github.com/cornerstone-data/methods/discussions/45).
The selection of target sectors and attribution approaches is done in a transparent and modular way to facilitate evaluation and sensitivity to these decisions.

In addition to the Use table, other sources are used to attribute the broader emissions to specific industries when the GHGI does not provide the needed resolution.
The Manufacturing Energy Consumption Survey (MECS) is the summary results of a national survey of energy used by manufacturers in the U.S.
Data used (Table 6) includes fuel use and nonfuel use of energy sources by industry.
The Census of Agriculture (CoA) is the most extensive national survey of agriculture and forestry. From the CoA, various data are used for area of land for general agriculture and specific crop types, both by crop name and NAICS code.
The measures used include "AREA", "AREA IRRIGATED", "AREA HARVESTED", "AREA HARVESTED, IRRIGATED", "AREA IN PRODUCTION, IRRIGATED", "AREA IN PRODUCTION", "AREA BEARING AND NON-BEARING", "AREA BEARING & NON-BEARING, IRRIGATED", "AREA GROWN","AREA GROWN, IRRIGATED", "FARM OPERATIONS", etc. as the measures available per crop type vary.

The approach for building the GHG emissions model generally follows from that described in [cite Young] with some minor updates.
The USGS Mineral Yearbook is dropped as an attribution source as emissions from lead production do not require attribution; primary lead production has not occured in the U.S. for many years (see discussion [#67](https://github.com/cornerstone-data/methods/discussions/67)).
Data from EIA MECS are udpated to 2022 to better align with the temporal scope of the emissions data (see discussion [#68](https://github.com/cornerstone-data/methods/discussions/68)).
Additional adjustments are performed to better align emissions to the target schema, e.g., in transportation (discussion [#39](https://github.com/cornerstone-data/methods/discussions/39)), natural gas (discussion [#34](https://github.com/cornerstone-data/methods/discussions/34)), and electricity (discussion [#33](https://github.com/cornerstone-data/methods/discussions/33)).
Finally, the emissions attribution modeling in FLOWSA is built primarily on a NAICS schema [cite Birney] which can be aggreagated to the necssary [IOT sector schema](#selected-iot-schema).
However to support cleaner aggregations where NAICS and the IOT schema do not align, a hybrid BEA schema is introduced into the emissions modeling, see discussion [#47](https://github.com/cornerstone-data/methods/discussions/47).
In particular, new NAICS-like sector codes are incorporated for the Constrution (23*) and Government (92*) sectors in place of the appropriate NAICS.


### Table 6. MECS Tables Used

No. | Name
-- | --
2-2 | [Energy Used as a Nonfuel (Feedstock)](https://www.eia.gov/consumption/manufacturing/data/2022/xls/Table2_2.xlsx)  By Manufacturing Industry and Region (trillion Btu)
3-2 | [Energy Consumption as a Fuel](https://www.eia.gov/consumption/manufacturing/data/2022/xls/Table3_2.xlsx) By Manufacturing Industry and Region (trillion Btu)

# Creating the Environmentally-Extended IO model

With the economic IOTs, GHG data, and indicators prepared, the data can be combined to form the EEIO model.

## Preparing the GHG Data for use with the IOT
The first step is to create the matrix of direct GHG emissions per commodity, $B$ from the $E$ matrix of GHGs by industry.
This requires some adjustments due to the differences in data years of the IOT and the GHG data and the need to align the data with commodities and not industries. 
The original relation between the environmental data in the form of national totals by industry, $E$, and the model economic data uses the model industry output, as described in the following equation.

$$ B_{I,y} = E_{I,z}\hat{x}^{-1}_{ey,by} $$ 

where $E_I$ is a emission x industry matrix of national totals of each flow by industry sector in emission year $ey$, and $x_{ey,by}$ is a vector of emission year gross output by industry given in base year, $by$, dollars. 
The industries in the $E$ columns match the industries in $x$.

For $x$ to be in year $by$ USD, the unit of the IOTs, $x$ must first be price adjusted using the same price index ratios for industries given above. 
Note that these are the industry price indices rather than the commodity price industries that make up $\rho$. 

$$ x_{ey,by} = x_{ey,ey} \frac{\Pi_{i,by}}{\Pi_{i,ey}} $$ 

The final $B$ matrix is prepared using the following equation:

$$ B = B_i W $$

$B$ is in flow x commodity form after transforming $B_i$ into this form with the transformation matrix.
We also refer to it as a flow coefficient matrix. 

The resulting coefficients from these calculations can be interpreted as a measure of the environmental intensity of a sector in the year the environmental data are reported, but given in terms of the IO year dollar value.

This approach to to prepare the $B$ matrix is the same as that used in USEEIO v2. A discussion of this approach is found in [#16](https://github.com/cornerstone-data/methods/discussions/16).

# Appendix - Cornerstone Schema

Lists of Commodities, Industries, Final Demand and Value Added Components of the Model

## Commodities

Code | Name
-- | --
1111A0 | Fresh soybeans, canola, flaxseeds, and other oilseeds
1111B0 | Fresh wheat, corn, rice, and other grains
111200 | Fresh vegetables, melons, and potatoes
111300 | Fresh fruits and tree nuts
111400 | Greenhouse crops, mushrooms, nurseries, and flowers
111900 | Tobacco, cotton, sugarcane, peanuts, sugar beets, herbs and spices, and   other crops
112120 | Dairies
1121A0 | Cattle ranches and feedlots
112300 | Poultry farms
112A00 | Animal farms and aquaculture ponds (except cattle and poultry)
113000 | Timber and raw forest products
114000 | Wild-caught fish and game
115000 | Agriculture and forestry support
211000 | Unrefined oil and gas
212100 | Coal
212230 | Copper, nickel, lead, and zinc
2122A0 | Iron, gold, silver, and other metal ores
212310 | Dimensional stone
2123A0 | Sand, gravel, clay, phosphate, other nonmetallic minerals
213111 | Well drilling
21311A | Other support activities for mining
221100 | Electricity
221200 | Natural gas
221300 | Drinking water and wastewater treatment
233210 | Health care buildings
233262 | Schools and vocational buildings
230301 | Nonresidential building repair and maintanence
230302 | Residential building repair and maintanence
2332A0 | Commercial structures, including farm structures
233412 | Multifamily homes
2334A0 | Other residential structures
233230 | Manufacturing buildings
2332D0 | Other nonresidential structures
233240 | Utilities buildings and infrastructure
233411 | Single-family homes
2332C0 | Highways, streets, and bridges
321100 | Lumber and treated lumber
321200 | Plywood and veneer
321910 | Wooden windows, door, and flooring
3219A0 | Veneer, plywood, and engineered wood
327100 | Clay and ceramic products
327200 | Glass and glass products
327310 | Cement
327320 | Ready-mix concrete
327330 | Concrete pipe, bricks, and blocks
327390 | Other concrete products
327400 | Lime and gypsum products
327910 | Abrasive products
327991 | Cut stone and stone products
327992 | Ground or treated minerals and earth
327993 | Mineral wool
327999 | Other nonmetallic mineral products
331110 | Primary iron, steel, and ferroalloy products
331200 | Secondary steel products
331313 | Primary aluminum
33131B | Secondary aluminum
331410 | Copper, gold and silver concentrates
331420 | Secondary copper products
331490 | Other secondary nonferrous metal products
331510 | Cast iron and steel
331520 | Nonferrous metal casts
332114 | Custom metal rolls
33211A | All other forging, stamping, and sintering
332119 | Lids, jars, bottle caps, other metal closures and crowns
332200 | Cutlery and handtools
332310 | Metal structural products
332320 | Metal windows, doors, and architectural products
332410 | Power boilers and heat exchangers
332420 | Heavy gauge metal tanks
332430 | Light gauge metal cans, boxes, and containers
332500 | Metal hinges, keys, lock, and other hardware
332600 | Springs and wires
332710 | Machine shops
332720 | Screws, nuts, and bolts
332800 | Metal coatings, engravings, and heat treatments
332913 | Metal plumbing drains, faucets, valves, and other fittings
33291A | Valve and fittings (except for plumbing)
332991 | Ball and roller bearings
332996 | Fabricated pipe and pipe fittings
33299A | Ammunition, arms, ordnance, and related accessories
332999 | Misc. fabricated metal products
333111 | Farm machinery and equipment
333112 | Lawn and garden equipment
333120 | Construction machinery
333130 | Mining and oil/gas field machinery
333242 | Semiconductor machinery
33329A | Machinery for the paper, textile, food or other industries (except   semiconductor machinery)
333314 | Optical instruments and lenses
333316 | Photography and photocopying equipment
333318 | Other commercial and service industry machinery
333414 | Heating equipment other than warm air furnaces
333415 | Air conditioning, refrigeration, and warm air heating equipment
333413 | Air purification and ventilation equipment
333511 | Industrial molds
333514 | Special tools, dies, jigs, and fixtures
333517 | Metal cutting and forming machine tools
33351B | Cutting and machine tool accessory, rolling mill, and other metalworking   machines
333611 | Turbines and turbine generator sets
333612 | Speed changers, industrial high-speed drives, and gears
333613 | Mechanical power transmission equipment
333618 | Other engine equipment
333912 | Air and gas compressors
333914 | Pumps and pumping equipment
333920 | Material handling equipment
333991 | Power tools
333993 | Packaging machinery
333994 | Industrial process furnaces and ovens
33399A | Welding and Soldering Equipment, Scales and Balances, and other general   purpose machinery
33399B | Hydraulic pumps, motors, cylinders and actuators
334111 | Computers
334112 | Computer storage device readers
334118 | Computer terminals and other computer peripheral equipment
334210 | Telephones
334220 | Wireless communications
334290 | Communications equipment
334413 | Semiconductors
334418 | Printed circuit and electronic assembly
33441A | Electronic capacitors, resistors, coils, transformers, connectors and   other components (except    semiconductors and printed circuit assemblies)
334510 | Electromedical appartuses
334511 | Navigation instruments
334512 | Automatic controls for HVAC and refrigeration equipment
334513 | Industrial process variable instruments
334514 | Fluid meters and counting devices
334515 | Signal testing instruments
334516 | Analytical laboratory instruments
334517 | Irradiation apparatuses
33451A | Watches, clocks, and other measuring and controlling devices
334300 | Audio and video equipment
334610 | External hard drives, CDs, other storage media
335110 | Light bulbs
335120 | Light fixtures
335210 | Small electrical appliances
335220 | Major home appliances
335311 | Specialty transformers
335312 | Motors and generators
335313 | Switchgear and switchboards
335314 | Relay and industrial controls
335911 | Storage batteries
335912 | Primary batteries
335920 | Communication and energy wire and cable
335930 | Wiring devices
335991 | Carbon and graphite products
335999 | Other miscellaneous electrical equipment and components
336111 | Automobiles
336112 | Pickup trucks, vans, and SUVs
336120 | Heavy duty trucks
336211 | Vehicle bodies
336212 | Truck trailers
336213 | Motor homes
336214 | Travel trailer and campers
336310 | Vehicle engines and engine parts
336320 | Vehicle electrical and electronic equipment
336350 | Transmission and power train parts
336360 | Vehicle seating and interior trim (upholstery)
336370 | Vehicle metal stamping
336390 | Other vehicle parts
3363A0 | Motor vehicle steering, suspension components (except spring), and brake   systems
336411 | Aircraft
336412 | Aircraft engines and parts
336413 | Other aircraft parts
336414 | Guided missiles and space vehicles
33641A | Propulsion units and parts for space vehicles and guided missiles
336500 | Railroad rolling stock
336611 | Ships and ship repair
336612 | Boats
336991 | Motorcycle, bicycle, and parts
336992 | Military armored vehicles and tanks
336999 | Other transportation equipment
337110 | Wood kitchen cabinets and countertops
337121 | Home furniture - upholstered
337122 | Home furniture - wood, nonupholstered
337127 | Institutional furniture
33712N | Home furniture - Cabinets and non-wood, nonupholstered
337215 | Shelving and lockers
33721A | Office furniture and custom architectural woodwork and millwork
337900 | Mattresses, blinds and shades
339112 | Surgical and medical instruments
339113 | Surgical appliance and supplies
339114 | Dental equipment and supplies
339115 | Ophthalmic goods
339116 | Dental laboratories
339910 | Jewelry and silverware
339920 | Sporting and athletic goods
339930 | Dolls, toys, and games
339940 | Office supplies (not paper)
339950 | Signs
339990 | Gaskets, seals, musical instruments, fasteners, brooms, brushes, mop and   other misc. goods
311111 | Dog and cat food
311119 | Other animal food
311210 | Flours and malts
311221 | Corn products
311225 | Refined vegetable, olive, and seed oils
311224 | Vegetable oils and by-products
311230 | Breakfast cereals
311300 | Sugar, candy, and chocolate
311410 | Frozen food
311420 | Fruit and vegetable preservation
311513 | Cheese
311514 | Dry, condensed, and evaporated dairy
31151A | Fluid milk and butter
311520 | Ice cream and frozen desserts
311615 | Packaged poultry
31161A | Packaged meat (except poultry)
311700 | Seafood
311810 | Bread and other baked goods
3118A0 | Cookies, crackers, pastas, and tortillas
311910 | Snack foods
311920 | Coffee and tea
311930 | Flavored drink concentrates
311940 | Seasonings and dressings
311990 | All other foods
312110 | Soft drinks, bottled water, and ice
312120 | Breweries and beer
312130 | Wineries and wine
312140 | Distilleries and spirits
312200 | Tobacco products
313100 | Fiber, yarn, and thread
313200 | Fabric
313300 | Finished and coated fabric
314110 | Carpets and rugs
314120 | Curtains and linens
314900 | Other textiles
315000 | Clothing
316000 | Leather
322110 | Wood pulp
322120 | Paper
322130 | Cardboard
322210 | Cardboard containers
322220 | Paper bags and coated paper
322230 | Stationery
322291 | Sanitary paper (tissues, napkins, diapers, etc.)
322299 | All other converted paper products
323110 | Books, newspapers, magazines, and other print media
323120 | Printing support
324110 | Gasoline, fuels, and by-products of petroleum refining
324121 | Asphalt pavement
324122 | Asphalt shingles
324190 | Other petroleum and coal products
325110 | Petrochemicals
325120 | Compressed Gases
325130 | Synthetic dyes and pigments
325180 | Other basic inorganic chemicals
325190 | Other basic organic chemicals
325211 | Plastics
3252A0 | Synthetic rubber and artificial and synthetic fibers
325411 | Medicinal and botanical ingredients
325412 | Pharmaceutical products (pills, powders, solutions, etc.)
325413 | Blood sugar, pregnancy, and other diagnostic test kits
325414 | Vaccines and other biological medical products
325310 | Fertilizers
325320 | Pesticides
325510 | Paints and coatings
325520 | Adhesives
325610 | Soap and cleaning compounds
325620 | Toiletries
325910 | Ink and ink cartridges
3259A0 | Chemicals (except basic chemicals, agrichemicals, polymers, paints,   pharmaceuticals,soaps, cleaning compounds)
326110 | Plastic bags, films, and sheets
326120 | Plastic pipe, fittings, and sausage casings
326130 | Laminated plastic plates and shapes
326140 | Polystyrene foam products
326150 | Urethane and other foam products
326160 | Plastic bottles
326190 | Other plastic products
326210 | Rubber tires
326220 | Rubber and plastic belts and hoses
326290 | Other rubber products
423100 | Motor vehicle and motor vehicle parts and supplies
423400 | Professional and commercial equipment and supplies
423600 | Household appliances and electrical and electronic goods
423800 | Machinery, equipment, and supplies
423A00 | Other durable goods merchant wholesalers
424200 | Drugs and druggists sundries
424400 | Grocery and related product wholesalers
424700 | Petroleum and petroleum products
424A00 | Other nondurable goods merchant wholesalers
425000 | Wholesale electronic markets and agents and brokers
4200ID | Customs duties
441000 | Vehicles and parts sales
445000 | Food and beverage stores
452000 | General merchandise stores
444000 | Building material and garden equipment and supplies dealers
446000 | Health and personal care stores
447000 | Gasoline stations
448000 | Clothing and clothing accessories stores
454000 | Nonstore retailers
4B0000 | Other retail
481000 | Air transport
482000 | Rail transport
483000 | Water transport (boats, ships, ferries)
484000 | Truck transport
485000 | Passenger ground transport
486000 | Pipeline transport
48A000 | Scenic and sightseeing transportation and support activities for   transportation
492000 | Couriers and messengers
493000 | Warehousing
511110 | Newspapers
511120 | Magazines and journals
511130 | Books
5111A0 | Directory, mailing list, and other publishers
511200 | Software
512100 | Movies and film
512200 | Sound recording
515100 | Radio and television
515200 | Cable and subscription programming
517110 | Telecommunications
517210 | Wireless telecommunications
517A00 | Satellite, telecommunications resellers, and all other telecommunications
518200 | Data processing and hosting
519130 | Internet publishing and broadcasting
5191A0 | News syndicates, libraries, archives, Internet publishing and all other   information services
522A00 | Nondepository credit intermediation and related activities
52A000 | Monetary authorities and depository credit intermediation
523900 | Investment advice, portfolio management, and other financial advising   services
523A00 | Securities and commodities brokerage and exchanges
524113 | Direct life insurance carriers
5241XX | Insurance carriers, except direct life
524200 | Insurance agencies and brokerages
525000 | Funds, trusts, and financial vehicles
531HSO | Owner-occupied housing
531HST | Tenant-occupied housing
531ORE | Other real estate
532100 | Vehicle rental and leasing
532400 | Commercial equipment rental
532A00 | Consumer goods and general rental centers
533000 | Lessors of nonfinancial intangible assets
541100 | Legal services
541511 | Custom computer programming
541512 | Computer systems design
54151A | Other computer related services, including facilities management
541200 | Accounting, tax preparation, bookkeeping, and payroll
541300 | Architectural, engineering, and related services
541610 | Management consulting
5416A0 | Environmental and other technical consulting services
541700 | Scientific research and development
541800 | Advertising and public relations
541400 | Specialized design
541920 | Photographers
541940 | Veterinarians
5419A0 | Marketing research and all other miscellaneous professional, scientific,   and technical services
550000 | Company and enterprise management
561300 | Employment services
561700 | Buildings and dwellings services
561100 | Office administration
561200 | Facilities support
561400 | Business support
561500 | Travel arrangement and reservation
561600 | Investigation and security
561900 | Other support services
562111 | Solid waste collection
562HAZ | Hazardous waste collection treatment and disposal
562212 | Solid waste landfilling
562213 | Solid waste combustors and incinerators
562910 | Remediation services
562920 | Material separation/recovery facilities
562OTH | Other waste collection and treatment services
611100 | Elementary and secondary schools
611A00 | Colleges, universities, junior colleges, and professional schools
611B00 | Other educational services
621100 | Physicians
621200 | Dentists
621300 | Healthcare practitioners (except physicians and dentists)
621400 | Outpatient healthcare
621500 | Medical laboratories
621600 | Home healthcare
621900 | Ambulances
622000 | Hospitals
623A00 | Nursing and community care facilities
623B00 | Residential mental retardation, mental health, substance abuse and other   facilities
624100 | Individual and family services
624400 | Child day care
624A00 | Community food, housing, and other relief services, including   rehabilitation services
711100 | Performances
711200 | Sports
711500 | Independent artists, writers, and performers
711A00 | Promoters and agents
712000 | Museums, historical sites, zoos, and parks
713100 | Amusement parks and arcades
713200 | Gambling establishments (except casino hotels)
713900 | Golf courses, marinas, ski resorts, fitness and other rec centers and   industries
721000 | Hotels and campgrounds
722110 | Full-service restaurants
722211 | Limited-service restaurants
722A00 | All other food and drinking places
811100 | Vehicle repair
811200 | Electronic  equipment repair and   maintenance
811300 | Commercial machinery repair
811400 | Household goods repair
812100 | Salons and barber shops
812200 | Funerary services
812300 | Dry-cleaning and laundry
812900 | Pet care, photofinishing, parking and other sundry services
813100 | Religious organizations
813A00 | Grantmaking, giving, and social advocacy organizations
813B00 | Civic, social, professional, and similar organizations
814000 | Household employees
S00500 | Federal general government (defense)
S00600 | Federal general government (nondefense)
491000 | Postal service
S00102 | Other federal government enterprises
GSLGE | State and local government educational services
GSLGH | State and local government hospitals and health services
GSLGO | State and local government other services
S00203 | Other state and local government enterprises
S00402 | Used and secondhand goods

## Industries

Code | Name
-- | --
1111A0 | Oilseed farming
1111B0 | Grain farming
111200 | Vegetable and melon farming
111300 | Fruit and tree nut farming
111400 | Greenhouse, nursery, and floriculture production
111900 | Other crop farming
112120 | Dairy cattle and milk production
1121A0 | Beef cattle ranching and farming, including feedlots and dual-purpose   ranching and farming
112300 | Poultry and egg production
112A00 | Animal production, except cattle and poultry and eggs
113000 | Forestry and logging
114000 | Fishing, hunting and trapping
115000 | Support activities for agriculture and forestry
211000 | Oil and gas extraction
212100 | Coal mining
212230 | Copper, nickel, lead, and zinc mining
2122A0 | Iron, gold, silver, and other metal ore mining
212310 | Stone mining and quarrying
2123A0 | Other nonmetallic mineral mining and quarrying
213111 | Drilling oil and gas wells
21311A | Other support activities for mining
221100 | Electric power generation, transmission, and distribution
221200 | Natural gas distribution
221300 | Water, sewage and other systems
233210 | Health care structures
233262 | Educational and vocational structures
230301 | Nonresidential maintenance and repair
230302 | Residential maintenance and repair
2332A0 | Office and commercial structures
233412 | Multifamily residential structures
2334A0 | Other residential structures
233230 | Manufacturing structures
2332D0 | Other nonresidential structures
233240 | Power and communication structures
233411 | Single-family residential structures
2332C0 | Transportation structures and highways and streets
321100 | Sawmills and wood preservation
321200 | Veneer, plywood, and engineered wood product manufacturing
321910 | Millwork
3219A0 | All other wood product manufacturing
327100 | Clay product and refractory manufacturing
327200 | Glass and glass product manufacturing
327310 | Cement manufacturing
327320 | Ready-mix concrete manufacturing
327330 | Concrete pipe, brick, and block manufacturing
327390 | Other concrete product manufacturing
327400 | Lime and gypsum product manufacturing
327910 | Abrasive product manufacturing
327991 | Cut stone and stone product manufacturing
327992 | Ground or treated mineral and earth manufacturing
327993 | Mineral wool manufacturing
327999 | Miscellaneous nonmetallic mineral products
331110 | Iron and steel mills and ferroalloy manufacturing
331200 | Steel product manufacturing from purchased steel
331314 | Secondary smelting and alloying of aluminum
331313 | Alumina refining and primary aluminum production
33131B | Aluminum product manufacturing from purchased aluminum
331410 | Nonferrous metal (except aluminum) smelting and refining
331420 | Copper rolling, drawing, extruding and alloying
331490 | Nonferrous metal (except copper and aluminum) rolling, drawing, extruding   and alloying
331510 | Ferrous metal foundries
331520 | Nonferrous metal foundries
332114 | Custom roll forming
33211A | All other forging, stamping, and sintering
332119 | Metal crown, closure, and other metal stamping (except automotive)
332200 | Cutlery and handtool manufacturing
332310 | Plate work and fabricated structural product manufacturing
332320 | Ornamental and architectural metal products manufacturing
332410 | Power boiler and heat exchanger manufacturing
332420 | Metal tank (heavy gauge) manufacturing
332430 | Metal can, box, and other metal container (light gauge) manufacturing
332500 | Hardware manufacturing
332600 | Spring and wire product manufacturing
332710 | Machine shops
332720 | Turned product and screw, nut, and bolt manufacturing
332800 | Coating, engraving, heat treating and allied activities
332913 | Plumbing fixture fitting and trim manufacturing
33291A | Valve and fittings other than plumbing
332991 | Ball and roller bearing manufacturing
332996 | Fabricated pipe and pipe fitting manufacturing
33299A | Ammunition, arms, ordnance, and accessories manufacturing
332999 | Other fabricated metal manufacturing
333111 | Farm machinery and equipment manufacturing
333112 | Lawn and garden equipment manufacturing
333120 | Construction machinery manufacturing
333130 | Mining and oil and gas field machinery manufacturing
333242 | Semiconductor machinery manufacturing
33329A | Other industrial machinery manufacturing
333314 | Optical instrument and lens manufacturing
333316 | Photographic and photocopying equipment manufacturing
333318 | Other commercial and service industry machinery manufacturing
333414 | Heating equipment (except warm air furnaces) manufacturing
333415 | Air conditioning, refrigeration, and warm air heating equipment   manufacturing
333413 | Industrial and commercial fan and blower and air purification equipment   manufacturing
333511 | Industrial mold manufacturing
333514 | Special tool, die, jig, and fixture manufacturing
333517 | Machine tool manufacturing
33351B | Cutting and machine tool accessory, rolling mill, and other metalworking   machinery manufacturing
333611 | Turbine and turbine generator set units manufacturing
333612 | Speed changer, industrial high-speed drive, and gear manufacturing
333613 | Mechanical power transmission equipment manufacturing
333618 | Other engine equipment manufacturing
333912 | Air and gas compressor manufacturing
333914 | Measuring, dispensing, and other pumping equipment manufacturing
333920 | Material handling equipment manufacturing
333991 | Power-driven handtool manufacturing
333993 | Packaging machinery manufacturing
333994 | Industrial process furnace and oven manufacturing
33399A | Other general purpose machinery manufacturing
33399B | Fluid power process machinery
334111 | Electronic computer manufacturing
334112 | Computer storage device manufacturing
334118 | Computer terminals and other computer peripheral equipment manufacturing
334210 | Telephone apparatus manufacturing
334220 | Broadcast and wireless communications equipment
334290 | Other communications equipment manufacturing
334413 | Semiconductor and related device manufacturing
334418 | Printed circuit assembly (electronic assembly) manufacturing
33441A | Other electronic component manufacturing
334510 | Electromedical and electrotherapeutic apparatus manufacturing
334511 | Search, detection, and navigation instruments manufacturing
334512 | Automatic environmental control manufacturing
334513 | Industrial process variable instruments manufacturing
334514 | Totalizing fluid meter and counting device manufacturing
334515 | Electricity and signal testing instruments manufacturing
334516 | Analytical laboratory instrument manufacturing
334517 | Irradiation apparatus manufacturing
33451A | Watch, clock, and other measuring and controlling device manufacturing
334300 | Audio and video equipment manufacturing
334610 | Manufacturing and reproducing magnetic and optical media
335110 | Electric lamp bulb and part manufacturing
335120 | Lighting fixture manufacturing
335210 | Small electrical appliance manufacturing
335220 | Major household appliance manufacturing
335311 | Power, distribution, and specialty transformer manufacturing
335312 | Motor and generator manufacturing
335313 | Switchgear and switchboard apparatus manufacturing
335314 | Relay and industrial control manufacturing
335911 | Storage battery manufacturing
335912 | Primary battery manufacturing
335920 | Communication and energy wire and cable manufacturing
335930 | Wiring device manufacturing
335991 | Carbon and graphite product manufacturing
335999 | All other miscellaneous electrical equipment and component manufacturing
336111 | Automobile manufacturing
336112 | Light truck and utility vehicle manufacturing
336120 | Heavy duty truck manufacturing
336211 | Motor vehicle body manufacturing
336212 | Truck trailer manufacturing
336213 | Motor home manufacturing
336214 | Travel trailer and camper manufacturing
336310 | Motor vehicle gasoline engine and engine parts manufacturing
336320 | Motor vehicle electrical and electronic equipment manufacturing
336350 | Motor vehicle transmission and power train parts manufacturing
336360 | Motor vehicle seating and interior trim manufacturing
336370 | Motor vehicle metal stamping
336390 | Other motor vehicle parts manufacturing
3363A0 | Motor vehicle steering, suspension component (except spring), and brake   systems manufacturing
336411 | Aircraft manufacturing
336412 | Aircraft engine and engine parts manufacturing
336413 | Other aircraft parts and auxiliary equipment manufacturing
336414 | Guided missile and space vehicle manufacturing
33641A | Propulsion units and parts for space vehicles and guided missiles
336500 | Railroad rolling stock manufacturing
336611 | Ship building and repairing
336612 | Boat building
336991 | Motorcycle, bicycle, and parts manufacturing
336992 | Military armored vehicle, tank, and tank component manufacturing
336999 | All other transportation equipment manufacturing
337110 | Wood kitchen cabinet and countertop manufacturing
337121 | Upholstered household furniture manufacturing
337122 | Nonupholstered wood household furniture manufacturing
337127 | Institutional furniture manufacturing
33712N | Other household nonupholstered furniture
337215 | Showcase, partition, shelving, and locker manufacturing
33721A | Office furniture and custom architectural woodwork and millwork   manufacturing
337900 | Other furniture related product manufacturing
339112 | Surgical and medical instrument manufacturing
339113 | Surgical appliance and supplies manufacturing
339114 | Dental equipment and supplies manufacturing
339115 | Ophthalmic goods manufacturing
339116 | Dental laboratories
339910 | Jewelry and silverware manufacturing
339920 | Sporting and athletic goods manufacturing
339930 | Doll, toy, and game manufacturing
339940 | Office supplies (except paper) manufacturing
339950 | Sign manufacturing
339990 | All other miscellaneous manufacturing
311111 | Dog and cat food manufacturing
311119 | Other animal food manufacturing
311210 | Flour milling and malt manufacturing
311221 | Wet corn milling
311225 | Fats and oils refining and blending
311224 | Soybean and other oilseed processing
311230 | Breakfast cereal manufacturing
311300 | Sugar and confectionery product manufacturing
311410 | Frozen food manufacturing
311420 | Fruit and vegetable canning, pickling, and drying
311513 | Cheese manufacturing
311514 | Dry, condensed, and evaporated dairy product manufacturing
31151A | Fluid milk and butter manufacturing
311520 | Ice cream and frozen dessert manufacturing
311615 | Poultry processing
31161A | Animal (except poultry) slaughtering, rendering, and processing
311700 | Seafood product preparation and packaging
311810 | Bread and bakery product manufacturing
3118A0 | Cookie, cracker, pasta, and tortilla manufacturing
311910 | Snack food manufacturing
311920 | Coffee and tea manufacturing
311930 | Flavoring syrup and concentrate manufacturing
311940 | Seasoning and dressing manufacturing
311990 | All other food manufacturing
312110 | Soft drink and ice manufacturing
312120 | Breweries
312130 | Wineries
312140 | Distilleries
312200 | Tobacco manufacturing
313100 | Fiber, yarn, and thread mills
313200 | Fabric mills
313300 | Textile and fabric finishing and fabric coating mills
314110 | Carpet and rug mills
314120 | Curtain and linen mills
314900 | Other textile product mills
315000 | Apparel manufacturing
316000 | Leather and allied product manufacturing
322110 | Pulp mills
322120 | Paper mills
322130 | Paperboard mills
322210 | Paperboard container manufacturing
322220 | Paper bag and coated and treated paper manufacturing
322230 | Stationery product manufacturing
322291 | Sanitary paper product manufacturing
322299 | All other converted paper product manufacturing
323110 | Printing
323120 | Support activities for printing
324110 | Petroleum refineries
324121 | Asphalt paving mixture and block manufacturing
324122 | Asphalt shingle and coating materials manufacturing
324190 | Other petroleum and coal products manufacturing
325110 | Petrochemical manufacturing
325120 | Industrial gas manufacturing
325130 | Synthetic dye and pigment manufacturing
325180 | Other basic inorganic chemical manufacturing
325190 | Other basic organic chemical manufacturing
325211 | Plastics material and resin manufacturing
3252A0 | Synthetic rubber and artificial and synthetic fibers and filaments   manufacturing
325411 | Medicinal and botanical manufacturing
325412 | Pharmaceutical preparation manufacturing
325413 | In-vitro diagnostic substance manufacturing
325414 | Biological product (except diagnostic) manufacturing
325310 | Fertilizer manufacturing
325320 | Pesticide and other agricultural chemical manufacturing
325510 | Paint and coating manufacturing
325520 | Adhesive manufacturing
325610 | Soap and cleaning compound manufacturing
325620 | Toilet preparation manufacturing
325910 | Printing ink manufacturing
3259A0 | All other chemical product and preparation manufacturing
326110 | Plastics packaging materials and unlaminated film and sheet manufacturing
326120 | Plastics pipe, pipe fitting, and unlaminated profile shape manufacturing
326130 | Laminated plastics plate, sheet (except packaging), and shape   manufacturing
326140 | Polystyrene foam product manufacturing
326150 | Urethane and other foam product (except polystyrene) manufacturing
326160 | Plastics bottle manufacturing
326190 | Other plastics product manufacturing
326210 | Tire manufacturing
326220 | Rubber and plastics hoses and belting manufacturing
326290 | Other rubber product manufacturing
423100 | Motor vehicle and motor vehicle parts and supplies
423400 | Professional and commercial equipment and supplies
423600 | Household appliances and electrical and electronic goods
423800 | Machinery, equipment, and supplies
423A00 | Other durable goods merchant wholesalers
424200 | Drugs and druggists' sundries
424400 | Grocery and related product wholesalers
424700 | Petroleum and petroleum products
424A00 | Other nondurable goods merchant wholesalers
425000 | Wholesale electronic markets and agents and brokers
4200ID | Customs duties
441000 | Motor vehicle and parts dealers
445000 | Food and beverage stores
452000 | General merchandise stores
444000 | Building material and garden equipment and supplies dealers
446000 | Health and personal care stores
447000 | Gasoline stations
448000 | Clothing and clothing accessories stores
454000 | Nonstore retailers
4B0000 | All other retail
481000 | Air transportation
482000 | Rail transportation
483000 | Water transportation
484000 | Truck transportation
485000 | Transit and ground passenger transportation
486000 | Pipeline transportation
48A000 | Scenic and sightseeing transportation and support activities
492000 | Couriers and messengers
493000 | Warehousing and storage
511110 | Newspaper publishers
511120 | Periodical Publishers
511130 | Book publishers
5111A0 | Directory, mailing list, and other publishers
511200 | Software publishers
512100 | Motion picture and video industries
512200 | Sound recording industries
515100 | Radio and television broadcasting
515200 | Cable and other subscription programming
517110 | Wired telecommunications carriers
517210 | Wireless telecommunications carriers (except satellite)
517A00 | Satellite, telecommunications resellers, and all other telecommunications
518200 | Data processing, hosting, and related services
519130 | Internet publishing and broadcasting and Web search portals
5191A0 | News syndicates, libraries, archives and all other information services
522A00 | Nondepository credit intermediation and related activities
52A000 | Monetary authorities and depository credit intermediation
523900 | Other financial investment activities
523A00 | Securities and commodity contracts intermediation and brokerage
524113 | Direct life insurance carriers
5241XX | Insurance carriers, except direct life
524200 | Insurance agencies, brokerages, and related activities
525000 | Funds, trusts, and other financial vehicles
531HSO | Owner-occupied housing
531HST | Tenant-occupied housing
531ORE | Other real estate
532100 | Automotive equipment rental and leasing
532400 | Commercial and industrial machinery and equipment rental and leasing
532A00 | General and consumer goods rental
533000 | Lessors of nonfinancial intangible assets
541100 | Legal services
541511 | Custom computer programming services
541512 | Computer systems design services
54151A | Other computer related services, including facilities management
541200 | Accounting, tax preparation, bookkeeping, and payroll services
541300 | Architectural, engineering, and related services
541610 | Management consulting services
5416A0 | Environmental and other technical consulting services
541700 | Scientific research and development services
541800 | Advertising, public relations, and related services
541400 | Specialized design services
541920 | Photographic services
541940 | Veterinary services
5419A0 | All other miscellaneous professional, scientific, and technical services
550000 | Management of companies and enterprises
561300 | Employment services
561700 | Services to buildings and dwellings
561100 | Office administrative services
561200 | Facilities support services
561400 | Business support services
561500 | Travel arrangement and reservation services
561600 | Investigation and security services
561900 | Other support services
562111 | Solid waste collection
562HAZ | Hazardous waste collection treatment and disposal
562212 | Solid waste landfilling
562213 | Solid waste combustors and incinerators
562910 | Remediation services
562920 | Material separation/recovery facilities
562OTH | Other waste collection and treatment services
611100 | Elementary and secondary schools
611A00 | Junior colleges, colleges, universities, and professional schools
611B00 | Other educational services
621100 | Offices of physicians
621200 | Offices of dentists
621300 | Offices of other health practitioners
621400 | Outpatient care centers
621500 | Medical and diagnostic laboratories
621600 | Home health care services
621900 | Other ambulatory health care services
622000 | Hospitals
623A00 | Nursing and community care facilities
623B00 | Residential mental health, substance abuse, and other residential care   facilities
624100 | Individual and family services
624400 | Child day care services
624A00 | Community food, housing, and other relief services, including vocational   rehabilitation services
711100 | Performing arts companies
711200 | Spectator sports
711500 | Independent artists, writers, and performers
711A00 | Promoters of performing arts and sports and agents for public figures
712000 | Museums, historical sites, zoos, and parks
713100 | Amusement parks and arcades
713200 | Gambling industries (except casino hotels)
713900 | Other amusement and recreation industries
721000 | Accommodation
722110 | Full-service restaurants
722211 | Limited-service restaurants
722A00 | All other food and drinking places
811100 | Automotive repair and maintenance (including car washes)
811200 | Electronic and precision equipment repair and maintenance
811300 | Commercial and industrial machinery and equipment repair and maintenance
811400 | Personal and household goods repair and maintenance
812100 | Personal care services
812200 | Death care services
812300 | Dry-cleaning and laundry services
812900 | Other personal services
813100 | Religious organizations
813A00 | Grantmaking, giving, and social advocacy organizations
813B00 | Civic, social, professional, and similar organizations
814000 | Private households
S00500 | Federal general government (defense)
S00600 | Federal general government (nondefense)
491000 | Postal service
S00102 | Other federal government enterprises
GSLGE | State and local government (educational services)
GSLGH | State and local government (hospitals and health services)
GSLGO | State and local government (other services)
S00203 | Other state and local government enterprises

## Final Demand Components

Code | Name | Group
-- | -- | --
F01000 | Personal consumption expenditures | Household
F02E00 | Nonresidential private fixed investment in equipment | Investment
F02N00 | Nonresidential private fixed investment in intellectual property products | Investment
F02R00 | Residential private fixed investment | Investment
F02S00 | Nonresidential private fixed investment in structures | Investment
F03000 | Change in private inventories | ChangeInventories
F04000 | Exports of goods and services | Export
F05000 | Imports of goods and services | Import
F06C00 | Federal Government defense: Consumption expenditures | Government
F06E00 | Federal national defense: Gross investment in equipment | Government
F06N00 | Federal national defense: Gross investment in intellectual property   products | Government
F06S00 | Federal national defense: Gross investment in structures | Government
F07C00 | Federal Government nondefense: Consumption expenditures | Government
F07E00 | Federal nondefense: Gross investment in equipment | Government
F07N00 | Federal nondefense: Gross investment in intellectual property products | Government
F07S00 | Federal nondefense: Gross investment in structures | Government
F10C00 | State and local government consumption expenditures | Government
F10E00 | State and local: Gross investment in equipment | Government
F10N00 | State and local: Gross investment in intellectual property products | Government
F10S00 | State and local: Gross investment in structures | Government

## Value Added Components

Code | Name
-- | --
V00100 | Compensation of employees
V00200 | Taxes on production and imports, less subsidies
V00300 | Gross operating surplus





