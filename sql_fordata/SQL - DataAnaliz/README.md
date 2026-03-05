<img width="857" height="343" alt="image" src="https://github.com/user-attachments/assets/305eee6f-860d-44e3-a643-eb8db1f8939f" />

-----------------------------------FactSales-----------------------------------------

select s.SalesKey 
,CAST(s.DateKey as date) as Date
,s.channelKey 
,s.StoreKey 
,s.ProductKey 
,s.PromotionKey 
,s.UnitCost
,s.UnitPrice
,s.SalesQuantity
,s.ReturnQuantity
,s.ReturnAmount
,s.DiscountQuantity
,s.DiscountAmount
,s.TotalCost
,s.SalesAmount
from FactSales s 

------------------------------------------DimDate----------------------------------------

select CAST(dd.FullDateLabel as date) as Date
,YEAR(dd.FullDateLabel) as Year
,MONTH(dd.FullDateLabel) as Month
,dd.CalendarMonth as YearAndMonth
,dd.CalendarMonthLabel as MonthName
,dd.CalendarDayOfWeekLabel as DayName
,dd.CalendarMonthLabel + ' ' + CAST(YEAR(dd.FullDateLabel) as varchar) as 'Calendar Year Month'
,case when dd.IsWorkDay = 'WorkDay' then 1 else 0 end as IsWorkDay
,dd.IsHoliday
from DimDate dd 

------------------------------------------DimChannel----------------------------------------

select cn.ChannelKey
,cn.ChannelName
from DimChannel cn 

------------------------------------------DimGeography----------------------------------------

select gp.GeographyKey 
,gp.GeographyType
,gp.ContinentName
,gp.RegionCountryName
from DimGeography gp 

------------------------------------------DimProduct----------------------------------------

select pd.ProductKey 
,pd.ProductLabel as ProductCode
,pd.ProductSubcategoryKey
,pd.ProductName
,pd.UnitCost
,pd.UnitPrice
,pd.ClassName
,pd.BrandName
,pd.Manufacturer
,pd.ColorID
,pd.ColorName
--,pd.Weight
--,pd.WeightUnitMeasureID
,case 
	when pd.WeightUnitMeasureID = 'ounces' then (pd.Weight * 28) / 1000
	when pd.WeightUnitMeasureID = 'pounds' then (pd.Weight * 454) / 1000
	when pd.WeightUnitMeasureID = 'grams' then pd.Weight / 1000
	end as Weight

from DimProduct pd 

------------------------------------------DimProductCategory----------------------------------------

select a.ProductCategoryKey 
,a.ProductCategoryName
from DimProductCategory a 

------------------------------------------DimProductSubcategory----------------------------------------

select a.ProductSubcategoryKey 
,a.ProductSubcategoryName
,a.ProductCategoryKey
from DimProductSubcategory a 


-----------------------------------DimPromotion-----------------------------------------

select pr.PromotionKey 
,pr.PromotionLabel as PromotionCode
,pr.PromotionName
,pr.DiscountPercent
,CAST(pr.StartDate as date) as StartDate
,CAST(pr.EndDate as date) as EndDate
from DimPromotion as pr

------------------------------------------DimStore----------------------------------------

select st.StoreKey 
,st.GeographyKey 
,st.StoreType
,st.StoreName
,st.Status
,CAST(st.OpenDate as date) as OpenDate
,st.EmployeeCount
,st.SellingAreaSize
,CAST(st.CloseDate as date) as CloseDate
from DimStore st

------------------------------------------DimCustomer----------------------------------------

select cs.CustomerKey 
,cs.GeographyKey 
,cs.FirstName
,cs.LastName
,cs.BirthDate
,cs.Gender
,CAST(cs.YearlyIncome as money) as YearlyIncome
,cs.TotalChildren
,cs.HouseOwnerFlag
from DimCustomer cs


------------------------------------------DimEmployee----------------------------------------

select a.EmployeeKey 
,a.ParentEmployeeKey
,a.FirstName
,a.LastName
,a.Gender
,a.BirthDate
,a.Title
,a.HireDate
,a.EndDate
,a.MaritalStatus
,a.VacationHours
,a.DepartmentName
from DimEmployee a 

