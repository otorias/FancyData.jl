# FancyData.jl

Some functions that I use regularly when working with DataFrames (DataFrames.jl) and uncertainties (Measurement.jl)

## Setup

Install via

```julia
] add https://github.com/otorias/FancyData.jl.git
```
Then use with

```julia
using FancyData
```

if you want to edit the package.

## Getting started

### Measurement utilities

One of the main functionalities of this package is the presentation of measurement-type variables.
This is handled by the function 
```julia
mes(x::Measurement)
```
which returns a String with format `value(uncertainty)`.

Another nice function is
```julia
wmean(X::AbstractVector; mode::Symbol=:max)
```
which returns the weighted mean of a Vector of Measurements.
Although `Measurement.jl` exports the function `weightedmean`, it only considers the external uncertainty, whereas `wmean` uses the larger of the external and internal uncertainty (and returns both uncertainties if used with `mode=:both`).

Also usefule is
```julia
mean_std(X::AbstractVector)
```
which returns the mean with standard deviation of an array without Measurements.

### DataFrame utilities

The funciton
```julia
tableDF(DF::AbstractDataFrame)
```
returns an preformatted TeX table from a DataFrame.
notably Measurement-parsing is included and siunitx columns formatting is done automatically.

For reading and writing of DataFrames the functions
```julia
writeDF(file_out::AbstractString, DF::AbstractDataFrame; delim=::Char=`\t`)
readDF(file_in::AbstractString; delim=::Char=`\t`)
```
are quite useful.
`writeDF` writes a DataFrame to a file with the first row being the names of the column.
`readDF` takes the first row of a delimited file (default delimiter `tab`) as the column names and builds a DataFrame from the file.
It also attempts to parse values as Measurements or Missing if possible.

### Other

Additionally the function
```julia
readfits(XML_file::AbstractString; mode::Symbol=:peak)
```
is exported which reads an hdtv-fit file and converts it to a DataFrame.
It operates in 3 different modes resulting in different columns.
- `:peak`: \[fit_ID, position, width, volume\]
- `:sub`: \[fit_ID, centroid, width, volume\]
- `:params`: \[fit_ID, peak_marker, region_marker, bg_marker\]

Here `:sub` is the background-subtracted integral.
