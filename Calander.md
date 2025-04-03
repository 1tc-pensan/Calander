# Calendar és DatePicker WPF-ben

Ez a dokumentum egy egyszerű WPF alkalmazás kódját mutatja be, amely egy `Calendar` és egy `DatePicker` vezérlőt használ a dátum kiválasztására.

## XAML kód (MainWindow.xaml)

```xaml
<Window x:Class="WpfApp3.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        Title="Calendar és DatePicker" Height="450" Width="800">
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
</Window>

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

A Grid egy egyszerű elrendezést biztosít, benne egy StackPanel, amely egymás alá rendezi az elemeket.
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

1.A Calendar egy naptár, ahol egyetlen dátumot lehet kiválasztani (SelectionMode="SingleDate").

<TextBlock Text="Naptár:" Margin="0,0,0,5"/>
<Calendar x:Name="myCalendar" SelectionMode="SingleDate"/>

2.A DatePicker egy dátumválasztó, amely legördülő naptárral és kézi bevitellel is működik.

<TextBlock Text="Dátumválasztó:" Margin="0,20,0,5"/>
            <DatePicker x:Name="myDatePicker"/>

3.A Button_Click esemény hatására a program kiírja a Calendar és a DatePicker által kiválasztott dátumokat a TextBlock-ba. Ha nincs kiválasztva dátum, a "nincs kiválasztva" szöveg jelenik meg.

 private void Button_Click(object sender, RoutedEventArgs e)
    {
        DateTime? selectedCalendarDate = myCalendar.SelectedDate;
        DateTime? selectedPickerDate = myDatePicker.SelectedDate;
        selectedDateText.Text = $"Mai dátum: {selectedCalendarDate?.ToString("yyyy.MM.dd") ?? "nincs kiválasztva"}\n" +
                              $"Kiválasztott dátum: {selectedPickerDate?.ToString("yyyy.MM.dd") ?? "nincs kiválasztva"}";
    }

A DateTime? típust használjuk, mert a kiválasztott dátum lehet null, ha a felhasználó még nem jelölt ki semmit.
DateTime? selectedCalendarDate = myCalendar.SelectedDate;
