## Data Cleaning Navshille Housing


select *
from portfolio_projects..navshilleHousing


# standarizing the date format of sale
select SalesDateConverted, convert(date,SaleDate) 
from portfolio_projects..navshilleHousing

alter table navshilleHousing
add SalesDateConverted Date;

update navshilleHousing
set SalesDateConverted = CONVERT(date,SaleDate)

------------------------------------------------------------------------------------------------

# populate address property data 

select *
from navshilleHousing
--where PropertyAddress is null
order by navshilleHousing.ParcelID

# self joining the table
# this inquiry will check that which both parcel id are same and both tables then we will check that in 
# table a which property address is null so we can copy the property addresses from table b to table a
# isnull(change to, change from)
select a.ParcelID , a.PropertyAddress , b.ParcelID , b.PropertyAddress, isnull(a.PropertyAddress,b.PropertyAddress)
from portfolio_projects..navshilleHousing a
join portfolio_projects..navshilleHousing b
on a.ParcelID = b.ParcelID
and a.[UniqueID ] <> b.[UniqueID ]
where a.PropertyAddress is null

# update property address in table a 
update a
set PropertyAddress = isnull(a.PropertyAddress,b.PropertyAddress)
from portfolio_projects..navshilleHousing a 
join portfolio_projects..navshilleHousing b
on a.ParcelID = b.ParcelID
and a.[UniqueID ] <> b.[UniqueID ]
where a.PropertyAddress is null

--------------------------------------------------------------------------------------------

# breaking out the addresses into the individual coloumn (address , city , state)
select PropertyAddress
from portfolio_projects..navshilleHousing

select 
SUBSTRING(PropertyAddress, 1, charindex(',',PropertyAddress)-1) as address
from portfolio_projects..navshilleHousing
