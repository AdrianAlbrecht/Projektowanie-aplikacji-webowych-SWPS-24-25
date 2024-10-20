# Aplikacje WWW, semestr 2024Z

## Lab 4
---
### **1. Więcej możliwości modeli w ekosystemie Django.**


Oprócz typowego mapowania obiektowo-relacyjnego klasy modeli frameworka Django oferują dużo więcej możliwości, m.in. implementację funkcji pomocniczych, określenie domyślnego porządku sortowania zwracanych krotek, określenie parametrów walidacji i inne. Poniżej zostaną zaprezentowane niektóre z możliwości wraz z przykładami użycia.

Przydatne linki do dokumentacji, które mogą się przydać w trakcie rozwiązywania zadań:
1. Lista dostępnych pól klas modeli: https://docs.djangoproject.com/pl/4.2/ref/models/fields/#model-field-types

W Django modele definiują strukturę tabel w bazie danych, a typy pól (field types) określają, jakie dane mogą być przechowywane w tych tabelach. Poniżej znajduje się lista najpopularniejszych typów pól w Django, wraz z przykładami zastosowań i sposobem ich użycia:

### 1. **CharField**
   Przechowuje tekst o ograniczonej długości.
   - **Przykład**: Imię, nazwisko, tytuł książki.
   - **Użycie**:
     ```python
     from django.db import models

     class Person(models.Model):
         first_name = models.CharField(max_length=50)
         last_name = models.CharField(max_length=50)
     ```

### 2. **TextField**
   Przechowuje długi tekst, bez limitu długości (przydatne dla treści jak opisy, artykuły).
   - **Przykład**: Opis produktu, artykuł na blogu.
   - **Użycie**:
     ```python
     class Article(models.Model):
         title = models.CharField(max_length=100)
         content = models.TextField()
     ```

### 3. **IntegerField**
   Przechowuje liczby całkowite.
   - **Przykład**: Wiek, liczba przedmiotów.
   - **Użycie**:
     ```python
     class Product(models.Model):
         name = models.CharField(max_length=100)
         stock_quantity = models.IntegerField()
     ```

### 4. **FloatField**
   Przechowuje liczby zmiennoprzecinkowe.
   - **Przykład**: Cena produktu, współrzędne geograficzne.
   - **Użycie**:
     ```python
     class Product(models.Model):
         name = models.CharField(max_length=100)
         price = models.FloatField()
     ```

### 5. **BooleanField**
   Przechowuje wartość logiczną (`True` lub `False`).
   - **Przykład**: Czy produkt jest dostępny.
   - **Użycie**:
     ```python
     class Product(models.Model):
         name = models.CharField(max_length=100)
         is_available = models.BooleanField(default=True)
     ```

### 6. **DateField**
   Przechowuje daty.
   - **Przykład**: Data urodzenia, data publikacji.
   - **Użycie**:
     ```python
     class Event(models.Model):
         event_name = models.CharField(max_length=100)
         event_date = models.DateField()
     ```

### 7. **DateTimeField**
   Przechowuje daty i godziny.
   - **Przykład**: Data i czas rejestracji użytkownika, czas utworzenia posta.
   - **Użycie**:
     ```python
     class Post(models.Model):
         title = models.CharField(max_length=100)
         created_at = models.DateTimeField(auto_now_add=True)
     ```

### 8. **EmailField**
   Specjalizacja `CharField` przechowująca adresy email.
   - **Przykład**: Email użytkownika.
   - **Użycie**:
     ```python
     class User(models.Model):
         name = models.CharField(max_length=100)
         email = models.EmailField(unique=True)
     ```

### 9. **URLField**
   Przechowuje adresy URL.
   - **Przykład**: Strona internetowa firmy.
   - **Użycie**:
     ```python
     class Company(models.Model):
         name = models.CharField(max_length=100)
         website = models.URLField()
     ```

