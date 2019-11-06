# Custom-Code


----------------------------------------------------------------------
--> Javascript function to run on page before each postback using customvalidator with fix group.

<script type="text/javascript">
        // Script to stop postback if records not saved.
        function ValidateListBox(sender, args) {
            var isSaved = document.getElementById("<%=hdn_IsSaved.ClientID%>").value;
            if (isSaved == "True") {
                args.IsValid = true;
            }
            else {
                alert('Records are not saved.');
                args.IsValid = false;
            }
        }
    </script>


----------------------------------------------------------------------
--> Jquery function to run on page before each postback

<script type="text/javascript">
        // Script to run before each postback to check validation.
        $('form').submit(function () {
            var hv = $('#ContentPlaceHolder1_hdn_IsSaved').val();
            return $.parseJSON(hv.toLowerCase());
        });
    </script>







----------------------------------------------------------------------
--> Nested Gridview Work

--ASP
  <div class="container-fluid">
        <div class="row">
            <div class="col-md-12">
                <div class="gridviewWrapper table table-responsive">
                    <asp:GridView ID="gv_Main" runat="server" CssClass="object-grid table-bordered table-condensed table-hover"
                        RowStyle-CssClass="rowStyle" ShowHeaderWhenEmpty="true" AlternatingRowStyle-CssClass="alterrow" AllowPaging="true"
                        EmptyDataText="No records found" AutoGenerateColumns="false" PageSize="10" HeaderStyle-CssClass="grid-header" OnRowDataBound="gv_Main_OnRowDataBound"
                        OnPageIndexChanging="gv_Main_PageIndexChanging" PagerStyle-CssClass="grdviewpage" PagerStyle-HorizontalAlign="Center">
                        <PagerSettings
                            Position="TopAndBottom"
                            Mode="NextPrevious" />
                        <PagerTemplate>
                            <div>
                                <div class="page-no">
                                    <% if (gv_Main.PageCount > 0)
                                        { %>
                                    <i>You are viewing page <%= gv_Main.PageIndex + 1 %> of <%=gv_Main.PageCount %></i> <% } %>
                                </div>
                                <div>
                                    <asp:LinkButton runat="server" ID="btn_First" CommandName="Page" Font-Underline="false" CommandArgument="First" ToolTip="Navigate to First Page" Text="<<"></asp:LinkButton>
                                    <asp:LinkButton runat="server" ID="btn_Pre" CommandName="Page" Font-Underline="false" CommandArgument="Prev" ToolTip="Navigate to Previous Page" Text="Previous"></asp:LinkButton>
                                    <asp:LinkButton runat="server" ID="btn_Next" CommandName="Page" Font-Underline="false" CommandArgument="Next" ToolTip="Navigate to Next Page" Text="Next"></asp:LinkButton>
                                    <asp:LinkButton runat="server" ID="btn_Last" CommandName="Page" Font-Underline="false" CommandArgument="Last" ToolTip="Navigate to Last Page" Text=">>"></asp:LinkButton>
                                </div>
                            </div>
                        </PagerTemplate>
                        <Columns>
                            <asp:TemplateField>
                                <ItemTemplate>
                                    <asp:Label ID="lbl_SNO" runat="server" Visible="false" Text='<%#Eval("DT_SNO") %>'></asp:Label>
                                    <img alt="" style="cursor: pointer" src="../../images/Other/plus.png" />
                                    <asp:Panel ID="pnlOrders" runat="server" Style="display: none">
                                        <asp:GridView ID="gv_IntegrationLogs" runat="server" AutoGenerateColumns="false" CssClass="object-grid table-bordered table-condensed table-hover"
                                             ShowHeader="false">
                                            <Columns>
                                                <asp:TemplateField HeaderStyle-Width="10%" HeaderText="Main">
                                                    <ItemTemplate>
                                                        <asp:Label ID="lbl_ObjectName" runat="server" Text='<%#Eval("ObjectName") %>'></asp:Label>
                                                    </ItemTemplate>
                                                </asp:TemplateField>
                                                <asp:TemplateField HeaderStyle-Width="10%" HeaderText="Date">
                                                    <ItemTemplate>
                                                        <asp:Label ID="lbl_LogDate" runat="server" Text='<%#Eval("LogDate") %>'></asp:Label>
                                                    </ItemTemplate>
                                                </asp:TemplateField>
                                                <asp:TemplateField HeaderStyle-Width="10%" HeaderText="Related">
                                                    <ItemTemplate>
                                                        <asp:Label ID="lbl_SubObject" runat="server" Text='<%#Eval("SubObject") %>'></asp:Label>
                                                    </ItemTemplate>
                                                </asp:TemplateField>
                                                <asp:TemplateField HeaderStyle-Width="15%" HeaderText="RecordID">
                                                    <ItemTemplate>
                                                        <asp:Label ID="lbl_RecordID" runat="server" Text='<%#Eval("RecordID") %>'></asp:Label>
                                                    </ItemTemplate>
                                                </asp:TemplateField>
                                                <asp:TemplateField HeaderStyle-Width="15%" HeaderText="Error">
                                                    <ItemTemplate>
                                                        <div class="cell-overflow">
                                                            <asp:Label ID="lbl_Error" runat="server" Text='<%#Eval("ErrorMessage") %>'></asp:Label>
                                                        </div>
                                                    </ItemTemplate>
                                                </asp:TemplateField>
                                                <asp:TemplateField HeaderStyle-Width="15%" HeaderText="Value" Visible="false">
                                                    <ItemTemplate>
                                                        <div class="cell-overflow">
                                                            <asp:Label ID="lbl_RecordValue" runat="server" Text='<%#Eval("RecordValue") %>'></asp:Label>
                                                        </div>
                                                    </ItemTemplate>
                                                </asp:TemplateField>
                                                <asp:TemplateField HeaderText="Detail" HeaderStyle-Width="20%" Visible="false">
                                                    <ItemTemplate>
                                                        <div id="DivStraceDetailToggle" class="cell-overflow">
                                                            <asp:Label ID="lbl_StraceDetail" runat="server" Text='<%#Eval("StraceDetail") %>'></asp:Label>
                                                        </div>
                                                    </ItemTemplate>
                                                </asp:TemplateField>
                                                <asp:TemplateField HeaderStyle-Width="5%" HeaderText="2-Way">
                                                    <ItemTemplate>
                                                        <asp:CheckBox ID="chk_TwoWay" runat="server" Checked='<%#Convert.ToBoolean(Eval("IsTwoWaySync")) %>' Enabled="false" CssClass="grid-checkbox" />
                                                    </ItemTemplate>
                                                </asp:TemplateField>
                                            </Columns>
                                        </asp:GridView>
                                    </asp:Panel>
                                </ItemTemplate>
                            </asp:TemplateField>
                            <asp:TemplateField HeaderStyle-Width="10%" HeaderText="Main">
                                <ItemTemplate>
                                    <%--<asp:Label ID="lbl_ObjectName" runat="server" Text='<%#Eval("ObjectName") %>'></asp:Label>--%>
                                </ItemTemplate>
                            </asp:TemplateField>
                            <asp:TemplateField HeaderStyle-Width="10%" HeaderText="Date">
                                <ItemTemplate>
                                    <%--<asp:Label ID="lbl_LogDate" runat="server" Text='<%#Eval("LogDate") %>'></asp:Label>--%>
                                    <asp:Label ID="lbl_DateHour" runat="server" Text='<%#Eval("DateHour") %>'></asp:Label>
                                </ItemTemplate>
                            </asp:TemplateField>
                            <asp:TemplateField HeaderStyle-Width="10%" HeaderText="Related">
                                <ItemTemplate>
                                    <%--<asp:Label ID="lbl_SubObject" runat="server" Text='<%#Eval("SubObject") %>'></asp:Label>--%>
                                </ItemTemplate>
                            </asp:TemplateField>
                            <asp:TemplateField HeaderStyle-Width="15%" HeaderText="RecordID">
                                <ItemTemplate>
                                    <%--<asp:Label ID="lbl_RecordID" runat="server" Text='<%#Eval("RecordID") %>'></asp:Label>--%>
                                </ItemTemplate>
                            </asp:TemplateField>
                            <asp:TemplateField HeaderStyle-Width="15%" HeaderText="Error">
                                <ItemTemplate>
                                    <div class="cell-overflow">
                                        <%--<asp:Label ID="lbl_Error" runat="server" Text='<%#Eval("ErrorMessage") %>'></asp:Label>--%>
                                    </div>
                                </ItemTemplate>
                            </asp:TemplateField>
                            <asp:TemplateField HeaderStyle-Width="15%" HeaderText="Value" Visible="false">
                                <ItemTemplate>
                                    <div class="cell-overflow">
                                        <%--<asp:Label ID="lbl_RecordValue" runat="server" Text='<%#Eval("RecordValue") %>'></asp:Label>--%>
                                    </div>
                                </ItemTemplate>
                            </asp:TemplateField>
                            <asp:TemplateField HeaderText="Detail" HeaderStyle-Width="20%" Visible="false">
                                <ItemTemplate>
                                    <div id="DivStraceDetailToggle" class="cell-overflow">
                                        <%--<asp:Label ID="lbl_StraceDetail" runat="server" Text='<%#Eval("StraceDetail") %>'></asp:Label>--%>
                                    </div>
                                </ItemTemplate>
                            </asp:TemplateField>
                            <asp:TemplateField HeaderStyle-Width="5%" HeaderText="2-Way">
                                <ItemTemplate>
                                    <%--<asp:CheckBox ID="chk_TwoWay" runat="server" Checked='<%#Convert.ToBoolean(Eval("IsTwoWaySync")) %>' Enabled="false" CssClass="grid-checkbox" />--%>
                                </ItemTemplate>
                            </asp:TemplateField>
                        </Columns>
                    </asp:GridView>
                </div>
            </div>
        </div>
    </div>
                        
                        
