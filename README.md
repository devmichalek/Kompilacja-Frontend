## Wstęp
Cześć, na wstępie chciałbym aby całą pracę włożoną w to repozytorium potraktować bardziej jako (obszerny) artykuł. Zebrane tutaj informacje są podsumowaniem całej zdobytej mojej wiedzy na temat tworzenia bibliotek dynamicznych (i nie tylko). Przykłady omawiane są w języku C i C++ oraz oczywiście w języku polskim ;). Możliwe, że artykuł nie pokrył wszystkich tematów związanych z bibliotekami jednak na pewno pokrywa ich zdecydowaną większość. Artykuł będzie co jakiś czas ulepszany, dodam, iż chętnie przyjmę jakiekolwiek sugestie.<br>

## Spis treści
0. Przebieg życia programu
- 0.0. Tworzenie kodu źrodłowego
- 0.1. Kompilacja
  - 0.1.0. Preprocessing
  - 0.1.1. Analiza leksykalna
  - 0.1.2. Analiza składniowa
  - 0.1.3. Analiza semantyczna
  - 0.1.4. Parser & Lexer
  - 0.1.5. Assembling
  - 0.1.6. Optymalizacja
  - 0.1.7. Generacja obiektu
- 0.2. Linkowanie
- 0.3. Ładowanie programu
- 0.4. Uruchamianie programu
1. Biblioteka statyczna
- 1.0. Nazewnictwo
- 1.0. Tworzenie biblioteki statycznej
- 1.1. Dodawanie obiektów
- 1.2. Usuwanie obiektów
- 1.3. Zmiana kolejności obiektów
- 1.4. Eksport obiektów
- 1.4. Konwersja do biblioteki dynamicznej
- 1.5. Współpraca z binarkami

## Przebieg życia programu
W pierwszym rozdziale postaram się krótko i zwięźle omówić proces powstawania programu oraz proces jego uruchamiania.

### Tworzenie kodu źródłowego
Pierwszym etapem w drodze do utworzenia programu jest napisanie kodu źródłowego. Na tym etapie nie dzieje się zbytnio dużo. Otwieramy ulubiony edytor tekstu lub dedykowane IDE. Warto wspomnieć, że podczas pracy nad danym projektem staramy trzymać się pewnej hierarchi. Każdą inną różniącą się funcjonalność staramy się trzymać w osobnych plikach, te zaś pliki o podobnej charakterystyce trzymamy w jednym projekcie, ogół projektów trzymamy w solucji (typowa hierarchia plików w Visual Studio), natomiast produkt końcowy składać się będzie zwykle z różnych solucji przykładem może być firma, która zatrudnia dwie grupy programistów, w której jedna zajmuje się rozwiązaniami nad GUI, a druga rozwiązaniami po stronie backendu, obie grupy pracują nad inną specyfikacją aplikacji (w różnych solucjach).

