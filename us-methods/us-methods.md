
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
| -               | bar over to represent a variable before transformation into CS conventions         |                       |
| $\circ$         | open dot for a Hadamard product (elementwise) of two matrices or vectors               |                       |

We start with the Make table from BEA, $\bar{V}$, and the Use table, $\bar{U}$.  
To combine the government industries we use an industry x industry correspondence matrices, $O_i$, and a commodity by commodity correspondence matrix, $O_c$, to aggregate the sector in the Make (the rows) and Use. 
In these correspondence matrices the CS sectors are on the rows and the BEA sectors on the columns. 
This removes the rows and columns of special accounting industries and commodities from the matrices, and aggregates the government utilities. 
Note that this does not account for the additions of the waste sectors.

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

$$
U_d = U - U_m
$$ {#eq:U_d}

Then it can be used to derive the domestic version of the direct requirements, $A_d$.

$$ A_d= U_d\hat{x}^{-1}W $$

We also develop two types of price adjustment factors, one to adjust for inflation, $\rho$, and one to adjust from producer price to purchaser price, $\Phi$.

The factor are specific to each commodity and year. 

$\rho_y$ is an inflation adjustment factor for commodities in the form of IO based year per given currency year (the columns of the matrix).

$\rho$ is the ratio of the commodity gross output chain price index in IO base year, $by$, to that of year $y$.  

$$
\rho_{y} = \frac{\Pi_{by}}{\Pi_{y}}
$$ {#eq:rho}

where $\rho_{y}$ is a vector of inflation factors for every commodity.

The price indices are derived from the BEA Gross Output Chain Price Index, which is a industry by year matrix, $\Pi_i$, of price indices reflecting the annual change in the price of industry gross output relative to the IO year, which is set to 100. 
The commodity price indices are derived from $\Pi_i$ , using the $\W$ transformation matrix after $\Pi$ has been conformed to the model schema.

$$ \Pi_i = O_i \bar{\Pi_i} $$

$$ \Pi_i = \Pi_i W $$


The second of price adjustment matrix is $\Phi$, which is composed of factors to convert into purchaser price. 
Purchaser price reflects the producer's price plus sale and transportation margins [see BEA IO manual](https://www.bea.gov/resources/methodologies/concepts-methods-io-accounts).

 The values in $\Phi$ are commodity-specific producer:purchaser price ratios where a value of $\Phi_{c,y}$ is a ratio of commodity output, $q$ in producer price for a given year to commodity output in purchaser price for that given year. 
 
$$
\Phi_c,y = \frac{q_{PRO,c,by}}{q_{PUR,c,y}}
$$ {#eq:Phi}


The origins of $\Phi$ is the Margins tables provide values for transportation, $t$, wholesale, $w$, and retail, $r$, specific to each commodity consumed by industries, households or investors. These same year margins data are used for all years, however they are price-adjusted first to calculate a total year specific margin

$$
q_{PUR,c,y} = q_cP_{c,y} + t_{c,y}P_{t,y} + w_{c,y}P_{w,y} + r_{c,y}P_{r,y}
$$ {#eq:PRO}

where $P_{m,y}$ for a margin type ($t$, $w$ or $r$) is calculated in [@Eq:Rho_m], which is the weighted average of price adjustments in commodities that make up $m$ (e.g. Truck transportation, Water transportation, Rail transportation are commodities in set $m$ for transportation).

$$
P_{m,y} = \frac{\sum_{c\in m}q_{c,y}P_{c,y}} {\sum_{c\in m}q_{c,y}}
$$ {#eq:Rho_m}


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