### 10. **ForeignKey**
   Tworzy relację jeden-do-wielu (One-to-Many) z inną tabelą.
   - **Przykład**: Posty powiązane z użytkownikiem.
   - **Użycie**:
     ```python
     class User(models.Model):
         name = models.CharField(max_length=100)

     class Post(models.Model):
         title = models.CharField(max_length=100)
         content = models.TextField()
         author = models.ForeignKey(User, on_delete=models.CASCADE)
     ```

### 11. **ManyToManyField**
   Tworzy relację wiele-do-wielu (Many-to-Many) z inną tabelą.
   - **Przykład**: Grupy użytkowników w systemie.
   - **Użycie**:
     ```python
     class Group(models.Model):
         name = models.CharField(max_length=100)

     class User(models.Model):
         name = models.CharField(max_length=100)
         groups = models.ManyToManyField(Group)
     ```

### 12. **OneToOneField**
   Tworzy relację jeden-do-jednego (One-to-One) z inną tabelą.
   - **Przykład**: Profil użytkownika.
   - **Użycie**:
     ```python
     class User(models.Model):
         name = models.CharField(max_length=100)

     class Profile(models.Model):
         user = models.OneToOneField(User, on_delete=models.CASCADE)
         bio = models.TextField()
     ```

### 13. **DecimalField**
   Przechowuje liczby dziesiętne z określoną precyzją (przydatne dla walut, wartości finansowych).
   - **Przykład**: Cena produktu.
   - **Użycie**:
     ```python
     class Product(models.Model):
         name = models.CharField(max_length=100)
         price = models.DecimalField(max_digits=10, decimal_places=2)
     ```

### 14. **SlugField**
   Przechowuje krótkie etykiety (slug), często używane w adresach URL.
   - **Przykład**: Etykieta dla artykułu.
   - **Użycie**:
     ```python
     class Article(models.Model):
         title = models.CharField(max_length=100)
         slug = models.SlugField(unique=True)
     ```

### 15. **FileField**
   Przechowuje ścieżkę do pliku (np. dokumentu lub obrazu).
   - **Przykład**: Załączniki do wiadomości, zdjęcia użytkowników.
   - **Użycie**:
     ```python
     class Document(models.Model):
         title = models.CharField(max_length=100)
         file = models.FileField(upload_to='documents/')
     ```

### 16. **ImageField**
   Specjalizacja `FileField` dla plików graficznych.
   - **Przykład**: Awatar użytkownika.
   - **Użycie**:
     ```python
     class UserProfile(models.Model):
         user = models.OneToOneField(User, on_delete=models.CASCADE)
         avatar = models.ImageField(upload_to='avatars/')
     ```

### 17. **TimeField**
   Przechowuje czas (bez daty).
   - **Przykład**: Czas rozpoczęcia spotkania.
   - **Użycie**:
     ```python
     class Meeting(models.Model):
         title = models.CharField(max_length=100)
         start_time = models.TimeField()
     ```

### 18. **DurationField**
   Przechowuje czas trwania.
   - **Przykład**: Czas trwania filmu, sesji.
   - **Użycie**:
     ```python
     class Movie(models.Model):
         title = models.CharField(max_length=100)
         duration = models.DurationField()
     ```

Każde pole w modelu jest przetwarzane i walidowane automatycznie przez Django ORM, co znacznie upraszcza pracę z danymi. Można dodatkowo używać atrybutów takich jak `null=True`, `blank=True`, `default=...`, aby dostosować sposób przechowywania danych w bazie.

2. Atrybuty modeli: https://docs.djangoproject.com/pl/4.2/topics/db/models/#field-options

Atrybuty modeli w Django definiują różne aspekty zachowania pól i modeli w interakcji z bazą danych oraz walidacją. Oto najczęściej używane atrybuty modeli Django, ich przykłady i zastosowanie:

