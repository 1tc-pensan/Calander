# Calendar és DatePicker WPF-ben

Ez a dokumentáció a megadott C# WPF alkalmazás, a "Calendar" működését és kódját magyarázza el röviden és érthetően.

A "Calendar" egy egyszerű WPF alkalmazás, amely egy regisztrációs űrlapot biztosít a felhasználók számára. Az űrlap segítségével a felhasználók megadhatják nevüket, jelszavukat, születési dátumukat, és elfogadhatják az Általános Szerződési Feltételeket (ÁSZF). A regisztráció csak akkor sikeres, ha minden adat érvényes és a felhasználó elmúlt 18 éves.
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
1. Felépítés
A kód egy Grid vezérlőelemet használ, amely egy rács alapú elrendezést biztosít az UI elemek pozicionálására. Ezen belül egy másik Grid található, amely további elrendezést biztosít, és a tartalom egy StackPanel segítségével van megszervezve.

2. StackPanel-ek
Külső StackPanel:
Orientation="Horizontal": Ez azt jelenti, hogy az elemek vízszintesen (balról jobbra) vannak elrendezve.
Margin="10,10,0,0": A panel 10 egységnyi margóval rendelkezik felül és bal oldalon.
Belső StackPanel-ek:
Két StackPanel van, mindkettő Orientation="Vertical", tehát az elemek függőlegesen vannak elrendezve egymás alatt.
Az első StackPanel szöveges címkéket tartalmaz, a második pedig az űrlap beviteli mezőit.
3. Első StackPanel (címkék)
Ez a panel tartalmazza a következő TextBlock elemeket, amelyek szöveges címkék az űrlap mezőihez:

"Felhasználó név:" – A felhasználónév mező címkéje.
"Jelszó:" – A jelszó mező címkéje.
"Születési dátum:" – A születési dátum mező címkéje.
Mindegyik FontSize="20" tulajdonsággal rendelkezik, tehát a betűméret 20 egység.
Ezenkívül van egy Button (gomb):

