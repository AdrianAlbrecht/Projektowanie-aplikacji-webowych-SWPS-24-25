# Aplikacje WWW, semestr 2024Z

## Lab 3
---

### **1. Narzędzia administracyjne frameworka Django.**

Django posiada narzędzie administracyjne w postaci skryptu `manage.py` oraz polecenia `django-admin` dzięki którym możemy wykonać wiele czynności administracyjnych, takich jak dodanie użytkownika administracyjnego, wykonanie migracji bazy danych, uruchomienie serwera i inne. Pełną listę poleceń można znaleźć w oficjalnej dokumentacji pod adresem: https://docs.djangoproject.com/pl/4.2/ref/django-admin/

Dodatkowe i przydatne narzędzia można dodać poprzez instalację zewnętrznych modułów, np. [django-admin-tools](https://github.com/django-admin-tools/django-admin-tools) (**archiwalnie istniał problem z instalacją z Django 4.1**),[django-debug-toolbar](https://django-debug-toolbar.readthedocs.io/en/latest/index.html) lub [django-extensions](https://pypi.org/project/django-extensions/).

Jeżeli w trakcie poprzednich zajęć wykonana została tylko pierwsza część oficjalnego tutoriala, to aby móc wykonać ćwiczenia i zadania dla bieżącego laboratorium należy wykonać poniższe czynności.

**Stworzenie schematu bazy danych**

>**Uwaga** Jak zostało już wspomniane na pierwszych zajęciach praca w trakcie zajęć powinn iść dwutorowo, tj. potrzebne są dwa projekty Django, jeden do wykonania zadań w trakcie zadań oraz jako środowisko eksperymentalne, a drugi projekt to projekt na zaliczenie. Od studenta zależy czy będzie to jedno dobrze zorganizowane repozytorium czy dwa oddzielne, jednak zalecane są oddzielne. Będzie to również miało wpływ na przygotowanie odpowiedniej konfiguracji połączenia z bazą danych przed wykonaniem operacji `migrate` (patrz dokumentacja: https://docs.djangoproject.com/en/4.2/ref/databases/).
Istnieje również dość ryzykowna opcja polegająca na stworzeniu oddzielnych aplikacji wewnątrz projektu, z której jedna będzie aplikacją testową, a druga docelowym projektem, którymi dodatkowo można zarządzać z poziomu oddzielnych gałęzi repozytorium. Jest to jednak rozwiązanie dość problematyczne, a w szczególności wymagające dużej uwagi w trakcie przełączania między nimi.

Aby podłączyć bazę danych MySQL do projektu Django, należy wykonać kilka kroków, w tym instalację odpowiedniego sterownika, konfigurację projektu Django oraz utworzenie bazy danych.

### Krok 1: Zainstaluj MySQL

1. **Pobierz i zainstaluj MySQL** z oficjalnej strony [MySQL](https://dev.mysql.com/downloads/).
2. **Utwórz nowego użytkownika i bazę danych** w MySQL, aby używać ich w projekcie Django.

### Krok 2: Zainstaluj MySQL Client dla Pythona

Aby Django mógł komunikować się z bazą danych MySQL, potrzebujesz zainstalować odpowiedni pakiet. Użyj `mysqlclient`, który jest popularnym wyborem:

```bash
pip install mysqlclient
```

**Alternatywa:** Możesz także użyć `PyMySQL`. Aby zainstalować, użyj:

```bash
pip install PyMySQL
```

Jeśli zdecydujesz się na `PyMySQL`, dodaj poniższy kod do swojego pliku `__init__.py` w katalogu głównym projektu, aby zarejestrować ten moduł jako sterownik MySQL:

```python
import pymysql
pymysql.install_as_MySQLdb()
```

### Krok 3: Skonfiguruj bazę danych w projekcie Django

1. **Otwórz plik `settings.py`** w katalogu swojego projektu Django.
2. **Znajdź sekcję `DATABASES`** i zmodyfikuj ją, aby wyglądała tak:

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'nazwa_bazy_danych',  # zamień na nazwę swojej bazy danych
        'USER': 'twoja_nazwa_uzytkownika',  # zamień na nazwę użytkownika
        'PASSWORD': 'twoje_haslo',  # zamień na hasło
        'HOST': 'localhost',  # zazwyczaj 'localhost'
        'PORT': '3306',  # domyślny port MySQL
    }
}
```

### Krok 4: Utwórz bazę danych w MySQL

Zalecana jest praca na MySQLWorkbench, ale jeśli ktokolwiek ma chęć pracować w terminalu to przydadzą się takie komendy:

1. **Zaloguj się do MySQL** przez terminal lub klienta MySQL:

```bash
mysql -u twoja_nazwa_uzytkownika -p
```

2. **Utwórz nową bazę danych**:

```sql
CREATE DATABASE nazwa_bazy_danych;
```

3. **Zakończ sesję**:

```sql
EXIT;
```

### Krok 5: Wykonaj migracje

Po skonfigurowaniu połączenia z bazą danych, musisz wykonać migracje, aby utworzyć tabele w nowej bazie danych.

1. W terminalu, w katalogu swojego projektu Django, uruchom:

*Poniższa operacja stworzy schematy dla wszystkich zainstalowanych aplikacji (zmienna `INSTALLED_APPS` w pliku `settings.py`). Jeżeli nie są niezbędne to można wybrane linie zakomentować lub dodać na końcu polecenia nazwy aplikacji, dla których chcemy stworzyć schemat.*


```bash
python manage.py makemigrations
```

2. Następnie uruchom:

```bash
python manage.py migrate
```

Warto przeczytać dokumentację dotyczącą tego polecenia, gdyż posiada ciekawe opcje, dość często potrzebne w trakcie początkowej pracy z modelem bazy danych i częstymi migracjami: https://docs.djangoproject.com/pl/4.2/ref/django-admin/#migrate

### Krok 6: Sprawdź połączenie

1. **Uruchom serwer deweloperski**, aby upewnić się, że wszystko działa poprawnie:

```bash
python manage.py runserver
```

**Stworzenie superużytkownika**

```console
python manage.py createsuperuser
```

Ten użytkownik jest niezbędny, aby możliwe było zalogowanie się do panelu administracyjnego i wykonywanie w nim czynności administracyjnych.

2. **Zaloguj się do panelu administracyjnego**, jeśli utworzyłeś superużytkownika, aby sprawdzić, czy dane są poprawnie przechowywane:

```bash
http://127.0.0.1:8000/admin
```

Sprawdź dostępne w nim opcje na tym etapie tworzenia aplikacji.

### Krok 7: Testuj

1. **Dodaj dane do bazy danych** za pomocą panelu administracyjnego lub interfejsu Django.
2. **Sprawdź, czy dane są przechowywane w bazie danych MySQL** używając MySQL Workbench, phpMyAdmin lub innego narzędzia do zarządzania bazą danych.

### Dodatkowe uwagi:

- Upewnij się, że masz zainstalowane wszystkie niezbędne biblioteki dla Pythona, aby uniknąć błędów.
- W przypadku korzystania z `PyMySQL`, upewnij się, że Twój projekt Django ma dostęp do tego modułu przez dodanie odpowiedniego kodu w `__init__.py`.
- Jeśli wystąpią jakiekolwiek problemy, sprawdź dokumentację Django oraz MySQL, aby uzyskać dodatkowe informacje na temat błędów.

Teraz Twój projekt Django powinien być poprawnie połączony z bazą danych MySQL!

### 2. Schemat bazy danych.

Każdy projekt potrzebuje jakiejś formy trwałego przechowywania choćby niewielkiej ilości danych, ale większość z nich przewiduje również powiększanie tych zbiorów przez aktywność użytkowników i rozwój aplikacji. Jeżeli nie dokonałeś jeszcze wyboru to zastanów się nad docelowym silnikiem bazy danych.

Tworzenie schematu bazy na potrzeby projektu Django można przeprowadzić na dwa sposoby.

**Sposób 1.**
Tworzymy odpowiednie implementacje klasy `django.db.models.Model` z API Django (patrz **listing 1**), a następnie poprzez migrację tworzymy schemat w docelowej bazie.

**Sposób 2**
Iżynieria wsteczna (ang. reverse engineering). Schemat bazy danych możemy przygotować bezpośrednio poprzez polecenia SQL lub narzędzia graficzne, a następnie poleceniem `manage.py inspectdb` wygenerować kod w języku Python zgodny z deklaracją, którą należy przygotować postępując sposobem pierwszym. Ten kod zostanie wyświetlony w konsoli i można strumień przekierować do pliku. Przykład poniżej.

```console
# dopisanie do pliku
manage.py inspectdb >> models.py 
```

### 3. Modele w Django.


> Dokumentacja: https://docs.djangoproject.com/pl/4.1/topics/db/models/

Definicje modeli dodajemy w pliku `projekt/aplikacja/models.py`.

__*Listing 1:*__
```python

# deklaracja statycznej listy wyboru do wykorzystania w klasie modelu
MONTHS = models.IntegerChoices('Miesiace', 'Styczeń Luty Marzec Kwiecień Maj Czerwiec Lipiec Sierpień Wrzesień Październik Listopad Grudzień')

SHIRT_SIZES = (
        ('S', 'Small'),
        ('M', 'Medium'),
        ('L', 'Large'),
    )


class Team(models.Model):
    name = models.CharField(max_length=60)
    country = models.CharField(max_length=2)

    def __str__(self):
        return f"{self.name}"


class Person(models.Model):

    name = models.CharField(max_length=60)
    shirt_size = models.CharField(max_length=1, choices=SHIRT_SIZES, default=SHIRT_SIZES[0][0])
    month_added = models.IntegerField(choices=MONTHS.choices, default=MONTHS.choices[0][0])
    team = models.ForeignKey(Team, null=True, blank=True, on_delete=models.SET_NULL)

    def __str__(self):
        return self.name
```

Po zdefiniowaniu modelu należy przygotować migracje poleceniem `manage.py makemigrations`, a nstępnie wykonać migrację poprzez `manage.py migrate`.

Wykonanie polecenia migracji powinno propagować powyższe modele na schemat domyślnej bazy danych zdefiniowanej w pliku `settings.py`.

**ZADANIA**

1. **PROJEKT** Zastanów się nad sposobem organizacji swojego kodu dla aplikacji testowej (zajęcia) oraz projektu docelowego i przygotuj środowisko w odpowiedni sposób.
2. **PROJEKT** Korzystając z odpowiedniego oprogramowania (MySql Workbench, pgAdmin, inne) przygotuj pierwszą wersję schematu bazy danych dla swojego projektu i utwórz ten schemat bezpośrednio w bazie danych.
3. **PROJEKT** Za pomocą polecenia `inspectdb` narzędzia administracyjnego Django stwórz plik z modelami z bazy i umieść kod w pliku `models.py` projektu. Sprawdź czy powinieneś wykonać dodatkowe czynności (edycja, pliki migracji?) po tej operacji.
4. Przeanalizuj plik z kodem modeli i dodaj/zmodyfikuj jedno z pól modelu. Przygotuj plik migracji (`makemigrations`), a następnie wykonaj migrację i sprawdź czy zmiany zostały propagowane na bazę. Przejrzyj zawartość pliku migracji.
5. Za pomocą polecenia `showmigrations` wylistuj wszystkie migracje dla danej aplikacji, a następnie sprawdzając w dokumentacji działanie polecenia `migrate` wycofaj ostatnią migrację.
6. Zainstaluj pakiet `django-debug-toolbar` i skonfiguruj go.
7. Posługując się przykładem z [oficjalnego tutoriala numer 2](https://docs.djangoproject.com/pl/4.1/intro/tutorial02/) dodaj jeden model do panelu administracyjnego i przetestuj dodanie, modyfikację i usunięcie instancji tego modelu. Sprawdzaj zawartość w panelu django debug toolbar za każdym razem.
8. Ponownie w tutorialu https://docs.djangoproject.com/pl/4.1/intro/tutorial02/ przeanalizuj przykłady z wykorzystaniem django shell i wykonaj analogiczny przykład (import modułów, modelu, utworzenie instancji, itd.) wykorzystując jeden z utworzonych modeli z listingu 1.
9. Zapisz zmiany w repozytorium i jeżeli jest to wymagane wyślij informacje o dodaktowym repozytorium dla prowadzącego zajęcia.


### Dodatkowe narzędzia i materiały.

* Do obsługi wielu źródeł baz danych może przydać się darmowe narzędzie o nazwie DBeaver, które posiada również możliwość tworzenia schematów bazy danych a następnie generowania poleceń SQL z takiego schematu. Więcej: https://dbeaver.com/2022/07/07/forward-engineering-with-erd/