### 1. **`null`**
   - Określa, czy pole może przechowywać wartość `NULL` w bazie danych.
   - **Typ pól**: Wszystkie pola (oprócz `ManyToManyField`, `OneToOneField` i `ForeignKey`).
   - **Domyślna wartość**: `False` (pole musi mieć wartość).
   - **Przykład**:
     ```python
     class Person(models.Model):
         name = models.CharField(max_length=100, null=True)
     ```

### 2. **`blank`**
   - Określa, czy pole może być puste podczas walidacji formularzy (dotyczy walidacji na poziomie formularzy, a nie bazy danych).
   - **Typ pól**: Wszystkie pola.
   - **Domyślna wartość**: `False` (pole nie może być puste w formularzach).
   - **Przykład**:
     ```python
     class Person(models.Model):
         email = models.EmailField(blank=True)
     ```

### 3. **`default`**
   - Ustala domyślną wartość dla pola, jeśli użytkownik jej nie poda.
   - **Typ pól**: Wszystkie pola.
   - **Przykład**:
     ```python
     class Product(models.Model):
         price = models.DecimalField(max_digits=10, decimal_places=2, default=0.00)
     ```

### 4. **`unique`**
   - Sprawia, że wartość w danym polu musi być unikalna w całej tabeli.
   - **Typ pól**: Wszystkie pola (oprócz pól relacyjnych jak `ManyToManyField`).
   - **Przykład**:
     ```python
     class User(models.Model):
         username = models.CharField(max_length=50, unique=True)
     ```

### 5. **`choices`**
   - Definiuje listę dostępnych wartości dla pola (jako pary: wyświetlana wartość i wewnętrzna wartość).
   - **Typ pól**: Najczęściej `CharField`, `IntegerField`, itp.
   - **Przykład**:
     ```python
     class Person(models.Model):
         STATUS_CHOICES = [
             ('A', 'Active'),
             ('I', 'Inactive'),
         ]
         status = models.CharField(max_length=1, choices=STATUS_CHOICES, default='A')
     ```

### 6. **`primary_key`**
   - Określa, czy pole jest kluczem głównym (zastępuje automatyczne tworzenie pola `id`).
   - **Typ pól**: Zazwyczaj `IntegerField` lub `CharField`.
   - **Przykład**:
     ```python
     class Person(models.Model):
         ssn = models.CharField(max_length=9, primary_key=True)
     ```

### 7. **`db_index`**
   - Tworzy indeks na kolumnie, co przyspiesza operacje wyszukiwania.
   - **Typ pól**: Wszystkie pola.
   - **Przykład**:
     ```python
     class Product(models.Model):
         sku = models.CharField(max_length=30, db_index=True)
     ```

### 8. **`verbose_name`**
   - Określa bardziej opisową nazwę pola, wyświetlaną w panelu administracyjnym Django.
   - **Typ pól**: Wszystkie pola.
   - **Przykład**:
     ```python
     class Person(models.Model):
         first_name = models.CharField(max_length=50, verbose_name="First Name")
     ```

### 9. **`verbose_name_plural`**
   - Definiuje liczbę mnogą nazwy modelu, wyświetlaną w panelu administracyjnym.
   - **Typ modeli**: Wszystkie modele.
   - **Przykład**:
     ```python
     class Person(models.Model):
         name = models.CharField(max_length=100)

         class Meta:
             verbose_name_plural = "People"
     ```

### 10. **`help_text`**
   - Dodaje pomocniczy tekst wyświetlany w formularzach, opisujący pole.
   - **Typ pól**: Wszystkie pola.
   - **Przykład**:
     ```python
     class Product(models.Model):
         price = models.DecimalField(max_digits=10, decimal_places=2, help_text="Enter price in USD")
     ```

### 11. **`auto_now`**
   - Automatycznie ustawia pole na bieżący czas i datę przy każdej aktualizacji obiektu.
   - **Typ pól**: `DateField`, `DateTimeField`.
   - **Przykład**:
     ```python
     class Post(models.Model):
         updated_at = models.DateTimeField(auto_now=True)
     ```

