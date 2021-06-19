## Triple G Coach Service Application

**Project description:** The application represents an online booking and search feature for an online coach service. 

### 1. Overview

This is an .NET application which was built in Visual Studio 2017 using C# and Xamarin, with SQL for data management (SQL Server management studio was used as the method for creation of Stored Procedures). The primary functions of the application are: account creation, Login, Input and Saving of payment details, Search features, payment method selection.  

### 2. Application Examples

<img src="images/TripleGHome.png"/>
<img src="images/TripleGSearch.png"/>
<img src="images/TripleGCards.png"/>
<img src="images/TripleGTicket.png"/>
<img src="images/TripleGSQL.png"/>

### 3. Code Snippets

SQL Connection + Use
```C#
    public static class ConnectionHelper
    {
        public static string ConnectionVal(string TripleG_SQLDBConnectionString)
        {
            TripleG_SQLDBConnectionString = ConfigurationManager.ConnectionStrings["TripleG_SQLDBConnectionString"].ConnectionString;

            return TripleG_SQLDBConnectionString;
        }
    }
```
```C#
        public List<cCoach> GetAnyValueOfSearchBoxIns(string searchBoxIns)
        {
            using (IDbConnection connection = new System.Data.SqlClient.SqlConnection(ConnectionHelper.ConnectionVal("TripleG_SQLDBConnectionString")))
            {
                var output = connection.Query<cCoach>("dbo.spCoach_SearchAllCoachInfo @LeavingFrom, @ArrivalDestination, @JourneyDate, @Price", new { LeavingFrom = searchBoxIns, ArrivalDestination = searchBoxIns, JourneyDate = searchBoxIns, Price = searchBoxIns }).ToList();

                return output;
            }
        }
```

