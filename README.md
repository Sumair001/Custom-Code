# Custom-Code
Code for use.


----------------------------------------------------------------------
-- Check if value exists in comma separated string


bool isExists = umbracoCSV.Split(',').Any(x => x == "2");


----------------------------------------------------------------------


----------------------------------------------------------------------
-- Data from rows to single column with no comma's



DECLARE @listStr VARCHAR(MAX)
SELECT @listStr = COALESCE(@listStr+',' ,'') + Keywords
FROM lmsbook

select Distinct ColumnData from [dbo].[fn_CSVToTable](@listStr) Where ColumnData is not null And ColumnData <> ''



----------------------------------------------------------------------

--------------------------------------------------------------------------------------------------------------------------------
-->Main Join Query

select * INTO #Temp1 from ABC
select * INTO #Temp2 from ABC

select * 
from #Temp1 t1
inner join #Temp2 t2 on t1.ID = t2.ID

drop table #Temp1 
drop table #Temp2


--------------------------------------------------------------------------------------------------------------------------------
