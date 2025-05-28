# Calendar és DatePicker WPF-ben

Ez a dokumentum egy egyszerű WPF alkalmazás kódját mutatja be, amely egy `Calendar` és egy `DatePicker` vezérlőt használ a dátum kiválasztására.

## XAML kód (MainWindow.xaml)

```xaml
<Window x:Class="calandar.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:calandar"
        mc:Ignorable="d"
        Title="MainWindow" Height="450" Width="800">
    <Grid>
        <Grid>
            <StackPanel Orientation="Horizontal" Margin="10,10,0,0">
                <StackPanel Orientation="Vertical">
                    <TextBlock Text="Felhasználó név:" FontSize="20"/>
                    <TextBlock Text="Jelszó:" FontSize="20"/>
                    <TextBlock Text="Születési dátum: " FontSize="20"/>
                    <Button x:Name="AszfButton" Content="Általános Szerőzdési feltételek" Margin="0,10,10,0" Click="AszfButton_Click" ></Button>
                </StackPanel>
                <StackPanel Orientation="Vertical">
                    <TextBox x:Name="UserNameTextBox" Width="200"/>
                    <PasswordBox x:Name="PasswordBox" Width="200"/>
                    <DatePicker x:Name="BirthDayDatePicker"></DatePicker>
                    <CheckBox x:Name="AszfCheckBox" Content="Ászf" FontSize="20" IsChecked="False"/>
                </StackPanel>
            </StackPanel>
            <Button x:Name="Register" Content="Regisztárico" Width="100" Height="40" Margin="138,137,162,57" Click="Register_Click"/>
        </Grid>
    </Grid>
</Window>
```
### Cs kód (MainWindow.xaml.cs)
```cs
namespace WpfApp3;

/// <summary>
/// Interaction logic for MainWindow.xaml
/// </summary>
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
    }

    private void Button_Click(object sender, RoutedEventArgs e)
    {
        DateTime? selectedCalendarDate = myCalendar.SelectedDate;
        DateTime? selectedPickerDate = myDatePicker.SelectedDate;
        selectedDateText.Text = $"Mai dátum: {selectedCalendarDate?.ToString("yyyy.MM.dd") ?? "nincs kiválasztva"}\n" +
                              $"Kiválasztott dátum: {selectedPickerDate?.ToString("yyyy.MM.dd") ?? "nincs kiválasztva"}";
    }
}
```
1. A Grid egy egyszerű elrendezést biztosít, benne egy StackPanel, amely egymás alá rendezi az elemeket.
```xaml
    <Grid>
        <StackPanel Margin="10">
            <TextBlock Text="Naptár:" Margin="0,0,0,5"/>
            <Calendar x:Name="myCalendar" SelectionMode="SingleDate"/>
            <TextBlock Text="Dátumválasztó:" Margin="0,20,0,5"/>
            <DatePicker x:Name="myDatePicker"/>
            <Button Content="Kiválasztott dátum mutatása" Click="Button_Click" Margin="0,20,0,0"/>
            <TextBlock x:Name="selectedDateText" Margin="0,10,0,0"/>
        </StackPanel>
    </Grid>
```
2. A Calendar egy naptár, ahol egyetlen dátumot lehet kiválasztani (SelectionMode="SingleDate").
```xaml
<TextBlock Text="Naptár:" Margin="0,0,0,5"/>
<Calendar x:Name="myCalendar" SelectionMode="SingleDate"/>
```
3. A DatePicker egy dátumválasztó, amely legördülő naptárral és kézi bevitellel is működik.
```xaml
<TextBlock Text="Dátumválasztó:" Margin="0,20,0,5"/>
            <DatePicker x:Name="myDatePicker"/>
```
4. A Button_Click esemény hatására a program kiírja a Calendar és a DatePicker által kiválasztott dátumokat a TextBlock-ba. Ha nincs kiválasztva dátum, a "nincs kiválasztva" szöveg jelenik meg.
```cs
 private void Button_Click(object sender, RoutedEventArgs e)
    {
        DateTime? selectedCalendarDate = myCalendar.SelectedDate;
        DateTime? selectedPickerDate = myDatePicker.SelectedDate;
        selectedDateText.Text = $"Mai dátum: {selectedCalendarDate?.ToString("yyyy.MM.dd") ?? "nincs kiválasztva"}\n" +
                              $"Kiválasztott dátum: {selectedPickerDate?.ToString("yyyy.MM.dd") ?? "nincs kiválasztva"}";
    }
```
5. A DateTime? típust használjuk, mert a kiválasztott dátum lehet null, ha a felhasználó még nem jelölt ki semmit.
```cs
DateTime? selectedCalendarDate = myCalendar.SelectedDate;
```
