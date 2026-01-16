# C-Wypo-yczalnia
README – Projekt: MINI WYPOŻYCZALNIA
(Windows Forms + C# + ListBox)

CEL ZADANIA:
Stwórz aplikację okienkową w Windows Forms, która symuluje małą wypożyczalnię.
Pozycje (np. książki lub filmy) można:
- dodawać
- wypożyczać
- zwracać

UWAGA:
Kod bazowy jest w 100% działający.
Twoim zadaniem jest uruchomić go i DODAĆ 2 rozszerzenia z listy na końcu.

------------------------------------------------------------
FORMULARZ (UI)

Na Form1 muszą być kontrolki o nazwach:

ListBox:   listBoxPozycje
TextBox:   textBoxTytul
TextBox:   textBoxRok
Button:    buttonDodaj
Button:    buttonWypozycz
Button:    buttonZwroc

Po dodaniu kontrolek podłącz eventy Click:
- buttonDodaj → buttonDodaj_Click
- buttonWypozycz → buttonWypozycz_Click
- buttonZwroc → buttonZwroc_Click

------------------------------------------------------------
KOD DZIAŁAJĄCY PROGRAMU (Form1.cs)

using System;
using System.Collections.Generic;
using System.Windows.Forms;

namespace MiniWypozyczalnia
{
    public class Pozycja
    {
        public int Id { get; set; }
        public string Tytul { get; set; }
        public int Rok { get; set; }
        public bool CzyDostepna { get; set; }

        public override string ToString()
        {
            return $"{Id}. {Tytul} ({Rok}) - {(CzyDostepna ? "Dostępna" : "Wypożyczona")}";
        }
    }

    public partial class Form1 : Form
    {
        private List<Pozycja> pozycje = new List<Pozycja>();
        private int kolejneId = 1;

        public Form1()
        {
            InitializeComponent();
            DodajDaneStartowe();
            OdswiezListe();
        }

        private void DodajDaneStartowe()
        {
            pozycje.Add(new Pozycja
            {
                Id = kolejneId++,
                Tytul = "Harry Potter",
                Rok = 2001,
                CzyDostepna = true
            });

            pozycje.Add(new Pozycja
            {
                Id = kolejneId++,
                Tytul = "Matrix",
                Rok = 1999,
                CzyDostepna = false
            });
        }

        private void OdswiezListe()
        {
            listBoxPozycje.DataSource = null;
            listBoxPozycje.DataSource = pozycje;
        }

        private void buttonDodaj_Click(object sender, EventArgs e)
        {
            string tytul = textBoxTytul.Text.Trim();
            string rokTxt = textBoxRok.Text.Trim();

            if (string.IsNullOrWhiteSpace(tytul))
            {
                MessageBox.Show("Podaj tytuł.");
                return;
            }

            if (!int.TryParse(rokTxt, out int rok))
            {
                MessageBox.Show("Rok musi być liczbą.");
                return;
            }

            pozycje.Add(new Pozycja
            {
                Id = kolejneId++,
                Tytul = tytul,
                Rok = rok,
                CzyDostepna = true
            });

            OdswiezListe();
            textBoxTytul.Clear();
            textBoxRok.Clear();
        }

        private void buttonWypozycz_Click(object sender, EventArgs e)
        {
            var zaznaczona = listBoxPozycje.SelectedItem as Pozycja;
            if (zaznaczona == null)
            {
                MessageBox.Show("Zaznacz pozycję.");
                return;
            }

            if (!zaznaczona.CzyDostepna)
            {
                MessageBox.Show("Pozycja jest już wypożyczona.");
                return;
            }

            zaznaczona.CzyDostepna = false;
            OdswiezListe();
        }

        private void buttonZwroc_Click(object sender, EventArgs e)
        {
            var zaznaczona = listBoxPozycje.SelectedItem as Pozycja;
            if (zaznaczona == null)
            {
                MessageBox.Show("Zaznacz pozycję.");
                return;
            }

            if (zaznaczona.CzyDostepna)
            {
                MessageBox.Show("Ta pozycja nie jest wypożyczona.");
                return;
            }

            zaznaczona.CzyDostepna = true;
            OdswiezListe();
        }
    }
}

------------------------------------------------------------
TWOJE DODATKOWE ZADANIA 

Dodaj więcej produktów do wypożyczalni 
Wybierz dowolne 2 rozszerzenia:
(ZROBIĆ MINIMUM 2 PODPUNKTY)

(1) Sortowanie po tytule
(2) Sortowanie po roku wydania
(3) Pokazuj tylko dostępne pozycje (filtr)
(4) Wyszukiwanie po tytule
(5) Licznik pozycji (ile dostępnych / ile wypożyczonych)
(6) Edycja zaznaczonej pozycji
(7) Dodaj własną funkcję (po uzgodnieniu z nauczycielem)

------------------------------------------------------------
PUNKTACJA 

+2 pkt – program działa (dodawanie, wypożyczenie, zwrot)
+2 pkt – 1 rozszerzenie
+2 pkt – 2 rozszerzenie
+2 pkt – estetyka + obsługa błędów