--> Javascript

<script type="text/javascript">
        $("[src*=plus]").live("click", function () {
            $(this).closest("tr").after("<tr><td></td><td colspan = '999'>" + $(this).next().html() + "</td></tr>")
            $(this).attr("src", "../../images/Other/minus.png");
        });
        $("[src*=minus]").live("click", function () {
            $(this).attr("src", "../../images/Other/plus.png");
            $(this).closest("tr").next().remove();
        });
    </script>
    
    
--> C#

 void GetIntegrationLogs(string sNo, GridView gv)
    {

        IntegrationHelper h = new IntegrationHelper(UserID, Environment, LCValue);
        DataTable dt = new DataTable();
        dt = h.GetIntegrationLogs(ddl_Users.SelectedValue, ddl_Object.SelectedValue,
                                   !string.IsNullOrEmpty(txt_FromDate.Text) ? DateTime.Parse(txt_FromDate.Text).ToString("yyyy-MM-dd") : txt_FromDate.Text,
                                   !string.IsNullOrEmpty(txt_ToDate.Text) ? DateTime.Parse(txt_ToDate.Text).ToString("yyyy-MM-dd") : txt_ToDate.Text, sNo);

        for (int i = 0; i < gv.Columns.Count; i++)
        {
            if (gv.Columns[i].HeaderText == "Value" || gv.Columns[i].HeaderText == "Detail")
            {
                if (currentUserRole == "BizConnectUser")
                {
                    gv.Columns[i].Visible = false;
                }
                else
                {
                    gv.Columns[i].Visible = true;
                }
            }
        }
        if (sNo == string.Empty)
        {
            DataView view = new DataView(dt);
            dt = view.ToTable(true, "DT_SNO", "DateHour");
        }
        else
        {
            if (currentUserRole == "BizConnectUser")
            {
                gv.Columns[5].Visible = false;
                gv.Columns[6].Visible = false;
            }
            else
            {
                gv.Columns[5].Visible = true;
                gv.Columns[6].Visible = true;
            }

            //if (dt != null && dt.Rows.Count > 0)
            //{
            //    DataView dv = new DataView(dt);
            //    dv.Sort = Helper.GetStringValue(Helper.BizConnectIntegrationVariables.LogDate) + " desc";
            //    gv.DataSource = dv.ToTable();
            //}
            //else
            //{
            //    gv.DataSource = dt;
            //}
        }
        gv.DataSource = dt;
        gv.DataBind();

    }
    
    
        protected void gv_Main_OnRowDataBound(object sender, GridViewRowEventArgs e)
    {
        if (e.Row.RowType == DataControlRowType.DataRow)
        {
            Label lbl_SNO = (Label)e.Row.FindControl("lbl_SNO");
            GridView gv_IntegrationLogs = (GridView)e.Row.FindControl("gv_IntegrationLogs");
            GetIntegrationLogs(lbl_SNO.Text, gv_IntegrationLogs);
        }
    }





----------------------------------------------------------------------
--> Query to assign Sno to same records.

Select dense_rank() OVER (ORDER BY CONVERT(nvarchar(13), IntegrationLogDetail.DateTime, 120) desc) as DT_SNO,

----------------------------------------------------------------------
--> Find Real Time according to coountry.


protected string RealDateTime(string BranchID)
    {
        try
        {
            if (BranchID == "2")
            {
                return TimeZoneInfo.ConvertTime(Convert.ToDateTime(DateTime.Now), TimeZoneInfo.FindSystemTimeZoneById("Pakistan Standard Time")).ToString();
            }
            else if (BranchID == "4")
            {
                return TimeZoneInfo.ConvertTime(Convert.ToDateTime(DateTime.Now), TimeZoneInfo.FindSystemTimeZoneById("E. Africa Standard Time")).ToString();
            }
            else
            {
                return TimeZoneInfo.ConvertTime(Convert.ToDateTime(DateTime.Now), TimeZoneInfo.FindSystemTimeZoneById("India Standard Time")).ToString();
            }
        }
        catch (Exception Exp)
        {
            lblError.Text = cMessages.Desc(GlobalMsg.g_UnkownFailed);
             
            return "";
        }
    }
    