### 12. **`auto_now_add`**
   - Automatycznie ustawia pole na bieżący czas i datę przy tworzeniu obiektu.
   - **Typ pól**: `DateField`, `DateTimeField`.
   - **Przykład**:
     ```python
     class Post(models.Model):
         created_at = models.DateTimeField(auto_now_add=True)
     ```

### 13. **`on_delete`**
   - Określa zachowanie relacyjnych pól (np. `ForeignKey`) przy usunięciu obiektu, do którego odnosimy się.
   - **Typ pól**: `ForeignKey`, `OneToOneField`.
   - **Przykład**:
     ```python
     class Post(models.Model):
         author = models.ForeignKey(User, on_delete=models.CASCADE)
     ```

   - Najczęściej używane wartości dla `on_delete`:
     - `CASCADE`: Usuwa powiązane obiekty.
     - `SET_NULL`: Ustawia `NULL` (pole musi mieć `null=True`).
     - `PROTECT`: Blokuje usunięcie powiązanego obiektu.
     - `SET_DEFAULT`: Ustawia wartość domyślną (pole musi mieć `default`).
     - `DO_NOTHING`: Nie wykonuje żadnej akcji.

### 14. **`related_name`**
   - Określa nazwę odwrotnej relacji dla pól relacyjnych (`ForeignKey`, `ManyToManyField`, `OneToOneField`).
   - **Typ pól**: `ForeignKey`, `OneToOneField`, `ManyToManyField`.
   - **Przykład**:
     ```python
     class Book(models.Model):
         title = models.CharField(max_length=100)

     class Author(models.Model):
         name = models.CharField(max_length=100)
         books = models.ManyToManyField(Book, related_name='authors')
     ```

### 15. **`unique_together`**
   - Definiuje zestaw pól, które muszą być unikalne razem (np. kombinacja pól).
   - **Typ modeli**: Meta klasy modeli.
   - **Przykład**:
     ```python
     class Order(models.Model):
         customer = models.ForeignKey(User, on_delete=models.CASCADE)
         order_date = models.DateField()

         class Meta:
             unique_together = ('customer', 'order_date')
     ```

### 16. **`ordering`**
   - Określa domyślne sortowanie rekordów w zapytaniach do bazy danych.
   - **Typ modeli**: Meta klasy modeli.
   - **Przykład**:
     ```python
     class Product(models.Model):
         name = models.CharField(max_length=100)
         price = models.DecimalField(max_digits=10, decimal_places=2)

         class Meta:
             ordering = ['price']
     ```

### 17. **`editable`**
   - Określa, czy pole może być edytowane w formularzach Django (np. panelu administracyjnym).
   - **Typ pól**: Wszystkie pola.
   - **Przykład**:
     ```python
     class Product(models.Model):
         sku = models.CharField(max_length=30, editable=False)
     ```

### 18. **`limit_choices_to`**
   - Ogranicza wybory dla pól relacyjnych, np. `ForeignKey` lub `ManyToManyField`, na podstawie filtrów.
   - **Typ pól**: `ForeignKey`, `ManyToManyField`.
   - **Przykład**:
     ```python
     class ActiveManager(models.Manager):
         def get_queryset(self):
             return super().get_queryset().filter(is_active=True)

     class Product(models.Model):
         name = models.CharField(max_length=100)
         manager = models.ForeignKey('self', limit_choices_to={'is_active': True}, on_delete=models.CASCADE)
     ```

Każdy atrybut pomaga dostosować sposób, w jaki pola modeli są przechowywane, walidowane i prezentowane w formularzach lub administracji Django. Kombinacja tych atrybutów pozwala na szczegółowe definiowanie zachowań aplikacji.

3. Klasa QuerySet i dostępne metody: https://docs.djangoproject.com/pl/4.2/ref/models/querysets/

