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