----------------------------------------------------------------------
--> Email Code using credentials.

 public string SendEmail(string fromAddr, string fromPswd, string toAddr, string subject, string body, string processName)
    {
        try
        {
        // First of all allow less secure app in sumair.sattar@biznussoft.com account's setting
            var fromAddress = new MailAddress("sumair.sattar@biznussoft.com");
            var toAddress = new MailAddress("sumairsattar62@gmail.com");

            System.Net.Mail.SmtpClient smtp = new System.Net.Mail.SmtpClient
            {
                Host = "smtp.gmail.com",
                Port = 587,
                EnableSsl = true,
                DeliveryMethod = System.Net.Mail.SmtpDeliveryMethod.Network,
                UseDefaultCredentials = true,
                Credentials = new NetworkCredential(fromAddress.Address, "sumair007")
            };

            using (var message = new System.Net.Mail.MailMessage(fromAddress, toAddress)
            {
                Subject = "Test Email",
                Body = "Salam! Sumair",
                IsBodyHtml = false
            })
                smtp.Send(message);


            // Save to DB
            SaveEmailRecords(processName, fromAddr, toAddr, body, "Sent");
            return string.Empty;
        }
        catch (Exception ex)
        {
            // Save to DB
            SaveEmailRecords(processName, fromAddr, toAddr, body, "Not Sent");
            return ex.Message + " " + ex.StackTrace;
        }

    }

----------------------------------------------------------------------
--> Get gridview row in button click event.

protected void lnk_Name_Click(object sender, EventArgs e)
    {
        GridViewRow row = (GridViewRow)((LinkButton)sender).NamingContainer;
        LinkButton lnk_Name = (LinkButton)row.FindControl("lnk_Name");


    }

----------------------------------------------------------------------
--> Date formate to nvarchar 2017-7-17.

CONVERT(nvarchar(10), RequestedDate, 120)

----------------------------------------------------------------------
--> Adding and removing space in string.
// Add space
hdnType.Value = hdnType.Value.Insert(6," ")

// Remove spaces from Type name
hdnType.Value = string.Join("", hdnType.Value.Split(default(string[]), StringSplitOptions.RemoveEmptyEntries));


----------------------------------------------------------------------
--> Sql Paging
DECLARE @Index INT;
DECLARE @PageSize INT;

SET @Index = 3;
SET @PageSize = 5;

SELECT *  FROM
  (SELECT  ROW_NUMBER() OVER (ORDER BY EmpID asc) as MyRowNumber,*
  FROM Employee) tblEmployee
WHERE MyRowNumber BETWEEN ( ((@Index - 1) * @PageSize )+ 1) AND @Index*@PageSize 

----------------------------------------------------------------------
--> Insert, UPdate and Delete in single table with @table

ALTER PROCEDURE [dbo].[SP_Hotel_Info_Update]
     -- Add the parameters for the stored procedure here
    @XHotelInfoDetails UpdateHotelTableType READONLY,

AS
BEGIN

    MERGE dbo.HotelInfo AS trg
    USING @XHotelInfoDetails AS src
      ON src.ID = trg.ID
     WHEN MATCHED THEN
       UPDATE SET FromDate = src.FromDate
     WHEN NOT MATCHED BY TARGET THEN
       INSERT (col1, col2, ...)
       VALUES (src.col1, src.col2, ...);
END


----------------------------------------------------------------------
--> Highlight text in gridview
<style type="text/css">
    .highlight {
        text-decoration: none;
        font-weight: bold;
        color: black;
        background: #ffff0087;
        display: inline-block;
    }
</style>

<asp:GridView ID="grdSearch" runat="server"
              AutoGenerateColumns="false">
<Columns>
<asp:TemplateField HeaderText="FirstName">
<ItemTemplate>
<asp:Label ID="lblFirstName" runat="server" 
           Text='<%# Highlight(Eval("FirstName").ToString()) %>'/>
</ItemTemplate>
</Columns>
</asp:GridView>

 public string Highlight(string InputTxt)
    {
        string Search_Str = txtBookTitle.Text.ToString();

        // Setup the regular expression and add the Or operator.

        Regex RegExp = new Regex(Search_Str.Replace(" ", "|").Trim(),

        RegexOptions.IgnoreCase);



        // Highlight keywords by calling the 

        //delegate each time a keyword is found.

        return RegExp.Replace(InputTxt,

          new MatchEvaluator(ReplaceKeyWords));
        // Set the RegExp to null.
        RegExp = null;
    }

    public string ReplaceKeyWords(Match m)

    {
        return "<span class=highlight>" + m.Value + "</span>";
    }

----------------------------------------------------------------------
--> Script for PFD downloading.

 Response.Clear();
                    Response.Buffer = true;
                    Response.ClearContent();
                    Response.ClearHeaders();
                    Response.Charset = "";
                    StringWriter strwritter = new StringWriter();
                    HtmlTextWriter htmltextwrtter = new HtmlTextWriter(strwritter);
                    Response.Cache.SetCacheability(HttpCacheability.NoCache);
                    Response.ContentType = "application/pdf";
                    Response.AddHeader("Content-Disposition", "attachment;filename=" + pdfFileName);
                    Response.ContentEncoding = System.Text.Encoding.Unicode;
                    Response.BinaryWrite(System.Text.Encoding.Unicode.GetPreamble());
                    Response.TransmitFile((pdfFilePathWithoutExtension.Value + ".pdf"));
                    Response.Flush();
                    Response.End();
                    
                    
----------------------------------------------------------------------
--> Text Water Mark Script.