x:Name="AszfButton": A gomb neve, amely lehetővé teszi, hogy a kódban hivatkozzunk rá.
Content="Általános Szerződési feltételek": A gomb felirata.
Margin="0,10,10,0": Margó a gomb körül.
Click="AszfButton_Click": Egy eseménykezelő, amely akkor fut le, ha a gombra kattintanak. Ez feltételez egy AszfButton_Click nevű metódust a háttérkódban (pl. C#), amely kezeli a gomb kattintását (pl. megnyithat egy ÁSZF dokumentumot).
4. Második StackPanel (beviteli mezők)
Ez a panel tartalmazza az űrlap beviteli elemeit:

1. **UserNameTextBox** (`TextBox`):  
   Egy szövegbeviteli mező, ahol a felhasználó megadhatja a felhasználónevét.  
   - `Width="200"`: A mező szélessége 200 egység.

2. **PasswordBox** (`PasswordBox`):  
   Egy jelszóbeviteli mező, amely elrejti a begépelt karaktereket (pl. csillagokkal vagy pontokkal).

3. **BirthDayDatePicker** (`DatePicker`):  
   Egy dátumválasztó vezérlő, amely lehetővé teszi a felhasználó számára, hogy kiválasszon egy születési dátumot.

4. **AszfCheckBox** (`CheckBox`):  
   Egy jelölőnégyzet, amelynek felirata "Ászf".  
   - `FontSize="20"`: A betűméret 20 egység.  
   - `IsChecked="False"`: Alapértelmezés szerint a jelölőnégyzet nincs bejelölve.

5. **Regisztrációs gomb** (`Button`):  
   A külső Grid tartalmaz egy külön `Button` elemet, amely a regisztráció indítására szolgál.


x:Name="Register": A gomb neve.
Content="Regisztárico": A gomb felirata (valószínűleg elírás, helyesen "Regisztráció" lenne).
Width="100", Height="40": A gomb mérete.
Margin="138,137,162,57": A gomb pozíciója a rácson belül (bal, felső, jobb, alsó margó).
Click="Register_Click": Egy eseménykezelő, amely a gombra kattintáskor fut le, és valószínűleg a regisztrációs folyamatot kezeli (pl. az űrlap adatainak ellenőrzését és mentését).
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
Metódus aláírása:
private void Register_Click(object sender, RoutedEventArgs e): Ez a Register gomb Click eseményéhez tartozó metódus, amelyet a XAML-ben definiáltak (Click="Register_Click").
Ellenőrzés, hogy a küldő egy gomb:
if (sender is Button button): Megbizonyosodik róla, hogy az eseményt kiváltó objektum egy gomb. Ez biztonsági ellenőrzés, bár itt valószínűleg felesleges, mivel a gomb hívja meg.
Feltételek ellenőrzése:
AszfCheckBox.IsChecked == true: Ellenőrzi, hogy az ÁSZF jelölőnégyzet be van-e jelölve.
UserNameTextBox.Text != null: Ellenőrzi, hogy a felhasználónév mező nem üres.
PasswordBox.Password != null: Ellenőrzi, hogy a jelszó mező nem üres.
YearOver18(): Egy feltételezett metódus, amely valószínűleg azt ellenőrzi, hogy a felhasználó 18 évnél idősebb-e a BirthDayDatePicker alapján.
Eredmény:
Ha minden feltétel teljesül, egy üzenetablak (MessageBox) jelenik meg: "Sikeresen regisztráltál".
Ha bármelyik feltétel nem teljesül, egy üzenetablak jelzi: "Érvénytelen adatok".
<TextBlock Text="Naptár:" Margin="0,0,0,5"/>
<Calendar x:Name="myCalendar" SelectionMode="SingleDate"/>
```cs
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
Visszatérési érték: A metódus bool típusú, tehát true vagy false értéket ad vissza.
Logika:
BirthDayDatePicker.SelectedDate.Value.Date: Lekéri a kiválasztott születési dátumot.
DateTime.Now.AddYears(-18): Az aktuális dátumot 18 évvel csökkenti, így megkapja azt a dátumot, amely előtt a felhasználó 18 évnél idősebb.
if (BirthDayDatePicker.SelectedDate.Value.Date < DateTime.Now.AddYears(-18)): Ha a születési dátum korábbi, mint az aktuális dátum mínusz 18 év, akkor a felhasználó 18 évnél idősebb, így result = true.
Hibás/ felesleges sor:
bool b = BirthDayDatePicker.SelectedDate.Value.Date > DateTime.Now.AddYears(-18): Ez a sor ellenőrzi, hogy a születési dátum későbbi-e, mint 18 évvel ezelőtt, de az eredményt (b) nem használja, csak kiírja a debug konzolra (System.Diagnostics.Debug.WriteLine). Ez redundáns, mert a tényleges logika az if feltétele.
Visszatérés: A result értéket adja vissza, amely true, ha a felhasználó 18 évnél idősebb, különben false.
```cs
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
```
Funkció:
A metódus egy üzenetablakot (MessageBox) jelenít meg, amely az alkalmazás ÁSZF-jét tartalmazza.
Tartalom:
Az üzenetablak szövege az ÁSZF részleteit írja le, például:
Az alkalmazás használata az ÁSZF elfogadását jelenti.
Az alkalmazás célja funkcionális és megbízható szolgáltatás nyújtása.
A tartalom szerzői jogvédelem alatt áll.
A fejlesztő nem felel adatvesztésért vagy hibás működésért.
Jogszerű használatra kötelezi a felhasználót.
Az alkalmazás lehet ingyenes vagy fizetős (verziófüggő).
A fejlesztő előzetes értesítés nélkül frissítheti az alkalmazást.
Személyes adatok kezelése adatvédelmi tájékoztató szerint történik.
Magyar jog az irányadó jogviták esetén.
Az ÁSZF módosítható, új verziók automatikusan érvénybe lépnek.
Üzenetablak:
MessageBox.Show(...): Megjeleníti az ÁSZF szövegét egy felugró ablakban, amelynek címe "Általános Szerződési Feltételek".
```cs
        private void AszfButton_Click(object sender, RoutedEventArgs e)
        {
            MessageBox.Show("A jelen alkalmazás használata a felhasználó által az ÁSZF elfogadását jelenti.\r\n\r\nAz alkalmazás célja, hogy funkcionális és megbízható szolgáltatást nyújtson felhasználói számára.\r\n\r\nAz alkalmazás minden tartalma és funkciója szerzői jogvédelem alatt áll.\r\n\r\nA fejlesztő nem vállal felelősséget az esetleges adatvesztésért vagy hibás működésből eredő károkért.\r\n\r\nA felhasználó köteles az alkalmazást jogszerűen és rendeltetésszerűen használni.\r\n\r\nAz alkalmazás használata ingyenes/fizetős – kérjük, ellenőrizze az adott verzió feltételeit.\r\n\r\nA fejlesztő fenntartja a jogot az alkalmazás frissítésére vagy módosítására előzetes értesítés nélkül.\r\n\r\nA személyes adatok kezelése az adatvédelmi tájékoztató szerint történik.\r\n\r\nBármely jogvita esetén a magyar jog az irányadó.\r\n\r\nA jelen ÁSZF módosítható, a frissített verziók a letöltéssel vagy frissítéssel automatikusan érvénybe lépnek.", "Általános Szerződési Feltételek");
        }
```
