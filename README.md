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
    - 0.1.1.5 [Flex](https://github.com/devmichalek/Biblioteki-Dynamiczne/blob/master/README.md#flex)
  - 0.1.2. [Analiza składniowa - parsowanie](https://github.com/devmichalek/Biblioteki-Dynamiczne/blob/master/README.md#analiza-sk%C5%82adniowa)
    - 0.1.2.0 [Jak działa parser?](https://github.com/devmichalek/Biblioteki-Dynamiczne/blob/master/README.md#jak-dzia%C5%82a-parser)
    - 0.1.2.1 [Gramatyka bezkontekstowa](https://github.com/devmichalek/Biblioteki-Dynamiczne/blob/master/README.md#gramatyka-bezkontekstowa)
    - 0.1.2.2 [Drzewo składniowe](https://github.com/devmichalek/Biblioteki-Dynamiczne/blob/master/README.md#drzewo-sk%C5%82adniowe)
    - 0.1.2.3 [Analiza zstępująca](https://github.com/devmichalek/Biblioteki-Dynamiczne#analiza-zst%C4%99puj%C4%85ca)
      - 0.1.2.3.0 [Przeszukiwanie wszerz](https://github.com/devmichalek/Biblioteki-Dynamiczne#przeszukiwanie-wszerz)
      - 0.1.2.3.1 [Przeszukiwanie wgłąb](https://github.com/devmichalek/Biblioteki-Dynamiczne#przeszukiwanie-wg%C5%82%C4%85b)
      - 0.1.2.3.2 [LL(1)](https://github.com/devmichalek/Biblioteki-Dynamiczne#ll1)
    - 0.1.2.4 [Analiza wstępująca]()
    - 0.1.2.5 [Bison](https://github.com/devmichalek/Biblioteki-Dynamiczne/blob/master/README.md#bison)
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

Warto dodać, że jest możliwość stworzenia makro funkcji z nieznaną liczbą argumentów oraz [makra generyczne](https://mort.coffee/home/obscure-c-features/), warto poczytać również o tym jak działają inne preprocessory jak na przykład [m4](https://en.wikipedia.org/wiki/M4_(computer_language)). GCC umożliwia spojrzenie na to jak nasz kod został sparsowany przez preprocessor:
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
| Leksemy        | Kategoria |
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
Cała więc trudność tkwi w napisaniu odpowiednich reguł czyli wyrażen regularnych przez, które lekser będzie w stanie dopasować ciąg znaków do danego tokenu. Przykładem może być token liczby całkowitej, pisząc wyrażenie regularne zakładamy, że będzię to ciąg znaków 0-9, cytując *"...specyfikacja języka programowania obejmuje szereg reguł które definiują składnię leksykalną. Składnia leksykalna jest zazwyczaj opisana za pomocą wyrażeń regularnych. Definiują one zbiór możliwych sekwencji znakowych które tworzą pojedyncze tokeny. Lekser przetwarza oddzielone znakami białymi ciągi znaków i dla każdego ciągu znaków podejmuje akcję zazwyczaj produkując token, bądź ogłasza błąd analizy leksykalnej...."*. Wiedząc to wszystko nasuwa się pytanie: jak napisać swój własny skaner? Zacznijmy od tego, że nikt nie piszę własnych lekserów od zera, są ku temu trzy powody: ilość czasu jaką trzeba było by poświęcić na napisanie kodu od zera w związku z tym strata pieniędzy, ale przede wszystkim błędy, które mogą się pojawić podczas pisania na piechotę... Obecnie do pisania lekserów używa się generatarów. Do stworzenia skanera posłużyć się można programem Flex, który wygeneruje nam nasz własny kod skanera (kod skanera w języku C, ponieważ brakuje nam jeszcze kodu parsera gdzie całość zostanie skompilowana). [Flex](https://en.wikipedia.org/wiki/Flex_(lexical_analyser_generator)) to następca oraz "świeższa" alternatywa dla generatora [Lex](https://en.wikipedia.org/wiki/Lex_(software)). Jednak zanim napiszesz swój własny skaner, zacznijmy od tego jak [wyrażenia regularne](https://en.wikipedia.org/wiki/Regular_expression) zostały zaimplementowane.

#### Wyrażenia regularne
Wyrażenia regularne *(regular expression)* przydatne są wszędzie tam gdzie w łańcuchu znaków zależy nam na wyszukaniu słowa klucza. Nic dziwnego więc, że używane są w różnego typu skanerach tekstu. Wyrażenia te mogą zostać zaimplementowane za pomocą automatów skończonych *(finite automata)*, w której wyróżniamy dwa główne NFA (Nondeterministic Finite Automata) i DFA (Deterministic Finite Automata). Automaty najlepiej wytłumaczyć obrazując ich działanie. Na poniższej animacji okrąg oznacza stan *(state*), w którym się znajdujemy, natomiast strzałka *(transition)* oznacza przejście w inny stan gdy zajdzie podany warunek, ostatni znów podwójny okrąg wskazuje na stan akceptacji *(accepting state)*, automat akceptuje stringa jesli znajduje się w stanie akceptacji (na poniższych trzech animacjach przedstawiony jest NFA).
![poprawny](https://user-images.githubusercontent.com/19840443/63118794-a6323100-bfaf-11e9-86c0-03e6b35938c0.gif)
Poniżej również animacje przejść w automatach, które nie zaakceptowały wejścia, w pierwszej brakuje kolejnego przejścia na końcu stanu akceptacji, natomiast w drugiej brakuje przejścia dzięki któremu przeszlibyśmy do stanu akceptacji.
![niepoprawny](https://user-images.githubusercontent.com/19840443/63119391-168d8200-bfb1-11e9-9172-15d3e851518f.gif)
![niepoprawny](https://user-images.githubusercontent.com/19840443/63119904-412c0a80-bfb2-11e9-9f0b-d00114db4daa.gif)
Celem automatu jest rozstrzygnięcie w skończonej ilości kroków, czy dowolne słowo należy, czy też nie należy do języka badanego przez automat. Teoria automatów jest efektem badań zapoczątkowanych przez A. Turinga. Automat reaguje na określone sygnały, zmieniając swój stan. Jeśli ustalimy, że sygnały będą reprezentowane przez poszczególne litery alfabetu, zdefiniujemy stan początkowy automatu oraz dopuszczalne stany końcowe, to automat może testować dowolne słowo, startując ze stanu początkowego. Słowo zostanie rozpoznane przez automat i uznane za poprawne, jeżeli rezultatem działania będzie założony stan końcowy.

#### NFA - Niedeterministyczny Automat Skończony
Na początku kilka suchych faktów:
- każde wyrażenie regularne jesteśmy w stanie zamienić na NFA
- mając stringa o długości **m** jesteśmy w stanie znależć klucz w czasie **O(mn^2)**

Dlaczego NFA i DFA są tak mocno związane z wyrażeniami regularnymi? W automacie niedeterministycznym, dla danego znaku, nie musi z każdego stanu wychodzić dokładnie jedno przejście odpowiadające wczytaniu tego znaku. Takich przejść może nie być, może być jedno, lub może być wiele. Podobnie, automat niedeterministyczny może mieć wiele stanów początkowych. Ta koncepcja wydaje się idealnie pasować w nature wyrażen regularnych. Warto wiedzieć, że wyrażenia regularne zaimplementowane w sposob NFA wykonują się wolniej od tych zaimplementowanych w sposób DFA, jednakże plusem jest mała ilość pamięci, której potrzebują.
Dodatkowo warto sprawdzić czym jest [Automat Büchiego](https://pl.wikipedia.org/wiki/Automat_B%C3%BCchiego)

#### Maximal Munch
Ten nagłówek musiał się pojawić, wyrażenia regularne to nie tylko szukanie klucza w stringu, ale szukanie **najdłuższego pasującego** klucza. "Maximal Munch" to inaczej "The Longest Match", najdłuższe pasujące wyrażenie. Mając wiele wyrażeń regularnych w jaki sposób zaimplementować "Maximal Munch"? Trzy niezbędne czynności to:
- zamiana wyrażeń regularnych na NFA
- uruchomienie wszystkich NFA równocześnie, zachowując *last match*
- gdy wszystkie automaty są w stanie, w którym nie ma przejść to zwróć *last match* i zacznij szukać ponownie w miejscu, w którym skończyłeś

Przypuśćmy, że mamy sytuacje, w której analizujemy ciąg znaków ```DOUBDOUBLE``` oraz, że mamy do dyspozycji trzy tokeny ```T_Do```, ```T_Double```, ```T_Mystery```, które reprezentują kolejno: słowo kluczowe ```do```, słowo kluczowe ```double```, string zawierający jedną literę małą lub dużą. Token o najwyższym priorytecie umieszczony jest najwyżej. Ponownie, animacja najlepiej przedstawi nasz problem:
![Maximal Munch](https://user-images.githubusercontent.com/19840443/63158357-19788900-c02a-11e9-881e-5dc1c562b861.gif)

#### DFA - Deterministyczny Automat Skończony
Kilka faktów:
- każde wyrażenie regularne jesteśmy w stanie zamienić na DFA (najpierw na NFA a później na DFA)
- mając stringa o długości **m** jesteśmy w stanie znależć klucz w czasie **O(m)**

Główna różnica między NFA to taka, że w tym przypadku posiadamy jeden stan początkowy, jedno przejście z każdego stanu oraz jeden stan akceptacji. Siła automatów niedeterministycznych jest dokładnie taka sama, jak deterministycznych, dla każdego niedeterministycznego automatu skończonego istnieje równoważny mu automat deterministyczny. Wyrażenia regularne zaimplementowane na wzór DFA są szybkie jednak mocno obciążają pamięć (często używana jest hybryda).
Dodatkowo warto sprawdzić jak działają automaty [Mealy’ego](https://pl.wikipedia.org/wiki/Automat_Mealy%E2%80%99ego), [Moore’a](https://pl.wikipedia.org/wiki/Automat_Moore%E2%80%99a) oraz legendarną [maszyne Turinga](https://pl.wikipedia.org/wiki/Maszyna_Turinga).

#### Flex
Wreszcie doszliśmy do momentu, w którym sami możemy się sprawdzić jako projektanci leksera. Teraz w zależności od tego co chcielibyśmy osiągnąć skaner dla każdego przypadku będzie się drametralnie różnił. Wszędzie tam gdzie wyrażenia regularne nie wystarczają jesteśmy w stanie zaimplementować lekser. Tak jak wspomniałem na początku nagłówka o analizie leksykalnej, lekser ściśle współpracuje z parserem, dlatego też Flex współpracuje z generatorem parserów tj. Bison'em. Czym są parsery? Jak działają? Dlaczego sam lekser nie wystarcza? To wszystko znajdziecie poniżej ;). [Tutaj](https://github.com/devmichalek/Biblioteki-Dynamiczne/blob/master/README.md#bison) umieściłem nagłówek o Bison'ie oraz liste bardzo dobrze zrobionych tutoriali dla zaczynających przygodę z Flex & Bison, których nie ma sensu duplikować.

### Analiza składniowa
W następnym etapie omówiona zostanie analiza składniowa podczas, której parser manipuluje otrzymanymi przez leksera tokenami.

#### Jak działa parser?
Jeśli kompilator znalazł się w etapie analizy składniowej oznacza to, że nasz kod jest leksykalnie poprawny, tj. udało się stworzyć tokeny z każdego znaku (lub ciągu znaków) znalezionego w pliku. Właściwie analize składniową ciężko nazwać następnym etapem ponieważ lekser i parser pracują nieustannie rownocześnie obok siebie, jednak dla uproszczenia napisałem, że jest to kolejny etap. Przykładowy kod poprawny leksykalnie:
```C
while (ip < z)
	++ip;
```
Teraz obrazek ilustrujący na jakim etapie się znajdujemy, nasz kod został pocięty na leksemy, a z grup leksemów zostały utworzone następujące tokeny:
![Poprawne tokeny](https://user-images.githubusercontent.com/19840443/63163268-0b306a00-c036-11e9-9a2d-1fec7bcac47b.png)
Kolejny przykład kodu poprawnego leksykalnie:
```C
do[for] = new 0;
```
Obrazek ilustrujący utworzone tokeny:
![Niepoprawne tokeny](https://user-images.githubusercontent.com/19840443/63163269-0b306a00-c036-11e9-929d-b57cf502666c.png)
Jak widać coś poszło nie tak, nasz kod faktycznie jest poprawny leksykalnie, ale niektóre z tych tokenów to słowa kluczowe, przykładem jest ```for```, które zostało użyte jako identyfikator (gdzieś wcześniej) prawdopodobnie jakiejś liczby całkowitej, lub ```do``` użyte prawdopodobnie jako nazwa tablicy.<br>

Podczas analizy składniowej parser z wcześniej przygotowanych przez leksera tokenów stara się dopasować istniejącą strukturę, w przypadku gdy jej nie znajdzie zwróci błąd analizy składniowej. Podczas skanowania naszym alfabetem były znaki ASCII lub Unicode tj. kod źródłowy, podczas parsowania naszym alfabetem jest zestaw utworzonych tokenów, również, podczas skanowania użyliśmy wyrażeń regularnych do opisu każdego tokenu, niestety, ale w przypadku parsowania wyrażenia regularne są zbyt słabe do opisania struktury gramatycznej, w tym przypadku konieczne jest użycie innego narzędzia.

#### Gramatyka bezkontekstowa
Context-Free Grammar lub po prostu CFG to zestaw zasad za pomocą, których opisywane są struktury rozumiane przez parsera. Przypuśćmy, że chcemy opisać operacje arytmetyczne takie jak dodawanie, odejmowanie, mnożenie i dzielenie. Po lewej przedstawiono zasady dla operacji arytmetycznych, natomiast po prawej utworzona struktura z działania ```10 * (1 + 2)```
![CFG](https://user-images.githubusercontent.com/19840443/63223999-081cb180-c1cf-11e9-9de2-ad634f7ca6c7.png)
- ![#ff0000](https://placehold.it/15/ff0000/000000?text=+) - ```E``` i ```Op```, symbole nieterminalne, rozpoczynajace się dużymi literami np. A, Bb, CcC
- ![#0000ff](https://placehold.it/15/0000ff/000000?text=+) - ```int```, ```()```, ```+```, ```-```, ```*```, ```/```, symbole terminalne, rozpoczynajace się małymi literami np. e, ff, gG
- ![#737373](https://placehold.it/15/737373/000000?text=+) - symbol dowolny (symbol terminalny i/lub symbol nieterminalny), zwykle oznaczany małymi literami greckimi np. α, γ

Ważne, aby zrozumieć, że sładnia wyrażeń regularnych nie może być użyta do opisu CFG.<br>
![CFG](https://user-images.githubusercontent.com/19840443/63224323-782d3680-c1d3-11e9-8b85-d66e84ef8567.png)<br>
Przykładowo ```*``` w wyrażeniach regularnych oznacza 0 lub więcej wystąpień, dla ```a*b```, dopasowane zostałyby słowa ```ab```, ```b```, ```aaaaab```, taka funkcjonalność nie jest jednak wspierana przez CFG.<br>
Warto wiedzieć, że istnieją dwa sposoby rozwijania symboli przez drzewo parsujące *(parse tree)* nazywane derywacją *(derivation)*. Poniżej przedstawiono: *Leftmost Derivation* tj. w każdym kolejnym kroku rozwija się pierwszy z lewej symbol nieterminalny, *Rightmost derivation*  tj. w każdym kolejnym kroku rozwija się pierwszy z prawej symbol nieterminalny.
![Derivation](https://user-images.githubusercontent.com/19840443/63224936-5d12f480-c1dc-11e9-8baf-d2f630df6d96.png)<br>
Podczas rozwijania struktur CFG natrafić możemy na niejednoznaczność *(ambiguity)* "priorytetu". Dla przykładu, w prostym działaniu matematycznym ```2 * 3 + 4```, zauważmy, że dla tego przykładu rozwijając strukturę z prawej strony otrzymamy zły wynik, najpierw wykona się operacja dodawania a dopiero później mnożenia. Nie musimy się jednak zbytnio skupiać na tego typu problemach, ponieważ zwykłe użycie nawiasów podczas projektowania CFG wystarczy. Innym sposobem na pozbycie się problemu jest zadeklarowanie najpierw operacji mnożenia i dzielenia, a później dodawania i odejmowania.

#### Drzewo składniowe
Podczas parsowania utworzone zostaje drzewo składniowe, tzw. Concrete Syntax Tree *(CST)* to drzewo odzwierciedlające nasz kod źródłowy w postaci tokenów, natomiast bardziej przydatnym drzewem jest Abstract Syntax Tree *(AST)*, które zachowuje najbardziej przydatne informacje w celu dalszej obróbki również w postaci tokenów, różnica pomiędzy CST i AST jest taka, że AST będzie starał się na bieżąco wyliczać wartości.
```
Kod źrodłowy: int b = ((5 * 10) + (5 - 10)) / a * 1;
CST: b = ((5 * 10) + (5 - 10)) / a * 1 -> drzewo dokładnie odzwierciedlające nasz kod źródłowy
AST: b = 45 / a -> drzewo z wyliczonymi wartościami oraz pominiętymi nic nie znaczącymi akcjami (np. mnożenie razy 1)
```
#### Analiza zstępująca
Analiza zstępująca zaczyna się niemalże bez informacji tj. pierwszy symbol od którego zaczynamy, pasuje do każdego zestawu symboli. Skąd zatem wiemy, który zestaw symboli to ten którego szukamy? Nie wiemy... Jedynie co możemy zrobić to zgadywać, jeśli natomiast się pomylimy to wracamy po węzłach. Skoro musimy zgadywać to w jaki sposób? Spróbuję teraz omówić dwa podstawowe algorytmy z "nawrotami" *(backtracking)* (nie śmiejcie się, tak podaje [wikipedia](https://pl.wikipedia.org/wiki/Algorytm_z_nawrotami)). Zacznijmy od tego żeby potraktować nasze wejście składające się z tokenów jako zdanie składające z symboli terminalnych i nieterminalnych oraz proces parsowania jako przeszukiwanie po węzłach. Węzłem będzie **każdy dowolny zestaw symboli**, natomiast przejście z węzła na węzeł będzie możliwe wtedy gdy napotkamy symbol, wchodzi w skład obecnego zestawu symboli, a tak po ludzku, chodzi o taką sytuację:
![example](https://user-images.githubusercontent.com/19840443/63461080-29dc9980-c458-11e9-9cab-310533588d9b.png)

#### Przeszukiwanie wszerz
Zacznijmy od pierwszego, raczej rzadziej używanego algorytmu *Breadth-First Search* lub *BFS*. Algorytm polega na przeszukiwaniu wszerz zaczynając od symbolu najbardziej na lewo lub prawo. Standardowe przejście po grafie wygląda następująco:
![bfs](https://user-images.githubusercontent.com/19840443/63718211-57f01e00-c84a-11e9-9ddf-100efc4d344f.png)<br>
Od razu wspomnę, że próba szukania od symbolu najbardziej na prawo jest raczej **błędem**. Dlaczego? Zaczynając od symbolu najbardziej na prawo rozwijamy niepotrzebnie symbol terminalny. Również błędem jest rozwijanie każdego napotkanego symbolu przy rozwijaniu zaczynając od symbolu najbardziej na lewo (poniższa animacja ilustruje powyższy obrazek):
![bfs](https://user-images.githubusercontent.com/19840443/63718697-6b4fb900-c84b-11e9-9633-e47b93ee23ed.gif)<br>
Jak można łatwo zauważyć szukanie klucza może potrwać naprawdę długo, dodatkowo zajmujme to mnóstwo pamięci. Najbardziej nieprzydatną rzeczą okazuję się być rozwijanie symbolu terminalnego, który potencjalnie zawiera inne symbole terminalne z niepasującym kluczem. Co można by było ulepszyć?


#### Przeszukiwanie wgłąb
Drugim algorytmem jest *Deep-First Search* lub *DFS*

#### LL(1)


#### Bison
Poniżej znajduje się lista świetnych tutoriali odnośnie generatorów Flex i Bison:<br>
[Flex and Bison - Aquamentus](https://aquamentus.com/flex_bison.html)<br>
[Flex and Bison in C++ - Jonathan Beard](http://www.jonathanbeard.io/tutorials/FlexBisonC++)<br>
[Using Flex and Bison - Mactech](http://preserve.mactech.com/articles/mactech/Vol.16/16.07/UsingFlexandBison/index.html)

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
