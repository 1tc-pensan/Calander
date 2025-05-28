# Calendar és DatePicker WPF-ben

Ez a dokumentáció a megadott C# WPF alkalmazás, a "Calendar" működését és kódját magyarázza el röviden és érthetően.

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
namespace calandar
{
    /// <summary>
    /// Interaction logic for MainWindow.xaml
    /// </summary>
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
        }

        private void Register_Click(object sender, RoutedEventArgs e)
        {
            if (sender is Button button)
            {
                if (AszfCheckBox.IsChecked == true && UserNameTextBox.Text != null && PasswordBox.Password != null && YearOver18())
                {
                    MessageBox.Show("Sikeresen regisztráltál");
                }
                else
                {
                    MessageBox.Show("Érvénytelen adatok");
                }

            }
        }
        private bool YearOver18()
        {
            bool result = false;
            bool b = BirthDayDatePicker.SelectedDate.Value.Date > DateTime.Now.AddYears(-18);
            System.Diagnostics.Debug.WriteLine($"18-nál idősebb{b}");
            if (BirthDayDatePicker.SelectedDate.Value.Date < DateTime.Now.AddYears(-18))
            {
                result = true;
            }
            return result;
        }

        private void AszfButton_Click(object sender, RoutedEventArgs e)
        {
            MessageBox.Show("A jelen alkalmazás használata a felhasználó által az ÁSZF elfogadását jelenti.\r\n\r\nAz alkalmazás célja, hogy funkcionális és megbízható szolgáltatást nyújtson felhasználói számára.\r\n\r\nAz alkalmazás minden tartalma és funkciója szerzői jogvédelem alatt áll.\r\n\r\nA fejlesztő nem vállal felelősséget az esetleges adatvesztésért vagy hibás működésből eredő károkért.\r\n\r\nA felhasználó köteles az alkalmazást jogszerűen és rendeltetésszerűen használni.\r\n\r\nAz alkalmazás használata ingyenes/fizetős – kérjük, ellenőrizze az adott verzió feltételeit.\r\n\r\nA fejlesztő fenntartja a jogot az alkalmazás frissítésére vagy módosítására előzetes értesítés nélkül.\r\n\r\nA személyes adatok kezelése az adatvédelmi tájékoztató szerint történik.\r\n\r\nBármely jogvita esetén a magyar jog az irányadó.\r\n\r\nA jelen ÁSZF módosítható, a frissített verziók a letöltéssel vagy frissítéssel automatikusan érvénybe lépnek.", "Általános Szerződési Feltételek");
        }
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