### Kompilacja
Kompilacja to nic innego jak zamiana kodu źródłowego na plik obiektowy. Na wstępie warto zaznaczyć, że kompilację przeprowadza kompilator, który najpierw kompiluje nasz kod do kodu pośredniego w tym przypadku asemblera, a później tworzy z niego pliki binarne w tym przypadku pliki obiektowe, kompilowany jest każdy *translation unit* czyli każdy kod źródłowy. Popularnymi kompilatorami są GCC, MinGW (port GCC na windowsa), CLang C++, Microsoft Visual C++, IBM C++ i wiele [innych](http://www.stroustrup.com/compilers.html)... Oprócz kompilacji wyróżniamy: *cross-compilation* gdy kompilacja jest przeprowadzana na jednej platformie (CPU/OS) by wyprodukować kod na inną platformę, *decompilation* czyli odwrotnie do kompilacji proces zamiany kodu źródłowego języka niższego poziomu do języka wyższego poziomu. GCC umożliwia kompilację plików w następujący sposób (przykłady):
```bash
gcc -c <input>.c        # Kompilacja pojedyczego pliku
gcc -fPIC -c <intput.c> # Kompilacja modulu biblioteki dynamiczej, niezbedna flaga -fPIC dla kompatybilnosci z systemami x64
gcc -c <input>.c -lm    # Kompilacja pliku korzystajacego z biblioteki standardowej <math.h>
gcc -c <input>.c -L. -lfirst    # Kompilacja pliku korzystajacego z biblioteki statycznej libfirst.a znajdującej się w obecnym folderze
```

### Preprocessing
W pierwszym etapie kompilacji specjalny program nazywany preprocessorem (w tym przypadku C Preprocessor) parsuje pliki źrodłowe w celu:
- Załączenia kodu z plików wskazanych dyrektywą *#include*,
- Konwersji makro definicji na stałe wskazane przez dyrektywę *#define*,
- Zamiany makro definicji na kod,
- Włączenia lub wyłączenia danej części kodu wskazanej dyrektywą *#if*, *#elif* i *#endif*.<br>

Warto dodać, że jest możliwość stworzenia makro funkcji z nieznaną liczbą argumentów oraz [makra generyczne](https://mort.coffee/home/obscure-c-features/), warto poczytać również o tym jak działają inne preprocessory jak na przykład [m4](https://en.wikipedia.org/wiki/M4_(computer_language)). GCC umożliwia spojrzenie na to jak nasz kod został sprasowany przez preprocessor:
```bash
gcc -E <input>.c -o <output>.i
```

### Analiza leksykalna
W drugim etapie specjalnie do tego stworzony *lekser* tnie kod źródłowy na leksemy (tokeny). Warto dodać, że *leksery* ściśle współpracują z *parserami*. Każdy token posiada własne cechy. Idea analizy leksykalnej nie jest niczym skomplikowanym, cała trudność tkwi bowiem w napisaniu odpowiednich reguł przez, którym *lekser* będzie przypisywał odpowiednie tokeny. Przykładem może być token liczby całkowitej, pisząc wyrażenie regularne zakładamy, że będzię to ciąg znaków 0-9 o maksymalnej długości n, cytując *"...specyfikacja języka programowania obejmuje szereg reguł które definiują składnię leksykalną. Składnia leksykalna jest zazwyczaj opisana za pomocą wyrażeń regularnych. Definiują one zbiór możliwych sekwencji znakowych które tworzą pojedyncze tokeny. Lekser przetwarza oddzielone znakami białymi ciągi znaków i dla każdego ciągu znaków podejmuje akcję zazwyczaj produkując token, bądź ogłasza błąd analizy leksykalnej...."*.<br>
Przykład z [wikipedii](https://pl.wikipedia.org/wiki/Analiza_leksykalna) świetnie obrazuję jak *lekser* przeanalizował kod źródłowy:<br>
```C
int suma = 3 + 2;
```
| Leksem (token) | Kategoria |
| -------------- | --------- |
| int            | Identyfikator typ zmiennej całkowitej |
| suma           | Identyfikator zmiennej |
| =              | Operator przypisania |
| 3              | Literał całkowitoliczbowy |
| +              | Operator dodawania |
| 2              | Literał całkowitoliczbowy |
| ;              | Znacznik końca wyrażenia |

## Źródła
[Avanced C and C++ Compiling](https://doc.lagout.org/programmation/C/Advanced%20C%20and%20C%20%20%20Compiling%20%5BStevanovic%202014-04-28%5D.pdf)<br>
[Linkers and Loaders](http://www.becbapatla.ac.in/cse/naveenv/docs/LL1.pdf)<br>
[Anatomy of Program in Memory](https://manybutfinite.com/post/anatomy-of-a-program-in-memory/)<br>
[An Introduction to GCC](https://tfetimes.com/wp-content/uploads/2015/09/An_Introduction_to_GCC-Brian_Gough.pdf)<br>
[Beginner's Guide to Linkers](https://www.lurklurk.org/linkers/linkers.html)<br>
[Position Independent Code and x86-64 libraries](https://www.technovelty.org/c/position-independent-code-and-x86-64-libraries.html)<br>
[PLT and GOT - the key to code sharing and dynamic libraries](https://www.technovelty.org/linux/plt-and-got-the-key-to-code-sharing-and-dynamic-libraries.html)<br>
[Version Script](https://ftp.gnu.org/old-gnu/Manuals/ld-2.9.1/html_node/ld_25.html)<br>
[Calling Conventions Demystified](https://www.codeproject.com/Articles/1388/Calling-Conventions-Demystified)<br>
[The History of Calling Conventions](https://devblogs.microsoft.com/oldnewthing/20040102-00/?p=41213)<br>
[Name Mangling](https://en.wikipedia.org/wiki/Name_mangling)<br>
[Binary File Descriptor](https://en.wikipedia.org/wiki/Binary_File_Descriptor_library)
