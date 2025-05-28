# Calendar és DatePicker WPF-ben

Ez a dokumentáció a megadott C# WPF alkalmazás, a **Calendar** működését és kódját magyarázza el röviden és érthetően.

A **Calendar** egy egyszerű WPF alkalmazás, amely egy regisztrációs űrlapot biztosít a felhasználók számára. Az űrlap segítségével a felhasználók megadhatják nevüket, jelszavukat, születési dátumukat, és elfogadhatják az Általános Szerződési Feltételeket (ÁSZF). A regisztráció csak akkor sikeres, ha minden adat érvényes és a felhasználó elmúlt 18 éves.

---

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
                    <TextBlock Text="Születési dátum:" FontSize="20"/>
                    <Button x:Name="AszfButton" Content="Általános Szerőzdési feltételek" Margin="0,10,10,0" Click="AszfButton_Click"/>
                </StackPanel>
                <StackPanel Orientation="Vertical">
                    <TextBox x:Name="UserNameTextBox" Width="200"/>
                    <PasswordBox x:Name="PasswordBox" Width="200"/>
                    <DatePicker x:Name="BirthDayDatePicker"/>
                    <CheckBox x:Name="AszfCheckBox" Content="Ászf" FontSize="20" IsChecked="False"/>
                </StackPanel>
            </StackPanel>
            <Button x:Name="Register" Content="Regisztárico" Width="100" Height="40" Margin="138,137,162,57" Click="Register_Click"/>
        </Grid>
    </Grid>
</Window>
```

---

## C# kód (MainWindow.xaml.cs)

```csharp
namespace calandar
{
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
            System.Diagnostics.Debug.WriteLine($"18-nál idősebb: {b}");
            if (BirthDayDatePicker.SelectedDate.Value.Date < DateTime.Now.AddYears(-18))
            {
                result = true;
            }
            return result;
        }

        private void AszfButton_Click(object sender, RoutedEventArgs e)
        {
            MessageBox.Show(
                "A jelen alkalmazás használata a felhasználó által az ÁSZF elfogadását jelenti.\r\n\r\n" +
                "Az alkalmazás célja, hogy funkcionális és megbízható szolgáltatást nyújtson felhasználói számára.\r\n\r\n" +
                "Az alkalmazás minden tartalma és funkciója szerzői jogvédelem alatt áll.\r\n\r\n" +
                "A fejlesztő nem vállal felelősséget az esetleges adatvesztésért vagy hibás működésből eredő károkért.\r\n\r\n" +
                "A felhasználó köteles az alkalmazást jogszerűen és rendeltetésszerűen használni.\r\n\r\n" +
                "Az alkalmazás használata ingyenes/fizetős – kérjük, ellenőrizze az adott verzió feltételeit.\r\n\r\n" +
                "A fejlesztő fenntartja a jogot az alkalmazás frissítésére vagy módosítására előzetes értesítés nélkül.\r\n\r\n" +
                "A személyes adatok kezelése az adatvédelmi tájékoztató szerint történik.\r\n\r\n" +
                "Bármely jogvita esetén a magyar jog az irányadó.\r\n\r\n" +
                "A jelen ÁSZF módosítható, a frissített verziók a letöltéssel vagy frissítéssel automatikusan érvénybe lépnek.",
                "Általános Szerződési Feltételek");
        }
    }
}
```

## Felépítés

A felhasználói felület `Grid` vezérlőt használ a rácsos elrendezéshez. Ezen belül két `StackPanel` segíti az elemek vízszintes és függőleges elhelyezését.

## Vezérlők leírása

### 1.`UserNameTextBox`

- Felhasználónév megadására.
- `Width="200"`

### 2.`PasswordBox`

- Jelszó bevitelére szolgál.
- A karaktereket elrejti.

### 3.`BirthDayDatePicker`

- A felhasználó születési dátumának kiválasztásához.

### 4.`AszfCheckBox`

- Jelölőnégyzet az ÁSZF elfogadásához.
- `FontSize="20"`, `IsChecked="False"`

### 5.`AszfButton`

- Általános Szerződési Feltételek megtekintése.
- `Click="AszfButton_Click"`

### 6.`Register`

- Regisztrációs művelet indítása.
- `Click="Register_Click"`
- `Width="100"`, `Height="40"`, `Margin="138,137,162,57"`

---
1. Register_Click – Regisztráció kezelése
Szerepe:
Ez a függvény akkor fut le, amikor a felhasználó a Regisztráció gombra kattint. Ellenőrzi az összes szükséges feltételt (ÁSZF elfogadása, beírt név, jelszó és életkor), majd visszajelzést ad a felhasználónak.
```csharp
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
```
Mit csinál?
sender is Button – Ellenőrzi, hogy valóban a regisztrációs gomb hívta-e meg.

Feltételek ellenőrzése:

ÁSZF el van fogadva (AszfCheckBox.IsChecked == true)

A név és jelszó mezők nem üresek

A felhasználó elmúlt 18 éves (YearOver18())

Ha minden igaz → sikeres regisztrációs üzenet Egyébként → "Érvénytelen adatok" üzenet

### 2. YearOver18 – Életkor ellenőrzése
Szerepe:
Ez a segédfüggvény ellenőrzi, hogy a felhasználó születési dátuma alapján legalább 18 éves-e.

private bool YearOver18()
{
    bool result = false;
    bool b = BirthDayDatePicker.SelectedDate.Value.Date > DateTime.Now.AddYears(-18);
    System.Diagnostics.Debug.WriteLine($"18-nál idősebb: {b}");
    if (BirthDayDatePicker.SelectedDate.Value.Date < DateTime.Now.AddYears(-18))
    {
        result = true;
    }
    return result;
}
Mit csinál?
SelectedDate.Value – Lekéri a kiválasztott születési dátumot.

DateTime.Now.AddYears(-18) – Megszámítja, mikor volt pontosan 18 évvel ezelőtt.

Összehasonlítás:

Ha a születési dátum korábbi (azaz az illető elmúlt 18 éves) → true

Ha későbbi vagy ugyanaz → false

A Debug.WriteLine csak fejlesztési célra szolgál, nem befolyásolja a működést.

3. AszfButton_Click – Általános Szerződési Feltételek megjelenítése
Szerepe:
Ez a függvény akkor fut le, amikor a felhasználó az ÁSZF gombra kattint. Megjelenít egy üzenetablakot, amely tartalmazza az Általános Szerződési Feltételeket.
```
private void AszfButton_Click(object sender, RoutedEventArgs e)
{
    MessageBox.Show(
        "A jelen alkalmazás használata a felhasználó által az ÁSZF elfogadását jelenti.\r\n\r\n" +
        "Az alkalmazás célja, hogy funkcionális és megbízható szolgáltatást nyújtson felhasználói számára.\r\n\r\n" +
        "... (többi szöveg)",
        "Általános Szerződési Feltételek");
}
Mit csinál?
Egy üzenetablakot (MessageBox) jelenít meg.

A szöveg tartalmazza a felhasználási feltételeket, pl. jogi nyilatkozat, adatkezelés, felelősség kizárása stb.

A felhasználó így el tudja olvasni, mit fogad el, amikor bepipálja az ÁSZF-et.