User Interface
```Xamarin
<Grid>
        <StackPanel Orientation="Vertical">
            <TextBlock Text="Timetable" VerticalAlignment="Top" Margin="355,0,0,0" FontSize="20" TextDecorations="Underline" Foreground="FloralWhite"/>

            <StackPanel Orientation="Horizontal" Margin="0,10,0,0">
                <ComboBox Name="SelectionSearchFieldBox" Text="Select Seach Field" Width="150" Margin="100,0,0,0" IsEditable="True" IsReadOnly="True" Background="Beige" Foreground="Black" BorderBrush="Beige" HorizontalContentAlignment="Center">
                    <ComboBoxItem Name="leavingFromSelection"        Content="Leaving From"        Background="Beige" Foreground="Black" BorderBrush="Beige"/>
                    <ComboBoxItem Name="arrivalDestinationSelection" Content="Arrival Destination" Background="Beige" Foreground="Black" BorderBrush="Beige"/>
                    <ComboBoxItem Name="journeyDateSelection"        Content="Journey Date"        Background="Beige" Foreground="Black" BorderBrush="Beige"/>
                    <ComboBoxItem Name="priceSelection"              Content="Price"               Background="Beige" Foreground="Black" BorderBrush="Beige"/>
                </ComboBox>
                <TextBox Name="SearchFieldText" MinWidth="300" FontSize="14"/>
                <Button  Name="SearchButton" MinWidth="100" HorizontalAlignment="Center" Content="Search" FontSize="14" Click="SearchButton_Click" Background="FloralWhite" Foreground="Black" BorderBrush="DarkGray" BorderThickness="2"/>
            </StackPanel>

        </StackPanel>

        <Frame x:Name="TimetableFrame" Margin="0,65,0,0" NavigationUIVisibility="Hidden"/>

        <ListView x:Name="dataFoundListBox" Height="250" Width="650" Margin="0,50,0,0" HorizontalContentAlignment="Center" ScrollViewer.HorizontalScrollBarVisibility="Disabled" SelectionChanged="DataFoundListBox_SelectionChanged">
            <ListView.View>
                <GridView>
                    <GridViewColumn x:Name="hLeavingFrom"        Header="Leaving From"        Width="130"/>
                    <GridViewColumn x:Name="hArrivalDestination" Header="Arrival Destination" Width="130"/>
                    <GridViewColumn x:Name="hJourneyTime"        Header="Journey Time"        Width="130"/>
                    <GridViewColumn x:Name="hJourneyDate"        Header="Journey Date"        Width="130"/>
                    <GridViewColumn x:Name="hPrice"              Header="Price"               Width="130"/>
                </GridView>
            </ListView.View>
        </ListView>

        <Popup VerticalOffset="-300" HorizontalOffset="780" x:Name="BuyTicketPopup">
            <Border BorderBrush="Black" Background="Beige" BorderThickness="2" Width="400" Height="200">
                <StackPanel HorizontalAlignment="Center" Orientation="Vertical" Width="400">

                    <StackPanel Orientation="Horizontal" HorizontalAlignment="Center">
                        <TextBlock Text="Ticket" FontSize="14" TextDecorations="Underline" HorizontalAlignment="Center" FontWeight="Bold" Margin="170,0,0,0"/>
                        <Button Name="CloseButton" Content="X" Width="20" Height="20" VerticalAlignment="Top" Margin="150,0,0,0" Background="DarkRed" Foreground="LightGray" BorderBrush="LightGray" BorderThickness="2" HorizontalContentAlignment="Center" VerticalContentAlignment="Stretch" Click="CloseButton_Click"/>
                    </StackPanel>
                    
                    <ListView Name="popUpCreditCardListBox" Width="400" Height="80" BorderBrush="Black" BorderThickness="1" ScrollViewer.HorizontalScrollBarVisibility="Disabled">
                        <ListView.View>
                            <GridView>
                                <GridViewColumn x:Name="popUpSelectCard" Header="Select Card" Width="400"/>
                            </GridView>
                        </ListView.View>
                    </ListView>
                    
                    <ListView Name="popUpJourneyDetails" Width="400" Height="45" Margin="0,10,0,0" ScrollViewer.HorizontalScrollBarVisibility="Disabled">
                        <ListView.View>
                            <GridView>
                                <GridViewColumn x:Name="popUpJdJourneyID" Header="JourneyID" Width="66"/>
                                <GridViewColumn x:Name="popUpJdLeavingFrom" Header="Leaving From" Width="76"/>
                                <GridViewColumn x:Name="popUpJdArrivalDestination" Header="Arriving At" Width="76"/>
                                <GridViewColumn x:Name="poUpJdJourneyTime" Header="Time" Width="63"/>
                                <GridViewColumn x:Name="popUpJdJourneyDate" Header="Date" Width="60"/>
                                <GridViewColumn x:Name="popUpJdPrice" Header="Price" Width="60"/>
                            </GridView>
                        </ListView.View>
                    </ListView>
                    
                    <Button Content="Purchase" FontSize="14" BorderBrush="Black" BorderThickness="1" Margin="0,10,0,0" Width="100" Click="Button_Click"/>
                    
                </StackPanel>
            </Border>
        </Popup>

        <Popup Name="popUpReciptOfPurchase" VerticalOffset="-90" HorizontalOffset="780">
            <Border Width="400" Height="170" Background="Beige" BorderBrush="Black" BorderThickness="1">
                <StackPanel>

                    <StackPanel Orientation="Horizontal" HorizontalAlignment="Center">
                        <TextBlock Text="Recipt" FontSize="14" TextDecorations="Underline" HorizontalAlignment="Center" FontWeight="Bold" Margin="170,0,0,0"/>
                        <Button Name="CloseBtn" Content="X" Width="20" Height="20" VerticalAlignment="Top" Margin="150,0,0,0" Background="DarkRed" Foreground="LightGray" BorderBrush="LightGray" BorderThickness="2" HorizontalContentAlignment="Center" VerticalContentAlignment="Stretch" Click="CloseBtn_Click" />
                    </StackPanel>

                    <StackPanel Orientation="Horizontal">

                        <ListView Width="auto" Height="50" FontSize="14" Name="ReciptCardListView" ScrollViewer.HorizontalScrollBarVisibility="Disabled" BorderBrush="Transparent" BorderThickness="0">
                            <ListView.View>
                                <GridView>
                                    <GridViewColumn x:Name="reciptCard" Header="Card" Width="110"/>
                                </GridView>
                            </ListView.View>
                        </ListView>
                        
                        <ListView Width="auto" Height="50" FontSize="14" Name="ReciptListView" ScrollViewer.HorizontalScrollBarVisibility="Disabled" BorderBrush="Transparent" BorderThickness="0" Margin="-4,0,0,0">
                            <ListView.View>
                                <GridView>
                                    <GridViewColumn x:Name="reciptCoachID"       Header="Coach ID"       Width="75"/>
                                    <GridViewColumn x:Name="reciptLeavingFrom"   Header="Leaving From"   Width="100"/>
                                    <GridViewColumn x:Name="reciptToDestination" Header="To Destination" Width="106"/>
                                </GridView>
                            </ListView.View>
                        </ListView>
                    </StackPanel>

                    <TextBlock Text="Thankyou for booking with Triple G online coach serives" TextAlignment="Center" Margin="0,10,0,0" FontSize="14"/>
                    <TextBlock Text="Have a safe journey, we hope to see you soon" TextAlignment="Center" Margin="0,10,0,0" FontSize="14"/>
                    <TextBlock Text="Payment Sucessful" TextAlignment="Center" Margin="0,10,0,0" FontSize="14"/>

                </StackPanel>
            </Border>
        </Popup>
    </Grid>
```