Django ORM korzysta z obiektów `QuerySet` do wykonywania zapytań do bazy danych. `QuerySet` reprezentuje kolekcję obiektów modelu i pozwala na ich filtrowanie, sortowanie oraz manipulowanie danymi. Poniżej znajdziesz przegląd najczęściej używanych metod `QuerySet`, ich zastosowanie i przykłady.

### 1. **`all()`**
   - Zwraca wszystkie obiekty modelu.
   - **Przykład**:
     ```python
     from myapp.models import Product
     products = Product.objects.all()
     ```

### 2. **`filter(**kwargs)`**
   - Zwraca obiekty spełniające podane kryteria.
   - **Przykład**:
     ```python
     active_products = Product.objects.filter(is_active=True)
     ```

### 3. **`exclude(**kwargs)`**
   - Zwraca obiekty, które nie spełniają podanych kryteriów.
   - **Przykład**:
     ```python
     inactive_products = Product.objects.exclude(is_active=True)
     ```

### 4. **`get(**kwargs)`**
   - Zwraca jeden obiekt spełniający podane kryteria. Podnosi wyjątek `DoesNotExist`, jeśli obiekt nie istnieje, lub `MultipleObjectsReturned`, jeśli istnieje więcej niż jeden.
   - **Przykład**:
     ```python
     product = Product.objects.get(id=1)
     ```

### 5. **`count()`**
   - Zwraca liczbę obiektów w `QuerySet`.
   - **Przykład**:
     ```python
     number_of_products = Product.objects.filter(is_active=True).count()
     ```

### 6. **`exists()`**
   - Sprawdza, czy `QuerySet` zawiera jakiekolwiek obiekty (zwraca `True` lub `False`).
   - **Przykład**:
     ```python
     has_active_products = Product.objects.filter(is_active=True).exists()
     ```

### 7. **`order_by(*fields)`**
   - Sortuje obiekty według podanych pól. Dodaj `-` przed nazwą pola, aby sortować malejąco.
   - **Przykład**:
     ```python
     sorted_products = Product.objects.order_by('price')
     ```

### 8. **`distinct()`**
   - Zwraca unikalne obiekty w `QuerySet`.
   - **Przykład**:
     ```python
     unique_categories = Product.objects.values('category').distinct()
     ```

### 9. **`values(*fields)`**
   - Zwraca `QuerySet` słowników, z wartościami tylko dla podanych pól.
   - **Przykład**:
     ```python
     product_names = Product.objects.values('name')
     ```

### 10. **`values_list(*fields, flat=False)`**
   - Zwraca `QuerySet` list wartości dla podanych pól. Użyj `flat=True`, aby otrzymać jednowymiarową listę dla jednego pola.
   - **Przykład**:
     ```python
     product_prices = Product.objects.values_list('price', flat=True)
     ```

### 11. **`annotate(*args, **kwargs)`**
   - Dodaje dodatkowe pole do obiektów w `QuerySet`, które wykonuje agregację.
   - **Przykład**:
     ```python
     from django.db.models import Count
     categories = Product.objects.values('category').annotate(num_products=Count('id'))
     ```

### 12. **`aggregate(**kwargs)`**
   - Zwraca słownik z wynikami agregacji na poziomie całego `QuerySet`.
   - **Przykład**:
     ```python
     from django.db.models import Sum
     total_price = Product.objects.aggregate(Sum('price'))
     ```

### 13. **`first()`**
   - Zwraca pierwszy obiekt w `QuerySet` lub `None`, jeśli `QuerySet` jest pusty.
   - **Przykład**:
     ```python
     first_product = Product.objects.all().first()
     ```

### 14. **`last()`**
   - Zwraca ostatni obiekt w `QuerySet` lub `None`, jeśli `QuerySet` jest pusty.
   - **Przykład**:
     ```python
     last_product = Product.objects.all().last()
     ```

