## Wstęp
Cześć, na wstępie chciałbym aby całą pracę włożoną w to repozytorium potraktować bardziej jako (bardzo obszerny) artykuł. Zebrane tutaj informacje są podsumowaniem całej zdobytej mojej wiedzy na temat tworzenia bibliotek dynamicznych, procesu kompilacji, linkowania i innych. Przykłady omawiane są w języku C i C++ oraz oczywiście w języku polskim ;). Możliwe, że artykuł nie pokrył wszystkich tematów związanych z bibliotekami jednak na pewno pokrywa ich zdecydowaną większość. Artykuł będzie co jakiś czas ulepszany, dodam, iż chętnie przyjmę jakiekolwiek sugestie.<br>

## Spis treści
0. [Przebieg życia programu](https://github.com/devmichalek/Biblioteki-Dynamiczne/blob/master/README.md#przebieg-%C5%BCycia-programu)
- 0.0. [Tworzenie kodu źrodłowego](https://github.com/devmichalek/Biblioteki-Dynamiczne/blob/master/README.md#tworzenie-kodu-%C5%BAr%C3%B3d%C5%82owego)
- 0.1. [Kompilacja](https://github.com/devmichalek/Biblioteki-Dynamiczne/blob/master/README.md#kompilacja)
  - 0.1.0. [Preprocessing](https://github.com/devmichalek/Biblioteki-Dynamiczne/blob/master/README.md#preprocessing)
  - 0.1.1. [Analiza leksykalna - skanowanie](https://github.com/devmichalek/Biblioteki-Dynamiczne/blob/master/README.md#analiza-leksykalna---skanowanie)
    - 0.1.1.0 [Jak działa lekser?](https://github.com/devmichalek/Biblioteki-Dynamiczne/blob/master/README.md#jak-dzia%C5%82a-lekser)
    - 0.1.1.1 [Wyrażenia regularne](https://github.com/devmichalek/Biblioteki-Dynamiczne/blob/master/README.md#wyra%C5%BCenia-regularne)
    - 0.1.1.2 [NFA - Niedeterministyczny Automat Skończony](https://github.com/devmichalek/Biblioteki-Dynamiczne/blob/master/README.md#nfa---niedeterministyczny-automat-sko%C5%84czony)
    - 0.1.1.3 [Maximal Munch](https://github.com/devmichalek/Biblioteki-Dynamiczne/blob/master/README.md#maximal-munch)
    - 0.1.1.4 [DFA - Deterministyczny Automat Skończony](https://github.com/devmichalek/Biblioteki-Dynamiczne/blob/master/README.md#dfa---deterministyczny-automat-sko%C5%84czony)
  - 0.1.2. [Analiza składniowa - parsowanie](https://github.com/devmichalek/Biblioteki-Dynamiczne/blob/master/README.md#analiza-sk%C5%82adniowa)
  - 0.1.3. [Analiza semantyczna](https://github.com/devmichalek/Biblioteki-Dynamiczne/blob/master/README.md#analiza-semantyczna)
  - 0.1.4. [Generacja IR]()
  - 0.1.5. [Optymalizaja IR]()
  - 0.1.6. [Generacja kodu]()
  - 0.1.7. [Optymalizacja]()
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
W pierwszym rozdziale szczegółowo omawiam proces kompilacji oraz proces uruchamiania programu.

### Tworzenie kodu źródłowego
Pierwszym etapem w drodze do utworzenia programu jest napisanie kodu źródłowego. Na tym etapie nie dzieje się zbytnio dużo. Otwieramy ulubiony edytor tekstu lub dedykowane IDE. Warto wspomnieć, że podczas pracy nad danym projektem staramy trzymać się pewnej hierarchi. Każdą inną różniącą się funcjonalność staramy się trzymać w osobnych plikach, te zaś pliki o podobnej charakterystyce trzymamy w jednym projekcie, ogół projektów trzymamy w solucji (typowa hierarchia plików w Visual Studio), natomiast produkt końcowy składać się będzie zwykle z różnych solucji przykładem może być firma, która zatrudnia dwie grupy programistów, w której jedna zajmuje się rozwiązaniami nad GUI, a druga rozwiązaniami po stronie backendu, obie grupy pracują nad inną specyfikacją aplikacji (w różnych solucjach).

### Kompilacja
Zacznijmy od tego, że kod który napisaliśmy w danym języku kompilujemy nie tylko po to aby otrzymać plik obiektowy ale również po to aby sprawdzić czy jest on poprawny gramatycznie (np. czy brak w nim polskich znaków?), czy pomimo braków błędów językowych struktura naszego kodu jest dopuszczalna (np. czy definicja funkcji zamyka się w klamrach?), czy pomimo poprawnej struktury kodu nie popełniliśmy błędu związanego z semantyką (np. czy nie próbujemy przypisać std::string do int?) oraz szereg innych rzeczy, które będą szczegółowo omawiane później. (Nie)stety sama znajomość języka programowania bez wiedzy jak nasz kod jest budowany nie wystarcza, dlatego postaram się szczegółowo omówić jak wygląda proces kompilacji krok po kroku.

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

### Analiza leksykalna - skanowanie
W następnym etapie omówiona zostania analiza leksykalna, nazwa ta pochodzi od programu, który ją wykonuje czyli leksera inaczej nazywanego skanerem.
#### Jak działa lekser?
Cała przygoda rozpoczyna się wtedy gdy lekser na wejście dostaje gotowy plik podczas, którego tnie kod źródłowy na leksemy, leksemami nazywamy najmniejszą cząstke czyli po prostu znaki, w których skład wchodzą litery, cyfry, znaki specjalne itd. Jak później można się przekonać, leksery ściśle współpracują z parserami. Mając daną grupę leksemów, skaner stara się dopasować ją do istniejącej listy tokenów. W zależności od języka i dla wygody tokeny są grupowane, przykładowo int, float i char będą należały do tej samej grupy tokenów skojarzonych z typami zmiennych. Żeby zobaczyć czym są tokeny oraz, że niektóre z nich posiadają atrybuty warto spojrzeć na poniższy kod:
```C
while (137 < i)
	++i;
```
Słowo kluczowe ```while``` będzie reprezentowane przez token *T_While*, natomiast liczba ```137``` będzie reprezentowana przez token *T_IntConst* oraz będzie posiadała atrybut , w której przechowana zostanie wspomniana wcześniej liczba. Poniższa animacja obrazuje to jeszcze lepiej. Animacje stworzyłem na bazie jednej z [prezentacji](https://web.stanford.edu/class/archive/cs/cs143/cs143.1128/lectures/01/Slides01.pdf) o analizie leksykalnej wykładanej na Stanfordzie.
![skanowanie](https://user-images.githubusercontent.com/19840443/63093185-428b1200-bf75-11e9-9364-1d51211e3768.gif)
Zazwyczaj słowa kluczowe danego języka posiadają swoje własne tokeny, również i znaki specjalne ```{}();,[]``` z reguły posiadają swój własny token, dodatkowo tokeny należace do tej samej grupy są po prostu grupowane (tak jak wcześniej wspomniane tokeny typów zmiennych), natomiast nic nie znaczące spacje, tabulacje lub komentarze są pomijane. Inny przykład tym razem z [wikipedii](https://pl.wikipedia.org/wiki/Analiza_leksykalna) świetnie obrazuję jak lekser przeanalizował kod źródłowy:<br>
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

Omawiane wcześniej przykłady to jedne z najprostszych do rozwiązania dla skanera. Weźmy pod uwagę fakt, że w C++ jest możliwość tworzenia szablonów, dla poniższego przykładu patrząc z perspektywy leksera wyodrębnienie zagnieżdżonego std::vector nie jest prostym zadaniem:
```C++
std::vector<std::vector<int>> vec
```
Również gdy ktoś użył słów kluczowych jako nazwy zmiennej:
```pascal
if then then then = else; else else = if
```
Cała więc trudność tkwi w napisaniu odpowiednich reguł czyli wyrażen regularnych przez, które lekser będzie w stanie dopasować ciąg znaków do danego tokenu. Przykładem może być token liczby całkowitej, pisząc wyrażenie regularne zakładamy, że będzię to ciąg znaków 0-9, cytując *"...specyfikacja języka programowania obejmuje szereg reguł które definiują składnię leksykalną. Składnia leksykalna jest zazwyczaj opisana za pomocą wyrażeń regularnych. Definiują one zbiór możliwych sekwencji znakowych które tworzą pojedyncze tokeny. Lekser przetwarza oddzielone znakami białymi ciągi znaków i dla każdego ciągu znaków podejmuje akcję zazwyczaj produkując token, bądź ogłasza błąd analizy leksykalnej...."*. Wiedząc to wszystko jak możemy napisać swój własny skaner? Zacznijmy od tego, że nikt nie piszę własnych lekserów od zera, są ku temu trzy powody: ilość czasu jaką trzeba by było poświęcić na napisanie kodu od zera w związku z tym strata pieniędzy, ale przedewszystkim błędy, które mogą się pojawić podczas pisania na piechotę... Obecnie do pisania lekserów używa się generetarów. Żeby stworzyć skaner można posłużyć się programem Flex, który wygeneruje nam nasz własny kod skanera (kod skanera, ponieważ brakuje nam jeszcze kodu parsera gdzie całość zostanie skompilowana). [Flex](https://en.wikipedia.org/wiki/Flex_(lexical_analyser_generator)) to następca oraz "świeższa" alternatywa dla generatora [Lex](https://en.wikipedia.org/wiki/Lex_(software)). Jednak zanim napiszemy swój własny kod rozumiany przez generatora Flex, zacznijmy od tego jak [wyrażenia regularne](https://en.wikipedia.org/wiki/Regular_expression) zostały zaimplementowane.

#### Wyrażenia regularne
Wyrażenia regularne *(regular expression)* przydatne są wszędzie tam gdzie w łańcuchu znaków zależy nam na wyszukaniu słowa klucza. Nic dziwnego więc, że używane są w różnego typu skanerach tekstu. Wyrażenia te mogą zostać zaimplementowane za pomocą automatów skończonych *(finite automata)*, w której wyróżniamy dwa główne NFA (Nondeterministic Finite Automata) i DFA (Deterministic Finite Automata). Automaty najlepiej wytłumaczyć obrazując ich działanie. Na poniższej animacji okrąg oznacza stan *(state*), w którym się znajdujemy, natomiast strzałka *(transition)* oznacza przejście w inny stan gdy zajdzie podany warunek, ostatni znów podwójny okrąg wskazuje na stan wyjściowy *(accepting state)*, automat akceptuje stringa jesli znajduje się w stanie wyjściowym.
![poprawny](https://user-images.githubusercontent.com/19840443/63118794-a6323100-bfaf-11e9-86c0-03e6b35938c0.gif)

Poniżej również animacje przejść w automatach, które nie zaakceptowały wejścia, w pierwszej brakuje kolejnego przejścia na końcu stanu wyjściowego, natomiast w drugiej brakuje przejścia dzięki któremu przeszlibysmy do stanu wyjściowego.
![niepoprawny](https://user-images.githubusercontent.com/19840443/63119391-168d8200-bfb1-11e9-9172-15d3e851518f.gif)
![niepoprawny](https://user-images.githubusercontent.com/19840443/63119904-412c0a80-bfb2-11e9-9f0b-d00114db4daa.gif)

#### NFA - Niedeterministyczny Automat Skończony

#### Maximal Munch

#### DFA - Deterministyczny Automat Skończony
Dodatkowo warto samemu sprawdzić jak działają automaty [Büchiego](https://pl.wikipedia.org/wiki/Automat_B%C3%BCchiego), [Mealy’ego](https://pl.wikipedia.org/wiki/Automat_Mealy%E2%80%99ego), [Moore’a](https://pl.wikipedia.org/wiki/Automat_Moore%E2%80%99a) oraz legendarnej [maszynie Turinga](https://pl.wikipedia.org/wiki/Maszyna_Turinga).

### Analiza składniowa
Podczas analizy składniowej parser z wcześniej przygotowanych przez leksera grup leksemów (tokenów) tworzy wyrażenia (jeśli znajdzie istniejącą regułę) i dodaje je do drzewa. Dla powyższego przykładu parser znajdzie regułe, w której są dwie liczby całkowite między, którymi jest znak dodawania i zamieni wyrażenie ``` 3 + 2``` na ``` 5```, następnie na przykład wracając się po wyrażeniu (parser działa rekurencyjnie) znajdzie regułę, w której przypisywana jest liczba do identyfikatora zmiennej ```suma = 5;```. Na tym etapie dopuszczalnym jest np. przypisanie nieprawidłowego typu do identyfikatora zmiennej (np. do zmiennej typu int przypisujemy std::string), ponieważ głównym zadaniem parsera jest stworzenie struktury, a nie dogłębne analizowanie i sprawdzanie typów (krótko mówiąc szkoda na to czasu). Wiemy już co robi *lekser* oraz czym zajmuję się *parser*. Dodam, że powód dla którego oba te mechanizmy współpracują razem to optymalizacja, *lekser* na bieżąco wypluwa gotowe tokeny, natomiast *parser* natychmiastowo próbuje dopasować do nich pasujące wyrażenie, oczywiście była by możliwość np. wrzucania *leksemów* do pliku, z którego czytałby *parser* (wtedy tak jakby dwa razy czytamy znaki, jednak znajduje to swoje zastosowanie). Warto również wiedzieć, że wymyślono różne sposoby parsowania tj. m. in. analizą zstępująca (top-down, rzadziej stosowana, czasami brakuje "abstrakcji" by coś można było nazwać "topem" danego wyrażenia) i analizą wstępująca (bottom-up, częściej stosowana, łatwiej zacząć znajdując mniejsze fragmenty), te zaś dzielą się na metody kierunkowe i niekierunkowe. Gdy parser analizuje wstępując (bottom-up) składa bezpośrednio grupy tokenów idąc w górę w całość, dla przykładu (struktura):
```bison
wyrazenie : liczba			{$$ = $1;}
	| wyrazenie '+' wyrazenie	{$$ = $1 + $3;}
	| wyrazenie '-' wyrazenie	{$$ = $1 - $3;}
	| wyrazenie '*' wyrazenie	{$$ = $1 * $3;}
	| wyrazenie '/' wyrazenie	{$$ = $1 / $3;}
  | '(' wyrazenie ')'	                {$$ = $2;}
	;
```
Z powyższej struktury rozumiemy kolejno, że wyrazenie to liczba (wtedy przypisz wartość liczby $1 do wyrazenia $$), *wyrazenie* to również *wyrazenie* + *wyrazenie* (przypisz wartość wyrażen $1 + $2 do wyrażenia po lewej) itd. Jak widać struktura jest rekurencyjna. Teraz działając wg. zasady *bottom-up* dla ```1 + 2``` parser najpierw znajdzie ```1``` będzie to token *liczba*, liczbę te przypisze do wyrażenia, później parser znajdzie token ```+``` (pozostawi bez zmian bo nie znalazł jeszcze pasującej reguły), następnie znajdzie ```2``` czyli kolejny token *liczba*, później mając te trzy tokeny dopiero wtedy znajduję *wyrazenie '+' wyrazenie* i zamienia je na *wyrazenie*. Działanie analizy zstępującej pozostawiam ciekawskim.

### Analiza semantyczna
Podczas analizy semantycznej na podstawie wcześniej sprawdzonej i utworzonej struktury drzewa następuję sprawdzanie poprawności typów, instrukcji i programu jako całości (analiza ta sprawdza czy program ma jakikolwiek sens). Ilość zadań i poziom skomplikowania podczas tej analizy zależy w głównej mierze od specyfikacji języka. W wielu źródłach podawane są przykłady 

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
[Binary File Descriptor](https://en.wikipedia.org/wiki/Binary_File_Descriptor_library)<br>
[Stanford CS143 About Compilation](https://web.stanford.edu/class/archive/cs/cs143/cs143.1128/lectures/)<br>