Account Creation
```C#
public partial class CreateAccountPage : Page
    {
        List<cAccount> UsernameList = new List<cAccount>();

        public CreateAccountPage()
        {
            InitializeComponent();
        }

        private void CreateAccountBtn_Click(object sender, RoutedEventArgs e)
        {
            if(passwordInsTextbox.Text == re_EnterPasswordInsTextbox.Text && userNameAvailabliltyStatus.Background == Brushes.LightGreen)
            {
                DataAccessFromDB dataAccessFromDB = new DataAccessFromDB();

                dataAccessFromDB.CreateAccount(userNameInsTextbox.Text, passwordInsTextbox.Text, firstNameIns.Text, lastNameIns.Text, emailAddressIns.Text, phoneNumberIns.Text );

                firstNameIns.Text = "";
                lastNameIns.Text = "";
                emailAddressIns.Text = "";
                phoneNumberIns.Text = "";

                userNameInsTextbox.Text = "";
                passwordInsTextbox.Text = "";
                re_EnterPasswordInsTextbox.Text = "";

                errorMessageDisplay.Text = "Account Creation SucessFul, Please Log In";
            }
            else if (passwordInsTextbox.Text != re_EnterPasswordInsTextbox.Text)
            {
                errorMessageDisplay.Text = "Entered passwords do not match, please try again";

                passwordInsTextbox.Text = "";
                re_EnterPasswordInsTextbox.Text = "";
            }
            else if(userNameAvailabliltyStatus.Background == Brushes.Red || userNameInsTextbox.Text == "")
            {
                errorMessageDisplay.Text = "Entered Username is unavailable, please enter a different Username";

                userNameInsTextbox.Text = "";

                passwordInsTextbox.Text = "";
                re_EnterPasswordInsTextbox.Text = "";
            }
            else if (passwordInsTextbox.Text == null || passwordInsTextbox.Text == " ")
            {
                errorMessageDisplay.Text = "Spaces are not allowed in the password";

                passwordInsTextbox.Text = "";
                re_EnterPasswordInsTextbox.Text = "";
            }
        }

        private void UserNameInsTextbox_TextChanged(object sender, TextChangedEventArgs e)
        {
            DataAccessFromDB dataAccessFromDB = new DataAccessFromDB();

            UsernameList = dataAccessFromDB.UserNameCheck(userNameInsTextbox.Text);

            UpdateListBoxBindingOfUsername();
        }

        private void UpdateListBoxBindingOfUsername()
        {
            userNameAvailabliltyStatus.ItemsSource = UsernameList;

            if(UsernameList.Count == 0)
            {
                userNameAvailabliltyStatus.Background = Brushes.LightGreen;
                userNameAvailabliltyStatus.Foreground = Brushes.LightGreen;
            }
            else
            {
                userNameAvailabliltyStatus.Background = Brushes.Red;
                userNameAvailabliltyStatus.Foreground = Brushes.Red;
            }
        }
    }
```

### 4. SQL Snippets

Username and password for login credentials
```SQL
USE [TripleG_SQLDB]
GO
/****** Object:  StoredProcedure [dbo].[spAccount_GetUsernameAndPassword] ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
-- =============================================
-- Author:		<Name>
-- Create date: <Create Date>
-- Description:	<Return Count where userName and password from the same row are entered,,>
-- =============================================
ALTER PROCEDURE [dbo].[spAccount_GetUsernameAndPassword]
	-- Add the parameters for the stored procedure here
	@Username NVARCHAR(50),
	@Password VARCHAR(50)
AS
BEGIN
	-- SET NOCOUNT ON added to prevent extra result sets from
	-- interfering with SELECT statements.
	SET NOCOUNT ON;

    -- Insert statements for procedure here
	SELECT *

	FROM dbo.Account

	WHERE Username = @Username AND Password = @Password;
END
```