### 15. **`slice()`**
   - Umożliwia używanie indeksów do uzyskiwania podzbiorów z `QuerySet`.
   - **Przykład**:
     ```python
     some_products = Product.objects.all()[0:10]  # Pierwsze 10 produktów
     ```

### 16. **`select_related(*fields)`**
   - Umożliwia pobieranie powiązanych obiektów w tym samym zapytaniu, co poprawia wydajność w przypadku relacji `ForeignKey` i `OneToOneField`.
   - **Przykład**:
     ```python
     products = Product.objects.select_related('category').all()
     ```

### 17. **`prefetch_related(*fields)`**
   - Umożliwia pobieranie powiązanych obiektów w oddzielnym zapytaniu, co poprawia wydajność w przypadku relacji `ManyToManyField` i `reverse ForeignKey`.
   - **Przykład**:
     ```python
     categories = Category.objects.prefetch_related('products').all()
     ```

### 18. **`update(**kwargs)`**
   - Umożliwia aktualizację wielu obiektów w `QuerySet` za pomocą jednego zapytania.
   - **Przykład**:
     ```python
     Product.objects.filter(is_active=True).update(price=F('price') * 1.1)  # Zwiększa cenę o 10%
     ```

### 19. **`delete()`**
   - Usuwa obiekty z bazy danych. Zwraca liczbę usuniętych obiektów oraz słownik informacyjny.
   - **Przykład**:
     ```python
     deleted_count, _ = Product.objects.filter(is_active=False).delete()
     ```

### 20. **`iterator()`**
   - Umożliwia iterowanie przez `QuerySet`, co jest przydatne w przypadku dużych zbiorów danych.
   - **Przykład**:
     ```python
     for product in Product.objects.iterator():
         print(product.name)
     ```

### 21. **`using(db_alias)`**
   - Umożliwia wykonanie zapytań na konkretnej bazie danych, jeśli masz skonfigurowane wiele baz danych.
   - **Przykład**:
     ```python
     products = Product.objects.using('secondary_db').all()
     ```

### 22. **`reverse()`**
   - Odwraca kolejność obiektów w `QuerySet`.
   - **Przykład**:
     ```python
     reversed_products = Product.objects.all().reverse()
     ```

### 23. **`defer(*fields)`**
   - Opóźnia pobieranie wartości dla podanych pól do momentu, gdy będą one potrzebne, co może poprawić wydajność.
   - **Przykład**:
     ```python
     products = Product.objects.defer('description').all()  # Opóźnia pobieranie 'description'
     ```

### 24. **`only(*fields)`**
   - Pobiera tylko podane pola z bazy danych, co może poprawić wydajność.
   - **Przykład**:
     ```python
     products = Product.objects.only('name', 'price').all()  # Pobiera tylko 'name' i 'price'
     ```

### 25. **`get_or_create(**kwargs)`**
   - Zwraca krotkę (obiekt, utworzone) lub zwraca istniejący obiekt, jeśli już istnieje.
   - **Przykład**:
     ```python
     product, created = Product.objects.get_or_create(name='New Product', defaults={'price': 10.0})
     ```

### 26. **`bulk_create(objs, batch_size=None)`**
   - Umożliwia szybkie tworzenie wielu obiektów w jednym zapytaniu.
   - **Przykład**:
     ```python
     products = [Product(name='Product 1'), Product(name='Product 2')]
     Product.objects.bulk_create(products)
     ```

### 27. **`bulk_update(objs, fields)`**
   - Umożliwia szybkie aktualizowanie wielu obiektów w jednym zapytaniu.
   - **Przykład**:
     ```python
     products = Product.objects.all()
     for product in products:
         product.price += 1.0
     Product.objects.bulk_update(products, ['price'])
     ```

