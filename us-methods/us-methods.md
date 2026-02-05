
# Methodology for Cornerstone U.S. National Model 

authors, TBD

This paper describes the methodological approach to building the detailed United States national environmentally-extended input-output (EEIO) model that is to be integrated into the Cornerstone (CS) global EEIO model, v1.0.
This methodology draws on approaches used previously in the the USEEI0 and CEDA-US models. 
The Cornerstone team first reviewed and identified all the differences between reference USEEIO and CEDA-US models, and the systematically assessed the differences both theoretically and quantitatively, as relevant. 
That work is captured in a discussions and related scripts found in the [cornerstone-data/methods](https://github.com/cornerstone-data/methods) repository. References to those discussions are provided herein.
This paper synthesizes those discussions into a consolidated and harmonious description of the selected methods.
The description of the methodology is split into three sections: (1) the economic data and steps used to forming the basis economic input-output table (IOT) matrices and associated economic datasets;
(2) the greenhouse gas emissions (GHG) and indicator datasets and the approach used to estimate industry GHG totals; and (3) the integration of the economic and environmental data to form model result matrices of direct and indirect GHG emissions per dollar. 

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
| Margins                                                    | BEA | [Bureau of Economic Analysis Industry Underlying Estimates](https://www.bea.gov/industry/industry-underlying-estimates) | 2017 |
| Import Matrix, After Redefinitions                         | BEA | [Bureau of Economic Analysis Industry Input-Output Accounts](https://www.bea.gov/industry/input-output-accounts-data) | 2017 |
| Summary Statistics for  Waste Sector (EC1756BASIC)                                              | Census Bureau | [2017 Administrative and Support and Waste Management and Remediation Services (NAICS Sector 56)](https://www.census.gov/data/tables/2017/econ/economic-census/naics-sector-56.html) | 2017 |
| Services Annual Survey (SAS)                                              | Census Bureau |  | 2017 |
| RCRA Online                                             |  EPA |  | 2012 |

The benchmark Make and Use matrices from the BEA, provided at 5 year intervals, were the fundamental US IOT inputs to USEEIO and CEDA models.
The 2017 tables are the most recent official release.
These tables are prepared in producer and purchaser price, as well as "Before redefinitions" (BR) and "After Redefinitions" (AR).
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
The Margins table contain data on cost of transportation, wholesale and retail to move each commodity to each user corresponding with the benchmark Use table.
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
| **A_d**         | domestic direct requirements matrix                                                    | commodity x commodity |
| **B**           | direct environmental flows per commodity output matrix                                 | flow x commodity      |
| **C**           | characterization factor matrix                                                         | indicator x flow      |
| c               | subscript for commodity                                                                |                       |    
| **i**           | vector of 1s                                                                           | varies                |
| **L**           | Leontief matrix                                                                        | commodity x commodity |
| **L_d**         | Leontief matrix only including total domestic requirements                             | commodity x commodity |
| **M**           | direct + indirect flows matrix, aka multipliers                                        | flow x commodity      |
| **q**           | commodity output vector                                                                | commodities           |
| **U**           | Use table industry transactions | commodity x industry |
| **V**           | Make table                                                                             | industry x commodity |
| **x**           | total final demand vector                                                              | commodities           |
| **y**           | total final demand vector                                                              | commodities           |
| ^               | symbol that indicates diagonalized form (matrix form) of a vector                      |                       |
| '               | symbol that indicates the transposition of a matrix of vector                      |                       |
| -               | bar over to represent a variable before transformation into CS conventions         |                       |
| $\circ$         | open dot for a Hadamard product (elementwise) of two matrices or vectors               |                       |

We start with the Make table from BEA, $\bar{V}$, and the Use table from BEA, $\bar{U}$.  
To combine the government industries we use an industry x industry correspondence matrices, $O_i$, and a commodity by commodity correspondence matrix, $O_c$, to aggregate the sector in the Make (the rows) and Use. 
In these correspondence matrices the CS sectors are on the rows and the BEA sectors on the columns. 
This removes the rows and columns of special accounting industries and commodities from the matrices, and aggregates the government utilities. 
Note that these correspondence matrices do not account for the additions of the waste sectors.

$$ V = O_i \bar{V} O_c' $$

$$ U = O_c \bar{U} O_i' $$

The total commodity and industry output, $q$ and $x$, is derived from the Make table, where $q$ is the columns sums and $x$ is the row sums.
Although the commodity, _Scrap_, is not part of the final sector schema, it does need to be included in order to adjust the IOT.  
The scrap amounts produced by each industry, $r$, must be added to get the industry output.   

$$ q = iV $$

$$ x = Vi + r $$

We calculate the non-scrap portion of industry output, $nsr$. 

$$ nsr = (x-r){x}^-1 $$

This ratio is used to modify the commodity output normalized form of the Make matrix, also knows as the market shares matrix, to reflect an increased input of non-scrap commodities needed to fill the gap left from scrap removal in industry output. This new matrix is $W$.

$$ W = \hat{nsr}^{-1} V\hat{q}^{-1} $$  

The Use table is normalized by industry output and then post-multiplied by $W$ to get $A$ in commodity by commodity format.
This method for creating the $A$ matrix is based on the _industry- technology_ assumption, wherein the manufacture of the primary and any secondary commodities by an industry uses the same production requirements, and the commodity requirements are based therefore on the mix of industries that produce that commodity, weighted by their relative share of total commodity output. See [Input Output Analysis by R. Miller and P. Blair 2022](https://doi.org/10.1017/9781108676212).

$$ A = U\hat{x}^{-1}W $$

$L$, the Leontief inverse, or the total requirements matrix, is obtained from $A$.
$L$ is in commodity x commodity form and represents the total upstream inputs of commodities (rows) used to make a commodity (columns).

$$
L = (I - A)^{-1}
$$ {#eq:L}

We also prepare domestic versions of the $A$ matrix.  This starts with using a version of the industry transactions in the Use table only with uses of domestic commodities, also known as the import matrix, $U_m$. 
This has to be conformed to the CS model schema through an aggregation matrix, then we subtracting it from from the Use matrix to estimate a domestic Use table, $U_d$, as in [@Eq:U_d].

$$ U_m = O_c \bar{U_m} O_i' $$

$$ U_d = U - U_m $$

Then it can be used to derive the domestic version of the direct requirements, $A_d$.

$$ A_d= U_d\hat{x}^{-1}W $$

We also develop two types of price adjustment factors, one to adjust for inflation, $\rho$, and one to adjust from producer price to purchaser price, $\Phi$.

The factor are specific to each commodity and year. 

$\rho_y$ is an inflation adjustment factor for commodities in the form of IO based year per given currency year (the columns of the matrix).

$\rho$ is the ratio of the commodity gross output chain price index in IO base year, $by$, to that of year $y$.  

$$ \rho_{y} = \frac{\Pi_{by}}{\Pi_{y}} $$

where $\rho_{y}$ is a vector of inflation factors for every commodity.

The price indices are derived from the BEA Gross Output Chain Price Index, which is a industry by year matrix, $\Pi_i$, of price indices reflecting the annual change in the price of industry gross output relative to the IO year, which is set to 100. 
The commodity price indices are derived from $\Pi_i$ , using the $\W$ transformation matrix after $\Pi$ has been conformed to the model schema.

$$ \Pi_i = O_i \bar{\Pi_i} $$

$$ \Pi_i = \Pi_i W $$

The second of price adjustment matrix is $\Phi$, which is composed of factors to convert into purchaser price. 
Purchaser price reflects the producer's price plus sale and transportation margins [see BEA IO manual](https://www.bea.gov/resources/methodologies/concepts-methods-io-accounts).

 The values in $\Phi$ are commodity-specific producer:purchaser price ratios where a value of $\Phi_{c,y}$ is a ratio of commodity output, $q$ in producer price for a given year to commodity output in purchaser price for that given year. 
 
$$ \Phi_c,y = \frac{q_{PRO,c,by}}{q_{PUR,c,y}} $$ 

The origins of $\Phi$ is the Margins tables provide values for transportation, $t$, wholesale, $w$, and retail, $r$, specific to each commodity consumed by industries, households or investors. These same year margins data are used for all years, however they are price-adjusted first to calculate a total year specific margin

$$ q_{PUR,c,y} = q_cP_{c,y} + t_{c,y}P_{t,y} + w_{c,y}P_{w,y} + r_{c,y}P_{r,y} $$

where $P_{m,y}$ for a margin type ($t$, $w$ or $r$) is calculated in [@Eq:Rho_m], which is the weighted average of price adjustments in commodities that make up $m$ (e.g. Truck transportation, Water transportation, Rail transportation are commodities in set $m$ for transportation).

$$ P_{m,y} = \frac{\sum_{c\in m}q_{c,y}P_{c,y}} {\sum_{c\in m}q_{c,y}} $$


# GHG Emissions Model and Indicators

## Data Sources

Table 4 has a list of the data sources used in the emissions model.

### Table 4. Data Sources Used in GHG Emission Model
| Name                                                       | Creator | Sources | DataYears |
|:-----------------------------------------------------------|:--------|:----------------------------------------------------------------------------------------------------------------------|:----------|
| GHG Inventory | EPA | [ Environmental Protection Agency Inventory of U.S. Greenhouse Gas Emissions and Sinks](https://www.epa.gov/ghgemissions/inventory-us-greenhouse-gas-emissions-and-sinks) | 2023 |
| COA Cropland   | USDA | [Department of Agriculture Census of Agriculture](https://www.eia.gov/consumption/manufacturing/) | 2022 |
| MECS  | EIA | [Energy Information Administration Manufacturing Energy Consumption Survey ](https://www.eia.gov/consumption/manufacturing/) | 2018 |
| MYB: Lead  | USGS | [U.S. Geological Survey Mineral Yearbook](https://www.usgs.gov/centers/national-minerals-information-center/lead-statistics-and-information) | 2020 |

The US GHG Inventory (GHGI) is an authoritative estimate of national GHG emissions and sinks for the U.S. that includes estimates of emissions by broad sectors as specific activities. 

The GHGI is the primary source of all GHG emissions data.
The specific tables from which data are used are included in Table 5. 
Data from these tables were selected because they were determined to be the most relevant to estimate emissions by detailed sector corresponding with the industry data.

Other sources are used to attribute the broader emissions to specific industries when the GHGI does not provide the needed resolution. 
The Manufacturing Energy Consumption Survey (MECS) is the summary results of a national survey of energy used by manufacturers in the U.S. 
Data used (Table 6) includes total energy consumption by industry as well as fuel use and nonfuel use of energy sources and average prices for energy. 
The Census of Agriculture (CoA) is the most extensive national survey of agriculture and forestry. From the CoA, various data are used for area of land for general agriculture and specific crop types, both by crop name and NAICS code. The measures used include "AREA", "AREA IRRIGATED", "AREA HARVESTED", "AREA HARVESTED, IRRIGATED", "AREA IN PRODUCTION, IRRIGATED", "AREA IN PRODUCTION" , "AREA BEARING AND NON-BEARING", "AREA BEARING & NON-BEARING, IRRIGATED", "AREA GROWN","AREA GROWN, IRRIGATED", "FARM OPERATIONS", ETC. as the measures available per crop type vary. 
From the Mineral Yearbook (MYB), data on secondary (recycled) lead production is used. 

### Table 5. GHG Inventory Tables Used
No. | Name
-- | --
2-1 | Recent Trends in U.S. Greenhouse Gas Emissions and Sinks (MMT CO2 Eq.)
3-13 | CO2 Emissions from Fossil Fuel Combustion in Transportation End-Use Sector
A-5 | 2023 Energy Consumption Data and CO2 Emissions from Fossil Fuel Combustion by Fuel Type
5-3 | CH4 Emissions from Enteric Fermentation (MMT CO2 Eq.)
3-64 | CH4 Emissions from Natural Gas Systems (MMT CO2 Eq.)
5-18 | Direct N2O Emissions from Agricultural Soils by Land Use Type and Nitrogen Input Type (MMT CO2 Eq.)
4-124 | Emissions of HFCs, PFCs, and CO2 from ODS Substitutes (MMT CO2 Eq.) by Sector
3-25 | 2023 Adjusted Non-Energy Use Fossil Fuel Consumption, Storage, and Emissions
5-7 | CH4 and N2O Emissions from Manure Management (MMT CO2 Eq.)
3-45 | CH4 Emissions from Petroleum Systems (MMT CO2 Eq.)
3-66 | CO2 Emissions from Natural Gas Systems (MMT)
A-90 | HFC Emissions from Transportation Sources (MMT CO2 Eq.)
4-55 | CO2 and CH4 Emissions from Petrochemical Production (MMT CO2 Eq.)
5-19 | Indirect N2O Emissions from Agricultural Soils (MMT CO2 Eq.)
3-47 | CO2 Emissions from Petroleum Systems (MMT CO2)
3-9 | N2O Emissions from Stationary Combustion (MMT CO2 Eq.)
3-15 | N2O Emissions from Mobile Combustion (MMT CO2 Eq.)
4-118 | PFC, HFC, SF6, NF3, and N2O Emissions from Electronics Manufacture (MMT CO2 Eq.)
3-8 | CH4 Emissions from Stationary Combustion (MMT CO2 Eq.)
4-64 | Emissions of HFCs, PFCs, SF6, and NF3 from production of Fluorochemicals Other Than HCFC-22 (Metric Tons)
3-14 | CH4 Emissions from Mobile Combustion (MMT CO2 Eq.)
4-106 | SF6, HFC-134a, FK 5-1-12 and CO2 Emissions from Magnesium Production and Processing (MMT CO2 Eq.)
4-132 | SF6 and PFC Emissions from Other Product Use (MMT CO2 Eq.)
4-63 | Emissions of HFCs, PFCs, SF6, and NF3 from production of Fluorochemicals Other Than HCFC-22 (MMT CO2 Eq.)
4-59 | HFC-23 Emissions from HCFC-22 Production (MMT CO2 Eq.)
4-100 | PFC Emissions from Aluminum Production (MMT CO2 Eq.)
5-29 | CH4 and N2O Emissions from Field Burning of Agricultural Residues (MMT CO2 Eq.)
3-49 | N2O Emissions from Petroleum Systems (Metric Tons CO2 Eq.)
3-68 | N2O Emissions from Natural Gas Systems (Metric Tons CO2 Eq.)

### Table 6. MECS Tables Used
No. | Name
-- | --
1-2 | [Consumption of Energy for All Purposes] By Manufacturing Industry and Region (trillion Btu)
1-5 | [Consumption of Energy for All Purposes]  By Further Classification of 'Other' Energy Sources
2-1 | [Energy Used as a Nonfuel (Feedstock)] By Manufacturing Industry and Region (physical units)
2-2 | [Energy Used as a Nonfuel (Feedstock)]  By Manufacturing Industry and Region (trillion Btu)
3-1 | [Energy Consumption as a Fuel] By Manufacturing Industry and Region (physical units)
3-2 | [Energy Consumption as a Fuel] By Manufacturing Industry and Region (trillion Btu)
7-2 | [Average Prices of Purchased Energy Sources] All Collected Energy Sources By Manufacturing Industry and Region (dollars per million Btu)
7-10 | [Expenditures for Purchased Energy (Millions of Dollars)] Purchased Electricity, Natural Gas, Steam by type of Supplier, Manufacturing, Industry, and Region

## Sector Attribution Model



## Global Warming Potential Indicator
The SAM provides data for emissions of individual gas (e.g. methane) by industries (with one exception listed above).  
These gases each have different potential to cause radiative forcing which is the primary way in which these gases cause global warming.
Global warming potentials (GWPs) are factors that relate GHGs to the global warming potential they have over a given time frame by estimating this potential in kg of CO2 equivalent (CO2e). 
We use the most GWPs for a 100 yr time horizon from the IPCC 6th Assessment Report (AR6) that are matched to the FEDEFL flows from the [IPCC AR4, AR5, and AR6 20-, 100-, and 500-year GWPs](https://doi.org/10.23719/1529821) dataset. 
These GWPs become the indicator used in the model.
We conform these into a vector, $c$, of GWPs per gas.

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

A series of coefficient matrices are provided that are products of combining more than one of the economic, physical flow, and indicator components. 
The direct impacts of a sector in a given indicator unit per model dollar year, can be calculated with the following equation.
$d$ is an indicator x sector matrix and contains in each column the direct CO2e result per 1 USD output of the sector, also known as an impact result.

$$ d = cB $$ 

With the flow coefficient matrix $B$ and the total requirements matrix $L$, the matrix $M$ which contains the direct and indirect flow coefficients can be calculated with the following equation. 

$$ M = BL $$ 

The matrix $M$ is a flow x sector matrix and contains in each row the direct plus indirect flows per 1 USD output of the sector in column $by$ dollars in producer price.

With the direct impacts $d$ and the total requirements $L$, the matrix $N$ which contains the direct plus indirect impact coefficients can be calculated via the following equation. 

$$ n = dL $$ 

$n$ combines each economic, flow and indicator component. $n$ is an indicator x sector vector and contains the direct and indirect impact result per 1 USD output of commodity in $by$ dollars in producer price.


