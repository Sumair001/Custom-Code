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

--> Asp Code
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

--> C# Code
hdnDivFocus.Value = "DivBookLost";
ScriptManager.RegisterStartupScript(Page, typeof(Page), "scrollToDiv();", "setTimeout(scrollToDiv, 1);", true);

----------------------------------------------------------------------

----------------------------------------------------------------------
-- Jquery to scroll at specific point from header at 0.

$(window).scrollTop($("#ContentPlaceHolder1_UDCLmsTransaction_DivMain").offset().top-60);
   
   ----------------------------------------------------------------------
   
   
   
----------------------------------------------------------------------
-- File uploading with formates

--> Asp 
  <asp:Label ID="lblFilePath" runat="server" Visible="false"></asp:Label>
  <asp:FileUpload ID="fuFile" runat="server" />


--> C# Code
if ((fileExtension == ".jpeg" || fileExtension == ".png" || fileExtension == ".jpg") || fuFile.HasFile == false)
{
if (fuFile.HasFile)
        {
            if (txtMode.Text == "Update")
            {
                string filePathDelete = lblFilePath.Text;
                string FolderPathDelete = ConfigurationManager.AppSettings["LmsUpload"];
                string FilePathDelete = Server.MapPath(FolderPathDelete + filePathDelete);
                if ((System.IO.File.Exists(FilePathDelete)))
                {
                    System.IO.File.Delete(FilePathDelete);
                }
            }
            if (!Directory.Exists(Server.MapPath("~/File/Lms/LmsCoverPage/")))
            {
                Directory.CreateDirectory(Server.MapPath("~/File/Lms/LmsCoverPage/"));
            }

            string fileExtension = Path.GetExtension(fuFile.PostedFile.FileName);
            string filePath = "B_" + objLMS.LmsBookID + "_" + txtLmsAccessionNo.Text + fileExtension;

            string FolderPath = ConfigurationManager.AppSettings["LmsUpload"];
            string FilePath = Server.MapPath(FolderPath + filePath);
            fuFile.SaveAs(FilePath);

            objLMS.CoverPagePath = filePath;
            objLMS.LmsBookUpdatePath();
        }
        else
        {

        }
}
else
{

}
----------------------------------------------------------------------


----------------------------------------------------------------------
-- Find procedure by name
 
select * 
from 
   sys.procedures 
where 
   name like '%Title%'


----------------------------------------------------------------------