public void AddWaterMark(string filePath)
    {
        // Write Text on Image
        DrawText("Sumair Water For Digital Page");

        // Image should be with low opacity.
        string WatermarkLocation = "D:\\myImage.png";

        string FileLocation = "D:\\B_177744_DB6_v1.pdf";

        Document document = new Document();
        PdfReader pdfReader = new PdfReader(FileLocation);
        PdfStamper stamp = new PdfStamper(pdfReader, new FileStream(FileLocation.Replace(".pdf", "[temp][file].pdf"), FileMode.Create));

        //iTextSharp.text.Image img = DrawText("Sumair Water", new System.Drawing.Font("Times New Roman", 12.0f), Color.Black, Color.White);
        iTextSharp.text.Image img = iTextSharp.text.Image.GetInstance(WatermarkLocation);

        // Decrease image size to increase quality.
        img.ScalePercent(40);

        // set the position in the document where you want the watermark to appear (0,0 = bottom left corner of the page)
        img.SetAbsolutePosition(20, 20); 

        PdfContentByte waterMark;
        for (int page = 1; page <= pdfReader.NumberOfPages; page++)
        {
            waterMark = stamp.GetOverContent(page);
            waterMark.AddImage(img);
        }
        stamp.FormFlattening = true;
        stamp.Close();
        pdfReader.Close();

        // now delete the original file and rename the temp file to the original file
        File.Delete(FileLocation);
        File.Move(FileLocation.Replace(".pdf", "[temp][file].pdf"), FileLocation);
        //File.Delete("D:\\myImage.png");
    }

    // Write Text on Image
    private void DrawText(String text)
    {

        text = text.Trim();
        Bitmap bitmap = new Bitmap(1, 1);
        System.Drawing.Font font = new System.Drawing.Font("Calibri", 15, FontStyle.Regular, GraphicsUnit.Pixel);
        Graphics graphics = Graphics.FromImage(bitmap);
        int width = 400;
        int height = (int)graphics.MeasureString(text, font).Height;
        bitmap = new Bitmap(bitmap, new Size(width, height));
        graphics = Graphics.FromImage(bitmap);
        graphics.Clear(Color.Transparent);
        graphics.SmoothingMode = SmoothingMode.AntiAlias;
        graphics.TextRenderingHint = TextRenderingHint.AntiAlias;
        graphics.DrawString(text, font, new SolidBrush(Color.Black), 0, 0);
        graphics.Flush();
        graphics.Dispose();
        //string fileName = Path.GetFileNameWithoutExtension(Path.GetRandomFileName()) + ".jpg";
        //string fileName = Path.GetFileNameWithoutExtension(Path.GetRandomFileName()) + ".jpg";
        bitmap.Save("D:\\myImage.png", ImageFormat.Png);

    }

----------------------------------------------------------------------
--> Regular expression for file uploader for formating.
-> Aspx


<asp:RegularExpressionValidator ID="revImage" ValidationGroup="LmsBookManageGroup" ValidationExpression="^.+(.jpg|.JPG|.png|.PNG|.jpeg|.JPEG)$" ControlToValidate="fuImage" runat="server" ForeColor="Red" Display="Dynamic" />



----------------------------------------------------------------------
--> Disable text box enter.

onkeydown="return (event.keyCode!=13);"


----------------------------------------------------------------------
--> Water mark image in pdf in asp.net 

-> C# Code

                public void AddWaterMark()
        {
            // Image should be with low opacity.
            string WatermarkLocation = "D:\\NewImage.png";
            string FileLocation = "D:\\Work\\NewFile.pdf";

            Document document = new Document();
            PdfReader pdfReader = new PdfReader(FileLocation);
            PdfStamper stamp = new PdfStamper(pdfReader, new FileStream(FileLocation.Replace(".pdf", "[temp][file].pdf"), FileMode.Create));

            iTextSharp.text.Image img = iTextSharp.text.Image.GetInstance(WatermarkLocation);
            img.SetAbsolutePosition(100, 200); // set the position in the document where you want the watermark to appear (0,0 = bottom left corner of the page)



            PdfContentByte waterMark;
            for (int page = 1; page <= pdfReader.NumberOfPages; page++)
            {
                waterMark = stamp.GetOverContent(page);
                waterMark.AddImage(img);
            }
            stamp.FormFlattening = true;
            stamp.Close();
            pdfReader.Close();

            // now delete the original file and rename the temp file to the original file
            File.Delete(FileLocation);
            File.Move(FileLocation.Replace(".pdf", "[temp][file].pdf"), FileLocation);
        }


----------------------------------------------------------------------


----------------------------------------------------------------------
--> Remove Item from dropdownlist with value.

-> C# Code

ddlUserType.Items.Remove(ddlUserType.Items.FindByValue("7"));


----------------------------------------------------------------------


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


----------------------------------------------------------------------
-- Custom work for dynamic colunms in grid

-- SQL
--  [sp_AllTeacherDetails2] '%','%',
'ID,[ITSID1],[ITSGender1],[AttendanceException1],Branch,ITSID,KHNO,Name,Mobile,Email,[Joining Date]' ,'' 

CREATE procedure [dbo].[sp_AllTeacherDetails2]                    
 @BranchID as nvarchar(100)=null,                    
 @StafftypeID as nvarchar(100)=null,                           
 @LoggedUserID as int,  
 @Field as nvarchar(max) ,
 @SearchField    as nvarchar(max)                