### 28. **`update_or_create(**kwargs)`**
   - Umożliwia aktualizowanie istniejącego obiektu lub tworzenie nowego, jeśli obiekt nie istnieje.
   - **Przykład**:
     ```python
     product, created = Product.objects.update_or_create(
         name='Existing Product',
         defaults={'price': 20.0}
     )
     ```

Metody `QuerySet` w Django ORM są

 potężne i elastyczne, umożliwiając łatwe manipulowanie danymi w bazie danych. Wybór odpowiednich metod może znacząco poprawić wydajność aplikacji.

#### **1.1 Statyczna lista wartości pola wyboru.** 

Bardzo często w wielu aplikacjach potrzebujemy przechować dane w postaci tabel słownikowych, czyli takich, które są statycznymi skończonymi zbiorami wartości definiowanymi za zwyczaj w momencie wdrażania aplikacji, a potrzebnymi w procesie jej eksploatacji. Takim przykładem może być lista miesięcy do wyboru, nazw województw itp. Często informacje te są przechowywane w bazie danych co ułatwia zarządzanie (nie są rozproszone w wielu miejscach systemu), ale istnieją również możliwości określenia takich list na poziomie modelu Django. Przykład poniżej.

**_Listing 1_**
```python
# kod z oficjalnej dokumentacji Django
class Person(models.Model):
    # lista wartości do wyboru w formie krotek
    SHIRT_SIZES = (
        ('S', 'Small'),
        ('M', 'Medium'),
        ('L', 'Large'),
    )
    name = models.CharField(max_length=60)
    # wskazanie listy poprzez przypisanie do parametru choices
    shirt_size = models.CharField(max_length=1, choices=SHIRT_SIZES)
```

Dodanie klasy do pliku `models.py` a następnie przygotowanie, wykonanie migracji oraz dodanie modelu do panelu administracyjnego pozwoli na przetestowanie działania takiego podejścia.

**Zadanie 1**  
Pracę rozpocznij od utworzenia nowego brancha o nazwie `lab_3` w swoim repozytorium. Następnie stwórz nowy model o nazwie `Osoba` z polami:
* `imie` - pole tekstowe, wymagane, niepuste (sprawdź dokumentację z pkt. 2)
* `nazwisko` - pole tekstowe, wymagane, niepuste
* `plec` - pole wyboru (kobieta, mężczyzna, inne)
* `stanowisko` - klucz obcy do modelu `Stanowisko` (do utworzenia w kolejnym kroku).

Stwórz model `Stanowisko` z polami:
* `nazwa` - pole tekstowe, wymagane, niepuste
* `opis` - pole tekstowe, opcjonalne

Przygotuj i wykonaj migrację. Dodaj modele do panelu administracyjnego i zapisz do bazy po 3 instancje obiektu `Stanowisko` i `Osoba`.

**Zadanie 2**  
W dokumentacji pola typu `DateField` znajdź informacje o sposobie automatycznego wstawiania wartości znacznika czasu (timestamp) w momencie utworzenia obiektu. Ustaw taką własność dla nowego pola `data_dodania` modelu `Osoba`.

**Zadanie 3**  
Znajdź w dokumentacji modułu admin możliwość ustawienia danego pola modelu tylko do odczytu i ustaw taką własność dla pola `data_dodania` modelu `Osoba`.

**Zadanie 4**  
Z dokumentacji typów wyliczeniowych https://docs.djangoproject.com/en/4.2/ref/models/fields/#enumeration-types wykorzystaj klasę `models.IntegerChoices` i zmień deklarację pola `plec`, jeżeli użyłeś/-aś innej metody.

**Zadanie 5**  
Dodaj nowe modele do panelu admina. Użyj panelu admina, żeby dodać nowe obiekty danych modeli. Co trzeba zrobić gdy tworzymy obeikt z modelu, który wykorzytsuje klucz obcy?

**Zadanie 6**  
PROJEKT Zastanów się jak zmodyfikować (albo dodać) modele w swoim projekcie. Zatanów się nad atrybutami.

