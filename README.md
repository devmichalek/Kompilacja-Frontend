## Wstęp
Cześć, na wstępie chciałbym aby całą pracę włożoną w to repozytorium potraktować bardziej jako (obszerny) artykuł. Zebrane tutaj informacje są podsumowaniem całej zdobytej mojej wiedzy na temat tworzenia bibliotek dynamicznych (i nie tylko). Przykłady omawiane są w języku C i C++ oraz oczywiście w języku polskim ;). Możliwe, że artykuł nie pokrył wszystkich tematów związanych z bibliotekami jednak na pewno pokrywa ich zdecydowaną większość. Artykuł będzie co jakiś czas ulepszany, dodam, iż chętnie przyjmę jakiekolwiek sugestie.<br>

## Spis treści
0. [Przebieg życia programu](https://github.com/devmichalek/Biblioteki-Dynamiczne/blob/master/README.md#przebieg-%C5%BCycia-programu)
- 0.0. [Tworzenie kodu źrodłowego](https://github.com/devmichalek/Biblioteki-Dynamiczne/blob/master/README.md#tworzenie-kodu-%C5%BAr%C3%B3d%C5%82owego)
- 0.1. [Kompilacja](https://github.com/devmichalek/Biblioteki-Dynamiczne/blob/master/README.md#kompilacja)
  - 0.1.0. [Preprocessing](https://github.com/devmichalek/Biblioteki-Dynamiczne/blob/master/README.md#preprocessing)
  - 0.1.1. [Analiza leksykalna](https://github.com/devmichalek/Biblioteki-Dynamiczne/blob/master/README.md#analiza-leksykalna)
  - 0.1.2. [Analiza składniowa](https://github.com/devmichalek/Biblioteki-Dynamiczne/blob/master/README.md#analiza-sk%C5%82adniowa)
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
Kompilacja to nic innego jak zamiana kodu źródłowego na plik obiektowy. Na wstępie warto zaznaczyć, że kompilację przeprowadza kompilator, który najpierw kompiluje nasz kod do kodu pośredniego w tym przypadku asemblera, a później tworzy z niego pliki binarne w tym przypadku pliki obiektowe, kompilowany jest każdy *translation unit* czyli każdy kod źródłowy. Popularnymi kompilatorami są GCC, MinGW (port GCC na windowsa), CLang C++, Microsoft Visual C++, IBM C++ i wiele [innych](http://www.stroustrup.com/compilers.html)... Oprócz kompilacji wyróżniamy: *cross-compilation* gdy kompilacja jest przeprowadzana na jednej platformie (CPU/OS) by wyprodukować kod na inną platformę, *decompilation* czyli odwrotnie do kompilacji proces zamiany kodu źródłowego języka niższego poziomu do języka wyższego poziomu.

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
W następnym etapie specjalnie do tego stworzony *lekser* tnie kod źródłowy na leksemy (leksem to najmniejszy unit, leksemami są np. litery, cyfry, znaki specjalne itd.). Warto dodać, że *leksery* ściśle współpracują z *parserami*. Każdy token posiada własne cechy (np. int, std::string, char to tokeny reprezentujące typ). Idea analizy leksykalnej nie jest niczym skomplikowanym, cała trudność tkwi bowiem w napisaniu odpowiednich wyrażen regularnych przez, które *lekser* będzie w stanie dopasować ciąg znaków do danego *tokenu*. Przykładem może być token liczby całkowitej, pisząc wyrażenie regularne zakładamy, że będzię to ciąg znaków 0-9 o maksymalnej długości n, cytując *"...specyfikacja języka programowania obejmuje szereg reguł które definiują składnię leksykalną. Składnia leksykalna jest zazwyczaj opisana za pomocą wyrażeń regularnych. Definiują one zbiór możliwych sekwencji znakowych które tworzą pojedyncze tokeny. Lekser przetwarza oddzielone znakami białymi ciągi znaków i dla każdego ciągu znaków podejmuje akcję zazwyczaj produkując token, bądź ogłasza błąd analizy leksykalnej...."*. Jednakże warto zapamiętać, że *token* to nie *leksem*, *leksemami* są po prostu znaki natomiast tokeny to odpowiednio ułożone leksemy. Żeby za bardzo nie mieszać, dla przykładu ```123``` to zwykły tekst, każdy z tych znaków to *leksem* **cyfra**, natomiast ułożone obok siebie dają token **liczba całkowita**.<br>
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

### Analiza składniowa
Podczas analizy składniowej parser z wcześniej przygotowanych przez leksera grup leksemów (tokenów) tworzy wyrażenia (jeśli znajdzie istniejącą regułę) i dodaje je do drzewa. Dla powyższego przykładu parser znajdzie regułe, w której są dwie liczby całkowite między, którymi jest znak dodawania i zamieni wyrażenie ``` 3 + 2``` na ``` 5```, następnie na przykład wracając się po wyrażeniu (parser działa rekurencyjnie) znajdzie regułę, w której przypisywana jest liczba do identyfikatora zmiennej ```suma = 5;```. Na tym etapie dopuszczalnym jest np. przypisanie nieprawidłowego typu do identyfikatora zmiennej (np. do zmiennej typu int przypisujemy std::string), ponieważ głównym zadaniem parsera jest stworzenie struktury, a nie dogłębne analizowanie i sprawdzanie typów (krótko mówiąc szkoda na to czasu). Wiemy już co robi *lekser* oraz czym zajmuję się *parser*. Dodam, że powód dla którego oba te mechanizmy współpracują razem to optymalizacja, *lekser* na bieżąco wypluwa gotowe tokeny, natomiast *parser* natychmiastowo próbuje dopasować do nich pasujące wyrażenie, oczywiście była by możliwość np. wrzucania *leksemów* do pliku, z którego czytałby *parser* (wtedy tak jakby dwa razy czytamy znaki, jednak znajduje to swoje zastosowanie). Warto również wiedzieć, że wymyślono różne sposoby parsowania tj. m. in. analizą zstępująca (top-down, rzadziej stosowana, czasami brakuje "abstrakcji" by coś można było nazwać "topem" danego wyrażenia) i analizą wstępująca (bottom-up, częściej stosowana, łatwiej zacząć znajdując mniejsze fragmenty) ,te zaś dzielą się na metody kierunkowe i niekierunkowe. Gdy parser analizuje wstępując (bottom-up) składa bezpośrednio grupy tokenów idąc w górę w całość, dla przykładu (struktura):
```bison
wyrazenie : liczba			    {$$ = $1;}
	| wyrazenie '+' wyrazenie	{$$ = $1 + $3;}
	| wyrazenie '-' wyrazenie	{$$ = $1 - $3;}
	| wyrazenie '*' wyrazenie	{$$ = $1 * $3;}
	| wyrazenie '/' wyrazenie	{$$ = $1 / $3;}
  | '(' wyrazenie ')'	      {$$ = $2;}
	;
```
Z powyższej struktury rozumiemy kolejno, że wyrazenie to liczba (wtedy przypisz wartość liczby $1 do wyrazenia $$), *wyrazenie* to również *wyrazenie* + *wyrazenie* (przypisz wartość wyrażen $1 + $2 do wyrażenia po lewej) itd. Jak widać struktura jest rekurencyjna. Teraz działając wg. zasady *bottom-up* dla ```1 + 2``` parser najpierw znajdzie ```1``` będzie to token *liczba*, liczbę te przypisze do wyrażenia, później parser znajdzie token ```+``` (pozostawi bez zmian bo nie znalazł jeszcze pasującej reguły), następnie znajdzie ```2``` czyli kolejny token *liczba*, później mając te trzy tokeny dopiero wtedy znajduję *wyrazenie '+' wyrazenie* i zamienia je na *wyrazenie*. Działanie analizy zstępującej pozostawiam ciekawskim.

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
