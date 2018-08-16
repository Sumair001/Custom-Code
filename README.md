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
