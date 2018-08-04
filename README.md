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

----------------------------------------------------------------------
-- Code to Scroll on specific portion OnClick

<script>
    function scrollToDiv() {
        if ($get('<%= hdnDivFocus.ClientID %>').value == "DivBookLost") {
            $get('<%= DivBookLost.ClientID %>').scrollIntoView();
        }
        else if ($get('<%= hdnDivFocus.ClientID %>').value == "DivPendingBooks") {
            $get('<%= DivPendingBooks.ClientID %>').scrollIntoView();
        }
        else if ($get('<%= hdnDivFocus.ClientID %>').value == "DivReturnBooks") {
            $get('<%= DivReturnBooks.ClientID %>').scrollIntoView();
        }
        else if ($get('<%= hdnDivFocus.ClientID %>').value == "DivMain") {
           $(window).scrollTop($("#ContentPlaceHolder1_UDCLmsTransaction_DivMain").offset().top-60);
        }
}
</script>


hdnDivFocus.Value = "DivBookLost";
ScriptManager.RegisterStartupScript(Page, typeof(Page), "scrollToDiv();", "setTimeout(scrollToDiv, 1);", true);

----------------------------------------------------------------------

----------------------------------------------------------------------
-- Jquery to scroll at specific point from header at 0.

$(window).scrollTop($("#ContentPlaceHolder1_UDCLmsTransaction_DivMain").offset().top-60);
   
   ----------------------------------------------------------------------