AS                    
BEGIN    
IF @SearchField ='' SET @SearchField =' cast(ITSID as nvarchar) +'' ''+ cast(KHNO as nvarchar) like ''%%%'' '
----------------------------------------------------------------------------------  
declare @sqlstring nvarchar(max)  
----------------------------------------------------------------------------------  
set @sqlstring = 'SELECT        ' + @Field + '
FROM            (SELECT        TOP (100) PERCENT tblTeacher.ID, tblTeacher.BranchID, tblTeacher.ITSID, tblTeacher.ITSID AS ITSID1, tblTeacher.KHNO, tblTeacher.RFID, 
                                                    tblTeacher.TRNO_Jamea AS TRNO, tblTeacher.ITS_F_Prefix AS [ITS First Prefix], tblTeacher.ITS_FirstName, tblTeacher.ITS_Surname, 
                                                    tblTeacher.SurnameAR, tblTeacher.Name, tblTeacher.NameAR, tblTeacher.CompletionYearFromJameaAR AS [Farigh Year], CONVERT(VARCHAR(11), 
                                                    tblTeacher.JoiningDate, 106) AS [Joining Date]
                          FROM            tblTeacher LEFT OUTER JOIN
                                                    tblJsTitle ON tblTeacher.JSTitleID = tblJsTitle.JsTitleID LEFT OUTER JOIN
                          WHERE        (ISNULL(CAST(tblTeacher.BranchID AS nvarchar), N'''') LIKE ''' + @BranchID + ''') AND (ISNULL(CAST(tblTeacher.StaffTypeID AS nvarchar), N'''') 
                                                    LIKE ''' + @StafftypeID + ''') AND (ISNULL(CAST(tblTeacher.ITSID AS nvarchar(100)), N'''') LIKE ''' +  @ITSID + ''')) AS TeacherDetail
WHERE ' +  @SearchField + '
ORDER BY BranchSortOrder,TeacherSortOrder1,TeacherSortOrder'  
----------------------------------------------------------------------------------  
--print (@sqlstring)  
execute (@sqlstring)  
----------------------------------------------------------------------------------  
END 



--> Asp 

-- Checkboxes
<label class="control-label bold">
                        JHS
                    </label>
                    <div class="cscroller" style="height: 125px;">
                        <asp:CheckBoxList ID="cblJHSCol" runat="server">
                            <asp:ListItem Value="[TRNO]">TRNO</asp:ListItem>
                            <asp:ListItem Value="[Farigh Year]">Farigh Year</asp:ListItem>
                        </asp:CheckBoxList>
                    </div>


-- Add column button
 <asp:LinkButton ID="btnFilter" runat="server" OnClick="btnFilter_Click" class="btn green-jungle"><i class="fa fa-search"></i>&nbsp;ADD</asp:LinkButton>

-- Search in specific fields
<asp:LinkButton ID="btnSearchField" runat="server" OnClick="btnSearchField_Click" class="btn green-jungle"><i class="fa fa-search"></i>&nbsp;Search Field</asp:LinkButton>

-- Gridview 
    <asp:GridView runat="server" ID="grdTeachersSelectAll" AutoGenerateColumns="false"
                                    GridLines="None" class="table table-striped table-hover murtaza stickHead" OnPageIndexChanging="grdTeachersSelectAll_PageIndexChanging"
                                    OnRowDataBound="grdTeachersSelectAll_RowDataBound" AllowSorting="True" OnSorting="grdTeachersSelectAll_Sorting"
                                    OnRowCommand="grdTeachersSelectAll_RowCommand" OnRowCreated="grdTeachersSelectAll_RowCreated">
                                    <AlternatingRowStyle BackColor="WhiteSmoke" />
                                    <Columns>
                                        <asp:TemplateField HeaderText="Photos">
                                            <ItemTemplate>
                                                <asp:Image ID="imgTeacher" CssClass="pic" Style="cursor: pointer" runat="server"
                                                    Height="30" Width="30" />
                                                <asp:Label ID="lblTeacherID" runat="server" Visible="false" Text='<%# Eval("ID") %>'></asp:Label>
                                                <asp:Label ID="lblITSID" runat="server" Visible="false" Text='<%# Eval("ITSID1") %>'></asp:Label>
                                                <asp:Label ID="lblGender" runat="server" Visible="false" Text='<%# Eval("ITSGender1") %>'></asp:Label>
                                            </ItemTemplate>
                                        </asp:TemplateField>
                                        <asp:TemplateField HeaderText="Exception">
                                            <ItemTemplate>
                                                <label class="mt-checkbox" onclick="togal()">
                                                    <%--OnCheckedChanged="chkException_CheckedChanged" --%>
                                                    <asp:CheckBox ID="chkException" OnCheckedChanged="chkException_CheckedChanged" Checked='<%#Convert.ToBoolean(Eval("AttendanceException1")) %>'
                                                        runat="server" AutoPostBack="true" />
                                                    <span></span>
                                                </label>
                                            </ItemTemplate>
                                        </asp:TemplateField>
                                        <asp:TemplateField HeaderText="Action">
                                            <ItemTemplate>
                                                <div class="btn-group">
                                                    <asp:LinkButton ID="lnkView" runat="server" CommandArgument='<%# Eval("ITSID1") %>'
                                                        CommandName="ViewProfile" ToolTip="View" class="btn green-jungle">View</asp:LinkButton>
                                                    <button type="button" class="btn green-jungle dropdown-toggle" data-toggle="dropdown"
                                                        data-hover="dropdown" data-delay="1000" data-close-others="true">
                                                        <i class="fa fa-angle-down"></i>
                                                    </button>
                                                    <ul class="dropdown-menu" role="menu">
                                                        <li>
                                                            <asp:LinkButton ID="lnkbtnEdit" CommandArgument='<%# Eval("ID") %>' CommandName="EditTeacher"
                                                                runat="server">Edit</asp:LinkButton>
                                                        </li>
                                                    </ul>
                                                </div>
                                            </ItemTemplate>
                                        </asp:TemplateField>
                                    </Columns>
                                    <PagerStyle CssClass="pagination" />
                                </asp:GridView>



-- c# Code

    protected void Page_Load(object sender, EventArgs e)
    {

                if (!IsPostBack)
                {
                    //Fixed Colunms
                    lblFilter.Text = "ID,[ITSID1],[ITSGender1],[AttendanceException1],Branch,ITSID,KHNO,Name,Mobile,Email,[Joining Date]";
                    ViewState["isSearch"] = "0";
                }
           
    }

 protected string GetSelectedItems()
    {
            string selectedItems = ",";
            for (int i = 0; i < cblJHSCol.Items.Count; i++)
            {
                if (cblJHSCol.Items[i].Selected)
                {
                    selectedItems += cblJHSCol.Items[i].Value + ",";
                }
            }
            
            return selectedItems.TrimEnd(',');
    }

    protected string GetSelectedItemsForSearchField()
    {
            string selectedItems = "";
            for (int i = 0; i < cblJHSCol.Items.Count; i++)
            {
                if (cblJHSCol.Items[i].Selected)
                {
                    selectedItems += "CAST(" + cblJHSCol.Items[i].Value + " as nvarchar) +'' ''+ ";
                }
            }

            return selectedItems.TrimEnd(',');
    }

    protected void ClearFilter()
    {
            for (int i = 0; i < cblJHSCol.Items.Count; i++)
            {
                if (cblJHSCol.Items[i].Selected)
                {
                    cblJHSCol.Items[i].Selected = false;
                }
            }
    }

    protected void AddColumnsToGridView(GridView gv, DataTable table)
    {
            foreach (DataColumn column in table.Columns)
            {
                BoundField field = new BoundField();
                field.DataField = column.ColumnName;
                field.HeaderText = column.ColumnName;
                if (column.ColumnName == "NameAR" || column.ColumnName == "Father NameAR" || column.ColumnName == "Mother NameAR")
                {
                    field.ItemStyle.CssClass = "makeThisArabic";
                }
                gv.Columns.Add(field);
            }
    }

    protected void SearchTeachers(int spageindex)
    {

            DataTable dtSearTeachers = new DataTable();
            cTeachers objTeachers = new cTeachers();
            objTeachers.BranchID = ddlFillBranch.SelectedValue.ToString();
            objTeachers.StaffID = ddlStaff.SelectedValue.ToString();
            objTeachers.AlAzhar = ddlAlAzhar.SelectedValue;
            objTeachers.UserID = Convert.ToInt32(Session["UserID"].ToString());

            objTeachers.Field = lblFilter.Text;
            objTeachers.SearchField = SearchField;
            dtSearTeachers = objTeachers.SearchTeachersNew();


            dtSearTeachers.DefaultView.Sort = txtLastSortColumn.Text;
            pds.DataSource = dtSearTeachers.DefaultView;
            pds.AllowPaging = true;
            pds.PageSize = 10;  // grdTeachersSelectAll.PageSize;
            pds.CurrentPageIndex = spageindex;
            lblCurrentPageIndex.Text = Convert.ToString(spageindex);
            lnkbtnPrevious.Enabled = !(pds.IsFirstPage);
            lnkbtnNext.Enabled = !(pds.IsLastPage);
            intRecordCount.Text = Convert.ToString(dtSearTeachers.Rows.Count);

            if (dtSearTeachers != null && dtSearTeachers.Rows.Count > 0)
            {
                DivNoRecord.Visible = false;
                InkExport.Visible = true;
                DivTeacher.Visible = true;

                if (IsPostBack)
                {
                    for (int i = 0; i < grdTeachersSelectAll.Columns.Count; i++)
                    {
                        if (grdTeachersSelectAll.Columns[i].HeaderText != "Photos" &&
                            grdTeachersSelectAll.Columns[i].HeaderText != "Exception" &&
                            grdTeachersSelectAll.Columns[i].HeaderText != "Action")
                        {
                            grdTeachersSelectAll.Columns[i].Visible = false;
                        }
                    }
                }


                //Dynamic Colunms
                AddColumnsToGridView(grdTeachersSelectAll, dtSearTeachers);

                grdTeachersSelectAll.DataSource = pds;
                grdTeachersSelectAll.DataBind();


                for (int i = 0; i < grdTeachersSelectAll.Columns.Count; i++)
                {
                    if (grdTeachersSelectAll.Columns[i].HeaderText == "ID" ||
                        grdTeachersSelectAll.Columns[i].HeaderText == "ITSID1" ||
                        grdTeachersSelectAll.Columns[i].HeaderText == "ITSGender1" ||
                        grdTeachersSelectAll.Columns[i].HeaderText == "AttendanceException1")
                    {
                        grdTeachersSelectAll.Columns[i].Visible = false;
                    }

                    if (Request.RawUrl.ToString().ToUpper().Contains(HOUrl))
                    {
                        if (Session["IsAdmin"] != null)
                        {
                            if (Convert.ToBoolean(Session["IsAdmin"]) == false && grdTeachersSelectAll.Columns[i].HeaderText == "Exception")
                            {
                                grdTeachersSelectAll.Columns[i].Visible = false;
                            }
                            else
                            {
                                for (int j = 0; j < grdTeachersSelectAll.Rows.Count; j++)
                                {
                                    CheckBox cb = (CheckBox)grdTeachersSelectAll.Rows[j].FindControl("chkException");
                                    // cb.CheckedChanged += chkException_CheckedChanged;
                                }
                            }
                        }
                    }
                }
            }
            else
            {
                DivNoRecord.Visible = true;
                InkExport.Visible = false;
                DivTeacher.Visible = false;
            }





            StringBuilder strRecordSummary = new StringBuilder();

            //Showing 1 to 12 of 12 (1 Pages
            strRecordSummary.Append("Showing ");
            // Creating X f X Records 
            int floor = (spageindex * pds.PageSize) + 1;
            strRecordSummary.Append(floor.ToString());
            strRecordSummary.Append(" to ");
            int ceil = ((spageindex * pds.PageSize) + pds.PageSize);
            if (ceil > Convert.ToInt32(intRecordCount.Text))
            {
                strRecordSummary.Append(intRecordCount.Text.ToString());
            }
            else
            {
                strRecordSummary.Append(ceil.ToString());
            }
            // Displaying Total number of records Creating X of X of X records. 
            strRecordSummary.Append(" of ");
            strRecordSummary.Append(intRecordCount.Text.ToString());
            strRecordSummary.Append(" entries ");
            lblRecordSummary.Text = strRecordSummary.ToString();

            doPaging();

            if (dtSearTeachers.Rows.Count < 11)
            {
                tblPaging.Visible = false;
                lblRecordSummary.Visible = false;

            }
            else
            {
                tblPaging.Visible = true;
                lblRecordSummary.Visible = true;
            }
        }
        catch (Exception Exp)
        {
            ErrorDiv.Visible = true;
            lblError.Text = Exp.Message + " " + Exp.StackTrace;
            objBasePage.ErrorEmail(Exp.Message.ToString(), Exp.StackTrace.ToString());
        }
    }



    protected void grdTeachersSelectAll_RowCreated(object sender, GridViewRowEventArgs e)
    {
        try
        {
            GridViewRow row = e.Row;
            List<TableCell> columns = new List<TableCell>();
            TableCell cell = new TableCell();

            // For Attendance Exception
            //Get the first Cell /Column
            cell = row.Cells[1];
            // Then Remove it after
            row.Cells.Remove(cell);
            //And Add it to the List Collections
            columns.Add(cell);
            // Add cells
            row.Cells.AddRange(columns.ToArray());

            // For Action
            //Get the first Cell /Column
            cell = row.Cells[1];
            // Then Remove it after
            row.Cells.Remove(cell);
            //And Add it to the List Collections
            columns.Add(cell);
            // Add cells
            row.Cells.AddRange(columns.ToArray());
        }
        catch (Exception Exp)
        {
            ErrorDiv.Visible = true;
            lblError.Text = Exp.Message + " " + Exp.StackTrace;
            objBasePage.ErrorEmail(Exp.Message.ToString(), Exp.StackTrace.ToString());
        }
    }



    protected void btnFilter_Click(object sender, EventArgs e)
    {
        try
        {
            //Fixed Colunms
            lblFilter.Text = "ID,[ITSID1],[ITSGender1],[AttendanceException1],Branch,ITSID,KHNO,Name,Mobile,Email,[Joining Date]";
            lblFilter.Text += GetSelectedItems();
            SearchTeachers(0);
        }
        catch (Exception Exp)
        {
            ErrorDiv.Visible = true;
            lblError.Text = Exp.Message + " " + Exp.StackTrace;
            objBasePage.ErrorEmail(Exp.Message.ToString(), Exp.StackTrace.ToString());
        }
    }


    protected void btnSearchField_Click(object sender, EventArgs e)
    {
        try
        {
            SearchField = "";
            SearchField += GetSelectedItemsForSearchField();
            SearchField += "like '' %" + txtSearchField.Text + "% ''";
            SearchTeachers(0);
        }
        catch (Exception Exp)
        {
            ErrorDiv.Visible = true;
            lblError.Text = Exp.Message + " " + Exp.StackTrace;
            objBasePage.ErrorEmail(Exp.Message.ToString(), Exp.StackTrace.ToString());
        }
    }

----------------------------------------------------------------------




----------------------------------------------------------------------
-- Dynamically using controls for Fast Add

-----> Parent Control

--> Asp

<%@ Register Src="../MyControls/UDCLmsLanguage.ascx" TagName="UDCLmsLanguage" TagPrefix="uc1" %>

<uc1:UDCLmsLanguage ID="UDCLmsLanguage" runat="server" />

--> C# 

    // Delegation work for fast add.  
    delegate void DelUserControlMehtodDigital();

    protected void Page_Load(object sender, EventArgs e)
    {
            DelUserControlMehtodDigital delUserControlLmsLanguageDigital = new DelUserControlMehtodDigital(FillLanguageManageDigital);
            UDCLmsLanguage.CallingPageMethodDigital = delUserControlLmsLanguageDigital;
    }

    

-----> Child Control

--> Asp
 <asp:Button ID="btnSave" runat="server" Text="Add" CssClass="btn btn-success" ValidationGroup="LmsLanguageGroup" OnClientClick="return ConfirmLmsLanguage();" OnClick="btnSave_Click" />

--> C# 

// Delegation work for fast add on book page.

    private System.Delegate _delPageMethod;

    public Delegate CallingPageMethod
    {
        set { _delPageMethod = value; }
    }


   public void MyPageLoad(string pageCall)
    {
        try
        {
            hdnPageCall.Value = pageCall;
            ddlBranch.SelectedValue = Session["BranchID"].ToString();
            FillBranch();
            txtMode.Text = "Add";
            btnAdd_Click(new object(), new EventArgs());
        }
        catch (Exception Exp)
        {
            ErrorDiv.Visible = true;
            lblError.Text = Exp.Message + " " + Exp.StackTrace;
            objBasePage.ErrorEmail(Exp.Message.ToString(), Exp.StackTrace.ToString());
        }
    }



    protected void btnSave_Click(object sender, EventArgs e)
    {
        try
        {
            objLMS.BranchID = ddlBranch.SelectedValue;
            objLMS.Language = txtLmsLanguage.Text;
            objLMS.LanguageAR = txtLmsLanguageAR.Text;
            objLMS.IsActive = chkStatus.Checked.ToString();
            objLMS.LmsLanguageID = "0";
                if (objLMS.LmsLanguageInsertUpdate())
                {
                    SuccessDiv.Visible = true;
                    lblSuccess.Text = "Saved Successfully";
                    FillLanguage();
                    ddlStatus.DataBind();

                    // Delegation work for fast add on book page.
                    if (Request.RawUrl.ToString().ToUpper().Contains(BookAddUrl))
                    {
                        if (hdnPageCall.Value == "Digital")
                        {
                            _delPageMethodDigital.DynamicInvoke();
                        }
                        else
                        {
                            _delPageMethod.DynamicInvoke();
                        }
                    }
                    else
                    {
                        LoadLmsLanguage();
                    }
                }
                else
                {
                    ErrorDivPopup.Visible = true;
                    lblErrorPopup.Text = objLMS.sErrorMessage;
                    mp1.Show();
                }

    }
    
    
    
    ----------------------------------------------------------------------
    
    
    ----------------------------------------------------------------------
-- Dynamic function for single click even validation of page is fired.

--> Asp Code

<script type="text/javascript">
    function ExecuteCodeClick(control) {
        if (typeof (Page_ClientValidate) == 'function' && Page_ClientValidate('SampleGroup') == false) {
            control.click();
            control.click();
        }
    }
</script>

<asp:Button ID="btnAddClassCode" Text="+" CssClass="inline-btn" runat="server"
            OnClientClick="return ExecuteCodeClick(this);" OnClick="btnAddClassCode_Click" TabIndex="5"></asp:Button>


--> C# Code

    protected void btnAddClassCode_Click(object sender, EventArgs e)
    {
        try
        {
            UDCLmsClassCode.MyPageLoad("Physical");
        }
        catch (ThreadAbortException)
        {
            // ignore it because we know that come from the redirect
        }
        catch (Exception Exp)
        {
            ErrorDiv.Visible = true;
            lblError.Text = Exp.Message + " " + Exp.StackTrace;
            objBasePage.ErrorEmail(Exp.Message.ToString(), Exp.StackTrace.ToString());
        }
    }


----------------------------------------------------------------------

----------------------------------------------------------------------
-- Code to validate image size before uploading file.


--> ASPX Code

<asp:CustomValidator ID="customValidatorUpload" runat="server" ErrorMessage="" ControlToValidate="fileUpload" ClientValidationFunction="checkImageSize" />
<asp:Button ID="button_fileUpload" runat="server" Text="Upload File" OnClick="button_fileUpload_Click" Enabled="false" />
<asp:Label ID="lbl_uploadMessage" runat="server" Text="" />

<script type="text/javascript">
    function checkImageSize(source, args) {
        var minFileSize = 6144; // 6Kb -> 6 * 1024
        var maxFileSize = 10240; // 10Kb -> 10 * 1024
        var fileUpload = $get('<%= fuImage.ClientID%>');
        var lblUploadMessage = $get('<%= lblUploadMessage.ClientID%>');
        if (fileUpload.value == '') {
            return false;
        }
        else {
            var file = fileUpload.files[0];
            if (file.size > minFileSize && file.size < maxFileSize) {
                args.IsValid = true;
                lblUploadMessage.style.display = 'none';
                return true;
            } else {
                args.IsValid = false;
                lblUploadMessage.innerText = 'File size is not acceptable.';
                lblUploadMessage.style.display = 'inline';
                return false;
            }
        }
    }
</script>



----------------------------------------------------------------------

----------------------------------------------------------------------
-- Query to separate charater and digits in column.


--> SQL
DECLARE @string varchar(100)

SET @string = (SELECT  Top (1)   AccessionNo FROM  LmsBook WHERE (BranchID = @BranchID) And LmsBookCategoryID=@LmsBookCategoryID Order By  LmsBookID Desc)

WHILE PATINDEX('%[^0-9]%',@string) <> 0
SET @string = STUFF(@string,PATINDEX('%[^0-9]%',@string),1,'')

SELECT IsNull(@string ,0)as AccessionNo

----------------------------------------------------------------------


----------------------------------------------------------------------
-- Dynamic validation for gridview.


--> Aspx Code

<script type="text/javascript">

    function CheckValidation() {
        var gv = document.getElementById('<%= gvLmsBookRoleRequestListing.ClientID %>');
        var gvRowCount = gv.rows.length - 1;
        var rwIndex = 0;
        var IsValid = true;
        for (rwIndex; rwIndex <= gvRowCount - 1; rwIndex++) {

            var txtFromDate = document.getElementById("ContentPlaceHolder1_UDCLmsBookRoleRequest_gvLmsBookRoleRequestListing_txtFromDate_" + rwIndex + "");
            var txtToDate = document.getElementById("ContentPlaceHolder1_UDCLmsBookRoleRequest_gvLmsBookRoleRequestListing_txtToDate_" + rwIndex + "");
            var ReqFromDate = document.getElementById("ContentPlaceHolder1_UDCLmsBookRoleRequest_gvLmsBookRoleRequestListing_ReqFromDate_" + rwIndex + "");
            var ReqToDate = document.getElementById("ContentPlaceHolder1_UDCLmsBookRoleRequest_gvLmsBookRoleRequestListing_ReqToDate_" + rwIndex + "");
            var chkSubmit = document.getElementById("ContentPlaceHolder1_UDCLmsBookRoleRequest_gvLmsBookRoleRequestListing_chkSubmit_" + rwIndex + "");

            if (chkSubmit.checked) {
                if (txtFromDate.value == '') {
                    ValidatorEnable(ReqFromDate, true);
                    IsValid = false;
                }
                if (txtToDate.value == '') {
                    ValidatorEnable(ReqToDate, true);
                    IsValid = false;
                }
            }
        }
        return IsValid;
    }
</script>

----------------------------------------------------------------------

----------------------------------------------------------------------
--> File exists or not 

--> C#
 if (File.Exists(Server.MapPath("~/assets/img/DefualtBook.png")))
                    {
                       
                    }
                    
                    
----------------------------------------------------------------------

----------------------------------------------------------------------
--> Remove item from dropdown 

--> C#                  
                    
                    
    ListItemCollection liCol = ddlUserType.Items;
                for (int i = 0; i < liCol.Count; i++)
                {
                    ListItem listitem = liCol[i];
                    if (listitem.Value == "8")
                        ddlUserType.Items.Remove(listitem);
                }      
                
                
 ----------------------------------------------------------------------      
 
 ----------------------------------------------------------------------
--> Script for submit all button for gridview with checkboxes and text fields

--> Javascript
 <script type="text/javascript">

    function CheckValidation() {
        var gv = document.getElementById('<%= gvLmsBookRoleRequestListing.ClientID %>');
        var gvRowCount = gv.rows.length - 1;
        var rwIndex = 0;
        var IsValid = true;
        var IsChk = false;
        for (rwIndex; rwIndex <= gvRowCount - 1; rwIndex++) {
            var txtFromDate = document.getElementById("ContentPlaceHolder1_UDCLmsBookRoleRequest_gvLmsBookRoleRequestListing_txtFromDate_" + rwIndex + "");
            var txtToDate = document.getElementById("ContentPlaceHolder1_UDCLmsBookRoleRequest_gvLmsBookRoleRequestListing_txtToDate_" + rwIndex + "");
            var ReqFromDate = document.getElementById("ContentPlaceHolder1_UDCLmsBookRoleRequest_gvLmsBookRoleRequestListing_ReqFromDate_" + rwIndex + "");
            var ReqToDate = document.getElementById("ContentPlaceHolder1_UDCLmsBookRoleRequest_gvLmsBookRoleRequestListing_ReqToDate_" + rwIndex + "");
            var chkSubmit = document.getElementById("ContentPlaceHolder1_UDCLmsBookRoleRequest_gvLmsBookRoleRequestListing_chkSubmit_" + rwIndex + "");

            if (chkSubmit.checked) {
                IsChk = true;
                if (txtFromDate.value == '' || txtToDate.value == '') {
                    if (txtFromDate.value == '') {
                        ValidatorEnable(ReqFromDate, true);
                    }
                    if (txtToDate.value == '') {
                        ValidatorEnable(ReqToDate, true);
                    }
                    IsValid = false;
                }
            }
            else {
                ValidatorEnable(ReqFromDate, false);
                ValidatorEnable(ReqToDate, false);
            }
        }
        // Final check validation
        if (IsChk != true) {
            alert('Please select atleast one record.');
            return false;
        }
        else {
            if (IsChk && IsValid) {
                return confirm('Are you sure you want to edit this record? Do you want to proceed?');
            }
            else {
                return false;
            }
        }
    }
</script>


----------------------------------------------------------------------

----------------------------------------------------------------------
--> Script for select and deselect all checkboxes in gridview.

--> Javascript

<script type="text/javascript">
    // Function to checked and unchecked all chkboxes on header chkbox.
    function chkSubmitAllChecked(chkSubmitAll) {
        var gvLmsNewspaperEntryList = $get('<%= gvLmsBookRoleRequestListing.ClientID %>');

            // Get all input elements in Gridview
            var inputList = gvLmsNewspaperEntryList.getElementsByTagName("input");

            //Checked and Unchecked all chkboxes on header chkbox.
            for (var i = 0; i < inputList.length; i++) {
                if (inputList[i].type == "checkbox") {
                    if (chkSubmitAll.checked) {
                        inputList[i].checked = true;
                    }
                    else {
                        inputList[i].checked = false;
                    }
                }
            }
        }

        // Function to checked and unchecked  header chkbox on all chkboxes.
        function chkSubmitChecked(chkSubmit) {
            var gvLmsNewspaperEntryList = $get('<%= gvLmsBookRoleRequestListing.ClientID %>');
            var chkSubmitAll = $get('ContentPlaceHolder1_UDCLmsBookRoleRequest_gvLmsBookRoleRequestListing_chkSubmitAll');
            var checkHeader = true;
            // Get all input elements in Gridview
            var inputList = gvLmsNewspaperEntryList.getElementsByTagName("input");

            // Loop through all check boxes .
            for (var i = 0; i < inputList.length; i++) {
                if (inputList[i].type == "checkbox" && inputList[i].id != chkSubmitAll.id) {
                    if (!inputList[i].checked) {
                        checkHeader = false;
                        break;
                    }
                }
            }

            // Checked and unchecked header checkbox.
            if (checkHeader) {
                chkSubmitAll.checked = true;
            }
            else {
                chkSubmitAll.checked = false;
            }
        }
</script>

--> C# Code

  protected void gvLmsBookRoleRequestListing_RowDataBound(object sender, GridViewRowEventArgs e)
    {
        try
        {

            if (e.Row.RowType == DataControlRowType.Header)
            {
                CheckBox chkSubmitAll = (CheckBox)e.Row.FindControl("chkSubmitAll");
                chkSubmitAll.Attributes.Add("onclick", "chkSubmitAllChecked(this);");
            }
            if (e.Row.RowType == DataControlRowType.DataRow)
            {
                CheckBox chkSubmit = (CheckBox)e.Row.FindControl("chkSubmit");
                chkSubmit.Attributes.Add("onclick", "chkSubmitChecked(this);");
                 }    }
        }
        catch (Exception Exp)
        {
            ErrorDiv.Visible = true;
            lblError.Text = Exp.Message + " " + Exp.StackTrace;
            objBasePage.ErrorEmail(Exp.Message.ToString(), Exp.StackTrace.ToString());
        }

----------------------------------------------------------------------
----------------------------------------------------------------------
--> Regex for password contains atleast 8 charaters,1 lowercase ,1 uppercase,1 special character.

 ValidationExpression="((?=.*\d)(?=.*[a-z])(?=.*[A-Z])(?=.*\W).{8,})"
 
 ----------------------------------------------------------------------
 
 ----------------------------------------------------------------------
--> Text changed event from javascript

-> Javascript
    function texboxchange(control) {
        var count = control.value.length;
        if (count > 0) {
            __doPostBack('<%=txtITSID.ClientID %>', "TextChanged");
        }
    }

-> Asp
<asp:TextBox ID="txtITSID" runat="server" class="form-control" MaxLength="8" OnTextChanged="txtITSID_TextChanged" OnChange="return texboxchange(this);"
 ValidationGroup="LmsUserAddGroup" autocomplete="off"></asp:TextBox>
----------------------------------------------------------------------

