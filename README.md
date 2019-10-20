## Wstęp
Cześć, na wstępie chciałbym aby całą pracę włożoną w to repozytorium potraktować bardziej jako obszerny artykuł. Zebrane tutaj informacje są krótkim podsumowaniem zdobytej wiedzy na temat procesu kompilacji. Przykłady omawiane są w języku C-podobnym  oraz oczywiście w języku polskim ;). Możliwe, że artykuł nie pokrył wszystkich tematów związanych z kompilacją, jednak chciabym zaznaczyć iż dopiąłem wszelkich starań by tak było. Artykuł będzie co jakiś czas ulepszany, dodam, iż chętnie przyjmę jakiekolwiek sugestie drogą mailowa.<br>

## Spis treści
0. [Tworzenie kodu źrodłowego](https://github.com/devmichalek/Biblioteki-Dynamiczne/blob/master/README.md#tworzenie-kodu-%C5%BAr%C3%B3d%C5%82owego)
1. [Kompilacja](https://github.com/devmichalek/Biblioteki-Dynamiczne/blob/master/README.md#kompilacja)
- 1.0. [Preprocessing](https://github.com/devmichalek/Biblioteki-Dynamiczne/blob/master/README.md#preprocessing)
- 1.1. [Analiza leksykalna - skanowanie](https://github.com/devmichalek/Biblioteki-Dynamiczne/blob/master/README.md#analiza-leksykalna---skanowanie)
  - 1.1.0 [Jak działa lekser?](https://github.com/devmichalek/Biblioteki-Dynamiczne/blob/master/README.md#jak-dzia%C5%82a-lekser)
  - 1.1.1 [Wyrażenia regularne](https://github.com/devmichalek/Biblioteki-Dynamiczne/blob/master/README.md#wyra%C5%BCenia-regularne)
  - 1.1.2 [NFA - Niedeterministyczny Automat Skończony](https://github.com/devmichalek/Biblioteki-Dynamiczne/blob/master/README.md#nfa---niedeterministyczny-automat-sko%C5%84czony)
  - 1.1.3 [Maximal Munch](https://github.com/devmichalek/Biblioteki-Dynamiczne/blob/master/README.md#maximal-munch)
  - 1.1.4 [DFA - Deterministyczny Automat Skończony](https://github.com/devmichalek/Biblioteki-Dynamiczne/blob/master/README.md#dfa---deterministyczny-automat-sko%C5%84czony)
  - 1.1.5 [Flex](https://github.com/devmichalek/Biblioteki-Dynamiczne/blob/master/README.md#flex)
  - 1.1.6 [Identyfikatory](https://github.com/devmichalek/Biblioteki-Dynamiczne/blob/master/README.md#identyfikatory)
- 1.2. [Analiza składniowa - parsowanie](https://github.com/devmichalek/Biblioteki-Dynamiczne/blob/master/README.md#analiza-sk%C5%82adniowa)
  - 1.2.0 [Jak działa parser?](https://github.com/devmichalek/Biblioteki-Dynamiczne/blob/master/README.md#jak-dzia%C5%82a-parser)
  - 1.2.1 [Gramatyka bezkontekstowa](https://github.com/devmichalek/Biblioteki-Dynamiczne/blob/master/README.md#gramatyka-bezkontekstowa)
  - 1.2.2 [Niejednoznaczność](https://github.com/devmichalek/Kompilacja/blob/master/README.md#niejednoznaczno%C5%9B%C4%87)
    - 1.2.2.0 [Niejednoznaczność Priorytetu](https://github.com/devmichalek/Kompilacja/blob/master/README.md#niejednoznaczno%C5%9B%C4%87-priorytetu)
    - 1.2.2.1 [Wiszący If-Else](https://github.com/devmichalek/Kompilacja/blob/master/README.md#wisz%C4%85cy-if-else)
    - 1.2.2.2 [Gramatyka Niejednoznaczna](https://github.com/devmichalek/Kompilacja/blob/master/README.md#gramatyka-niejednoznaczna)
  - 1.2.3 [Drzewo składniowe](https://github.com/devmichalek/Biblioteki-Dynamiczne/blob/master/README.md#drzewo-sk%C5%82adniowe)
  - 1.2.4 [Hierarchia Chomsky’ego](https://github.com/devmichalek/Kompilacja/blob/master/README.md#hierarchia-chomskyego)
  - 1.2.5 [Analiza zstępująca](https://github.com/devmichalek/Biblioteki-Dynamiczne#analiza-zst%C4%99puj%C4%85ca)
    - 1.2.5.0 [Przeszukiwanie wszerz](https://github.com/devmichalek/Biblioteki-Dynamiczne#przeszukiwanie-wszerz)
    - 1.2.5.1 [Przeszukiwanie wgłąb](https://github.com/devmichalek/Biblioteki-Dynamiczne#przeszukiwanie-wg%C5%82%C4%85b)
    - 1.2.5.2 [LL(1)](https://github.com/devmichalek/Biblioteki-Dynamiczne#ll1)
    - 1.2.5.3 [LL(1) Parse Tables](https://github.com/devmichalek/Biblioteki-Dynamiczne#ll1-parse-tables)
    - 1.2.5.4 [FIRST, FOLLOW](https://github.com/devmichalek/Biblioteki-Dynamiczne#first-follow)
  - 1.2.6 [Analiza wstępująca](https://github.com/devmichalek/Biblioteki-Dynamiczne#analiza-wst%C4%99puj%C4%85ca)
    - 1.2.6.0 [Redukcje, Przesunięcia, Uchwyty](https://github.com/devmichalek/Kompilacja/blob/master/README.md#redukcje-przesuni%C4%99cia-uchwyty)
    - 1.2.6.1 [Gdzie są uchwyty?](https://github.com/devmichalek/Kompilacja/blob/master/README.md#gdzie-s%C4%85-uchwyty)
    - 1.2.6.2 [Jak szukamy uchwytów?](https://github.com/devmichalek/Kompilacja/blob/master/README.md#jak-szukamy-uchwyt%C3%B3w)
    - 1.2.6.2 [LR(0)](https://github.com/devmichalek/Kompilacja/blob/master/README.md#lr0)
    - 1.2.6.3 [LR(1)]()
  - 1.2.7 [Bison](https://github.com/devmichalek/Biblioteki-Dynamiczne/blob/master/README.md#bison)
- 1.3. [Analiza semantyczna](https://github.com/devmichalek/Biblioteki-Dynamiczne/blob/master/README.md#analiza-semantyczna)
- 1.4. [Generacja IR](https://github.com/devmichalek/Kompilacja/blob/master/README.md#generacja-ir)
- 1.5. [Optymalizaja IR](https://github.com/devmichalek/Kompilacja/blob/master/README.md#optymalizaja-ir)
- 1.6. [Generacja kodu](https://github.com/devmichalek/Kompilacja/blob/master/README.md#generacja-kodu)
- 1.7. [Optymalizacja](https://github.com/devmichalek/Kompilacja/blob/master/README.md#optymalizacja)
2. Linkowanie
3. Ładowanie programu
4. Uruchamianie programu

## Tworzenie kodu źródłowego
Pierwszym etapem w drodze do utworzenia programu jest napisanie kodu źródłowego. Na tym etapie nie dzieje się zbytnio dużo. Otwieramy ulubiony edytor tekstu lub dedykowane IDE. Warto wspomnieć, że podczas pracy nad danym projektem staramy trzymać się pewnej hierarchi. Każdą inną różniącą się funcjonalność trzymiemy w osobnych plikach, pliki o podobnej charakterystyce trzymamy w jednym projekcie, a ogół projektów trzymamy w rozwiązaniu *solution* (rekomendowana hierarchia plików w Visual Studio), natomiast produkt końcowy składać się będzie zwykle z różnych rozwiązań. Przykładem może być firma, która zatrudnia dwie grupy programistów, w której jedna zajmuje się rozwiązaniami nad GUI, a druga nad rozwiązaniami dotyczącymi mechaniki aplikacji, obie grupy pracują nad inną specyfikacją aplikacji (wspomniany przykład to jedynie uproszczenie całej sytuacji, w rzeczywistości jest to bardziej skomplikowane).

## Kompilacja
Zacznijmy od tego, że kod który napisaliśmy w danym języku kompilujemy nie tylko po to aby otrzymać plik z kodem maszynowym (tzw. plik obiektowy) ale również po to aby sprawdzić czy jest on poprawny gramatycznie (Czy brak w nim polskich znaków?), czy pomimo braków błędów językowych struktura naszego kodu jest dopuszczalna (Czy definicja funkcji zamyka się w klamrach?), czy pomimo poprawnej struktury kodu nie popełniliśmy błędu związanego z semantyką (Czy nie próbujemy przypisać dwóch kompletnie różnych obiektów do siebie?) oraz szereg innych rzeczy, które będą szczegółowo omawiane później. Sama znajomość języka programowania bez wiedzy jak nasz kod jest budowany nie wystarcza, dlatego postaram się szczegółowo omówić jak wygląda proces kompilacji krok po kroku.

## Preprocessing
W przypadku języka C w pierwszym etapie kompilacji specjalny program nazywany preprocessorem parsuje pliki źrodłowe w celu:
- Załączenia kodu z plików wskazanych dyrektywą *#include*,
- Konwersji makro definicji na stałe wskazane przez dyrektywę *#define*,
- Zamiany makro definicji na kod,
- Włączenia lub wyłączenia danej części kodu wskazanej dyrektywą *#if*, *#elif* i *#endif*.<br>

Warto wiedzieć, że istnieje możliwość stworzenia makro funkcji z nieznaną liczbą argumentów oraz [makra generyczne](https://mort.coffee/home/obscure-c-features/), które dodatkowo utrudniają pracę preprocesorowi. GCC umożliwia spojrzenie na to jak nasz kod został sparsowany przez preprocessor:
```bash
gcc -E <input>.c -o <output>.i
```

## Analiza leksykalna - skanowanie
W następnym etapie omówiona zostania analiza leksykalna, nazwa ta pochodzi od programu, który ją wykonuje czyli leksera inaczej nazywanego skanerem.
### Jak działa lekser?
Proces skanowania rozpoczyna się od momentu, w którym lekser na wejście dostaje gotowy plik podczas, którego tnie kod źródłowy na leksemy, leksemami nazywamy pasujący do danego wyrażenia ciąg znaków, w których skład wchodzą słowa kluczowe, liczby, znaki specjalne itd. Jak później można się przekonać, leksery ściśle współpracują z parserami. Mając dany leksem, skaner konwertuje go na istniejący token w przypadku gdy takowy istnieje. W zależności od języka i dla wygody tokeny są grupowane, przykładowo int, float i char będą należały do tej samej grupy tokenów skojarzonych z typami zmiennych. Żeby zobaczyć czym są tokeny oraz, że niektóre z nich posiadają atrybuty warto spojrzeć na poniższy kod:
```C
while (137 < i)
	++i;
```
W wielu źródłach słowo leksem używane jest jako zamiennik słowa token, natomiast starsze źródła podają, że leksem jest ciągiem znaków i nabiera on znaczenia w momencie gdy znaleziony został odpowiadający mu token. Początkowo ```while``` będzie leksemem składających się z pięciu znaków, jednak zostanie ono zamienione na token *T_While* (reprezentujące słowo kluczowe), który informuje nas o tym, że jest to deklaracja pętli. W powyższym kodzie liczba ```137``` jest leksemem składającym się z cyfr, natomiast dopasowana, jest tokenem reprezentującym liczbę całkowitą *T_IntConst* posiadającym atrybut , w której przechowana zostanie liczba. Poniższa animacja obrazuje to jeszcze lepiej. Poniższą animacje (jak i większość, które tu zobaczysz) stworzyłem na bazie [prezentacji](https://web.stanford.edu/class/archive/cs/cs143/cs143.1128/lectures/01/Slides01.pdf) o analizie leksykalnej wykładanej na Stanfordzie.
![Skanowanie](https://raw.githubusercontent.com/devmichalek/Kompilacja/master/assets/1.1.0_0.gif)
Znaki specjalne takie jak klamry i nawiasy, które wzbogacają gramatykę języka z reguły posiadają swój własny odpowiadający im token (zazwyczaj słowa kluczowe również posiadają swój własny token), dodatkowo tokeny należace do tej samej grupy są po prostu grupowane (tak jak wcześniej wspomniane tokeny typów zmiennych), natomiast nic nie znaczące spacje, tabulacje lub komentarze są pomijane. Inny przykład tym razem z [wikipedii](https://pl.wikipedia.org/wiki/Analiza_leksykalna) świetnie obrazuję jak lekser przeanalizował kod źródłowy:<br>
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

Omawiane wcześniej przykłady są jednymi z najprostszych do rozwiązania dla skanera. Weźmy pod uwagę fakt, że w C++ jest możliwość tworzenia szablonów, dla poniższego przykładu patrząc z perspektywy leksera wyodrębnienie zagnieżdżonego std::vector nie jest prostym zadaniem:
```C++
std::vector<std::vector<int>> vec
```
Również gdy ktoś użył słów kluczowych jako nazwy zmiennej w języku Pascal:
```pascal
if then then then = else; else else = if
```
W powyższych przykładach nie należy zastanawiać się nad tym jak skaner odróżnił słowa kluczowe od identyfikatorów bo po prostu tego nie zrobił, na tym etapie skaner ma za zadanie przeskanować tekst i zwrócić otrzymane w wyniku tej operacji tokeny. Cała więc trudność tkwi w napisaniu odpowiednich reguł opisujących leksem, reguły te przyjeły nazwę wyrażeń regularnych. Wyrażenia regularne mają za zadanie znaleźć szukane przez nas ciągi znaków przez, które lekser będzie w stanie zwrócić odpowiedni token. Przykładem może być leksem reprezentujący liczbę, tworząc wyrażenie regularne zakładamy, że będzię to ciąg znaków 0-9, cytując *"...specyfikacja języka programowania obejmuje szereg reguł które definiują składnię leksykalną. Składnia leksykalna jest zazwyczaj opisana za pomocą wyrażeń regularnych. Definiują one zbiór możliwych sekwencji znakowych które tworzą pojedyncze tokeny. Lekser przetwarza oddzielone znakami białymi ciągi znaków i dla każdego ciągu znaków podejmuje akcję zazwyczaj produkując token, bądź ogłasza błąd analizy leksykalnej...."*. Wiedząc to wszystko nasuwa się pytanie: jak napisać swój własny skaner? Zacznijmy od tego, że nikt nie piszę własnych lekserów od zera, są ku temu dwa powody: ilość czasu jaką trzeba było by poświęcić na napisanie kodu od zera oraz błędy, które bardzo prawdopodobnie pojawią się podczas pisania na piechotę. Obecnie do pisania lekserów używa się generatarów. Do stworzenia skanera posłużyć się można programem Flex, który wygeneruje nam nasz własny kod skanera (kod skanera w języku C, ponieważ brakuje nam jeszcze kodu parsera gdzie całość zostanie skompilowana jako jeden program). [Flex](https://en.wikipedia.org/wiki/Flex_(lexical_analyser_generator)) *(Fast Lex)* to następca oraz nowsza alternatywa wobec generatora [Lex](https://en.wikipedia.org/wiki/Lex_(software)). Jednak zanim napiszesz swój własny skaner, warto zobaczyć jak [wyrażenia regularne](https://en.wikipedia.org/wiki/Regular_expression) zostały zaimplementowane.

### Wyrażenia regularne
Wyrażenia regularne *(regular expression)* używane są wszędzie tam gdzie w ciągu znaków zależy nam na wyszukaniu słowa klucza. Nic więc dziwnego, że używane są w różnego typu programach skanujących tekst. Składnią regexów są tzw. metaznaki w skład, którego wchodzi alfabet, cyfry lub znaki specjalne. Wyrażenia regularne najlepiej pokazać na przykładach:

| Wyrażenie regularne | Opis                                                      | Pasujący ciąg znaków |
| ------------------- | --------------------------------------------------------- | -------------------- |
| while               | Brak znaków specjalnych                                   | while                |
| [abc]               | Jakakolwiek litera z zbioru {a, b, c}                     | a, b, c              |
| [a-z]               | Jedna mała litera od a do z                               | a, b, c, ... x, y, z |
| a\*b                | 0 lub więcej wystąpień a po lewej stronie oraz litera b   | b, ab, aab, aaab...  |
| (a\|c)b             | a lub c oraz b                                            | ab, ac               |
| (+\|-)?[0-9]+       | Liczba bez znaku lub z znakiem +/-                        | 4, -18, +389, 258963 |

Warto wspomnieć, że wyrażenia regularne posiadają swoje ograniczenia, jednym z głównym problemów jest tzw. *pumping lemma*. Przypuśćmy, że szukamy takiego słowa, które z lewej i prawej strony ma tyle samo wystąpień znaku **a**, natomiast pośrodku oczekujemy znaku **b**. Pasujące słowa to np. **aabaa**, **aba**, **aaaabaaaa**. Niestety, ale tego typu reguły nie jesteśmy w stanie uzyskać za pomocą regexów. Jedynie co moglibyśmy zrobić to wyszukać znaną nam liczbę wystąpień po lewej i prawej stronie lub nieznaną liczbę n wystąpień po lewej stronie i m wystąpień po prawej stronie. Wspomniany problem nie zalicza się do "składni gramatyki bezkontekstowej" (o tym już za chwilę). Wyrażenia regularne mogą zostać zaimplementowane za pomocą automatów skończonych *(finite automata)*, w której wyróżniamy dwa główne NFA (Nondeterministic Finite Automata) i DFA (Deterministic Finite Automata). Automaty najlepiej wytłumaczyć obrazując ich działanie. Na poniższej animacji okrąg oznacza stan *(state*), w którym się znajdujemy, natomiast strzałka *(transition)* oznacza przejście w inny stan gdy zajdzie podany warunek, ostatni znów podwójny okrąg wskazuje na stan akceptacji *(accepting state)*, automat akceptuje stringa jesli znajduje się w stanie akceptacji (na poniższych trzech animacjach przedstawiony jest NFA).<br>
![Poprawny](https://raw.githubusercontent.com/devmichalek/Kompilacja/master/assets/1.1.1_0.gif)<br>
Poniżej animacje przejść w automatach, które nie zaakceptowały wejścia, w pierwszej brakuje kolejnego przejścia na końcu stanu akceptacji, natomiast w drugiej brakuje warunku przejścia dzięki któremu przeszlibyśmy do stanu akceptacji.<br>
![Niepoprawny](https://raw.githubusercontent.com/devmichalek/Kompilacja/master/assets/1.1.1_1.gif)<br>
![Niepoprawny](https://raw.githubusercontent.com/devmichalek/Kompilacja/master/assets/1.1.1_2.gif)<br>
Cytując *"Celem automatu jest rozstrzygnięcie w skończonej ilości kroków, czy dowolne słowo należy, czy też nie należy do języka badanego przez automat. Teoria automatów jest efektem badań zapoczątkowanych przez A. Turinga. Automat reaguje na określone sygnały, zmieniając swój stan. Jeśli ustalimy, że sygnały będą reprezentowane przez poszczególne litery alfabetu, zdefiniujemy stan początkowy automatu oraz dopuszczalne stany końcowe, to automat może testować dowolne słowo, startując ze stanu początkowego. Słowo zostanie rozpoznane przez automat i uznane za poprawne, jeżeli rezultatem działania będzie założony stan końcowy."*

### NFA - Niedeterministyczny Automat Skończony
Na początku kilka faktów:
- każde wyrażenie regularne jesteśmy w stanie zamienić na NFA
- mając stringa o długości **m** jesteśmy w stanie znaleźć słowo klucz w czasie **O(mn^2)**

Dlaczego NFA i DFA są tak mocno związane z wyrażeniami regularnymi? Głównie ze względu na prostotę z jaką możemy ukazać problem za pomocą stanów i przejść po grafie. DFA to automat, który szczegółowo opisuję każdy węzeł oraz jego **jedno** przejście (rozgałęzienie). NFA natomiast traktowane jest jako "symboliczna" i "skrócona" wersja DFA. W automacie niedeterministycznym, dla danego stanu, nie jest wymaganym by wychodziło dokładnie jedno przejście. Takich przejść może nie być, może być jedno, lub może być ich wiele. Podobnie, automat niedeterministyczny może mieć wiele stanów początkowych. NFA będąc w danym stanie z, którego nie ma juz przejścia zmuszone jest wrócić się po węzłach do tyłu tak aby znaleźć odpowiednią tranzycję do wykonania, stąd szybkość agorytmów bazujących na NFA jest stosunkowo wolniejsza. Wyrażenia regularne zaimplementowane za pomocą DFA wykonują się szybciej od tych zaimplementowanych w sposób NFA, jednakże minusem jest większa ilość pamięci, której potrzebują. Swoją drogą, skoro mamy kilka przejść z jednego węzła tak naprawdę nie wiemy, które się wykona stąd właśnie nazwa "niedeterminisyczny" automat skończony i stąd potrzeba wracania się po stanach *(backtracking)* w przypadku podjęcia złej decyzji.
Dla ciekawych tematem polecam zobaczyć czym jest [Automat Büchiego](https://pl.wikipedia.org/wiki/Automat_B%C3%BCchiego)

### Maximal Munch
Ten nagłówek musiał się pojawić, wyrażenia regularne to nie tylko szukanie klucza w stringu, ale szukanie **najdłuższego pasującego** klucza. "Maximal Munch" to inaczej "The Longest Match", najdłuższe pasujące wyrażenie. Mając wiele wyrażeń regularnych w jaki sposób zaimplementować "Maximal Munch"? Trzy niezbędne czynności to:
- zamiana wyrażeń regularnych na NFA
- uruchomienie **wszystkich NFA równocześnie**, zachowując ostatni wynik *(last match)*
- gdy wszystkie automaty są w stanie, w którym nie ma przejść to zwróć *last match* i zacznij szukać ponownie w miejscu, w którym skończyłeś

Przypuśćmy, że mamy sytuacje, w której analizujemy ciąg znaków ```DOUBDOUBLE``` oraz, że mamy do dyspozycji trzy tokeny ```T_Do```, ```T_Double```, ```T_Mystery```, które reprezentują kolejno: słowo kluczowe ```do```, słowo kluczowe ```double```, string zawierający jedną literę małą lub dużą. Token o najwyższym priorytecie umieszczony jest najwyżej. Animacja przedstawiająca nasz problem:
![Maximal Munch](https://raw.githubusercontent.com/devmichalek/Kompilacja/master/assets/1.1.3_0.gif)<br>
Powyższa animacja być może i wygląda na trzy automaty deterministyczne uruchomione jednocześnie jednak w rzeczywistości ukazuje ona jak NFA działa "wewnątrz", starajmy się raczej wyobrazić, że nasz punkt początkowy jest jeden od którego wychodzą trzy przejścia do odpowiednich węzłów (jeden węzeł "start").

### DFA - Deterministyczny Automat Skończony
Kilka faktów:
- każde wyrażenie regularne jesteśmy w stanie zamienić na DFA (najpierw na NFA a później na DFA)
- mając stringa o długości **m** jesteśmy w stanie znależć klucz w czasie **O(m)**

Główna różnica między NFA to taka, że w tym przypadku posiadamy jeden stan początkowy, jedno przejście z każdego stanu oraz jeden stan akceptacji. Siła automatów niedeterministycznych jest dokładnie taka sama, jak deterministycznych, dla każdego niedeterministycznego automatu skończonego istnieje równoważny mu automat deterministyczny. Wyrażenia regularne zaimplementowane na wzór DFA są szybkie jednak mocno obciążają pamięć (często używana jest hybryda). Jeśli już jesteśmy przy automatach skończonych to warto wspomnieć, że zwykle automaty te podczas skanowania tekstu posiadają tzw. *lookahead character* tj. znak aktualnie rozpatrywany oraz jeden znak wprzód (w wielu przypadkach znacznie ułatwia to skanowanie i podejmowanie decyzji wykonywanych przez automat).
```C
/* Poniżej przestawiam DFA
   do rozróżniania komentarzy
   wieloliniowych w C.
*/
```
![DFA](https://raw.githubusercontent.com/devmichalek/Kompilacja/master/assets/1.1.4_0.png)<br>
Dla zainteresowanych tematem polecam sprawdzić jak działają automaty [Mealy’ego](https://pl.wikipedia.org/wiki/Automat_Mealy%E2%80%99ego), [Moore’a](https://pl.wikipedia.org/wiki/Automat_Moore%E2%80%99a), legendarną [maszynę Turinga](https://pl.wikipedia.org/wiki/Maszyna_Turinga) oraz sposób w jaki tworzone są automaty z wyrażen regularnych - [Thompson's construction](https://en.wikipedia.org/wiki/Thompson%27s_construction).

### Flex
Wreszcie doszliśmy do momentu, w którym sami możemy się sprawdzić jako projektanci leksera. Teraz w zależności od tego co chcielibyśmy osiągnąć skaner dla każdego przypadku będzie się diametralnie różnił. Wszędzie tam gdzie wyrażenia regularne nie wystarczają jesteśmy w stanie zaimplementować lekser. Flex to program, które na wejście przyjmuję plik składający się z trzech sekcji, z której pierwsza to sekcja definicji np. stałych, ```#include``` itd., druga to sekcja zasad, która składa się z listy zdefiniowanych wyrażeń regularnych oraz z akcji, która zostanie wykonana gdy dane wyrażenie regularne zostanie spełnione (np. wywołanie funkcji i zwrócenie tokenu), oraz z sekcji trzeciej składającego się z "kodu użytkownika". Tak jak wspomniałem na początku nagłówka o analizie leksykalnej, lekser ściśle współpracuje z parserem, dlatego też Flex współpracuje z generatorem parserów tj. Bison'em. Czym są parsery? Jak działają? Dlaczego sam lekser nie wystarcza? To wszystko znajdziecie poniżej. [Tutaj](https://github.com/devmichalek/Biblioteki-Dynamiczne/blob/master/README.md#bison) umieściłem nagłówek o Bison'ie oraz liste bardzo dobrych tutoriali dla zaczynających przygodę z Flex & Bison, których nie ma sensu duplikować. [Tutaj](http://people.cs.aau.dk/~marius/sw/flex/Flex-Regular-Expressions.html) zostawiam linka dla tych, którzy chcieliby dowiedzieć się jak pisać wyrażenia regularne tak aby były one poprawnie interpretowane.

### Identyfikatory
Na początku wspomniałem, że tokeny słów kluczowych posiadają swoje własne zdefiniowane regexy, okazuje się, że nie jest to reguła, którą musi spełniać każdy kompilator. Warto zaznaczyć, że zarezerwowane słowa muszą posiadać większy priorytet od identyfikatorów tzn. ich regexy powinny być zdefiniowane przed regexem identyfikatora przez co skaner rozpatruje najpierw słowa kluczowe. Istnieje również inne zabezpieczenie przed tym gdy uzytkownik będzie chciał użyć słowa kluczowego jako identyfikator. Należy pamiętać, że każda definicja wyrażenia regularnego kosztuje nas sporo pamięci w przypadku użycia generatora Flex, który tworzy tabele do ich wyszukiwania. Projektanci skanera wyróżniają druga metodę polegającą na zdefiniowanu tabeli składającej się z stringa i wskaźnika na funkcję. Tabela ma za zadanie reprentować parę **słowo zarezerwowane** + **akcja**. W przypadku gdy lekser dopasuje regex identyfikatora następuję wyszukiwanie binarne w tablicy słów kluczowych i zostaje podjęta odpowiednia przypisana mu akcja. W przypadku gdy słowo nie zostanie znalezione zwracany jest token ```T_Identity```. W ten właśnie sposób znacznie oszczędzamy na pamięci kosztem nieodczuwalnego spowolnienia kompilatora.

## Analiza składniowa
W następnym etapie omówiona zostanie analiza składniowa podczas, której parser manipuluje otrzymanymi przez leksera tokenami.

### Jak działa parser?
Jeśli kompilator znalazł się na etapie analizy składniowej oznacza to, że nasz kod jest leksykalnie poprawny, tj. udało się stworzyć tokeny z każdego znaku (lub ciągu znaków) znalezionego w pliku. Właściwie analize składniową ciężko nazwać następnym etapem ponieważ lekser i parser pracują nieustannie rownocześnie obok siebie (przykładowo parser może chcieć pobrać kolejny token funkcją skanera o nazwie GetToken() gdy zajdzie taka potrzeba), jednak dla uproszczenia napisałem, że jest to kolejny etap, dlatego na tym etapie zakładamy, że tokeny zostały poprawnie przekierowane do pliku z którego teraz nasz parser zacznie czytać. Przykładowy kod poprawny leksykalnie:
```C
while (ip < z)
	++ip;
```
Teraz obrazek ilustrujący na jakim etapie się znajdujemy, nasz kod został pocięty na leksemy z których utworzone zostały następujące tokeny:
![Poprawne tokeny](https://github.com/devmichalek/Kompilacja/blob/master/assets/1.2.0_0.png?raw=true)<br>
Kolejny przykład kodu poprawnego leksykalnie:
```C
do[for] = new 0;
```
Obrazek ilustrujący utworzone tokeny:
![Niepoprawne tokeny](https://github.com/devmichalek/Kompilacja/blob/master/assets/1.2.0_1.png?raw=true)
Jak widać coś poszło nie tak, nasz kod faktycznie jest poprawny leksykalnie, ale niektóre z tych tokenów to słowa kluczowe, przykładem jest ```for```, które zostało użyte jako identyfikator (gdzieś wcześniej) prawdopodobnie jakiejś liczby całkowitej, lub ```do``` użyte prawdopodobnie jako nazwa tablicy.<br>

Podczas analizy składniowej parser z wcześniej przygotowanych przez leksera tokenów stara się dopasować istniejącą strukturę *(pattern)*, w przypadku gdy jej nie znajdzie zwróci błąd analizy składniowej. Podczas skanowania naszym alfabetem były znaki ASCII lub Unicode czyli po prostu kod źródłowy, podczas parsowania naszym alfabetem jest zestaw utworzonych tokenów. Podczas skanowania używaliśmy wyrażeń regularnych do opisu leksemów, które później skonwertowane zostały na odpowiadający im token. Niestety, ale w przypadku parsowania wyrażenia regularne okazują się zbyt słabe do opisania struktury gramatycznej, w tym przypadku konieczne jest użycie innego narzędzia głównie ze względu na fakt, iż opis struktury gramatycznej może być bardzo złożoną strukturą **rekurencyjną**.

### Gramatyka bezkontekstowa
**Context-Free Grammar** lub po prostu CFG to zestaw zasad za pomocą, których opisywane są struktury rozumiane przez parsera. Przypuśćmy, że chcemy opisać operacje arytmetyczne takie jak dodawanie, odejmowanie, mnożenie i dzielenie. Po lewej przedstawiono zasady dla operacji arytmetycznych, natomiast po prawej utworzona struktura z działania
```
10 * (1 + 2)
```
![CFG](https://github.com/devmichalek/Kompilacja/blob/master/assets/1.2.1_0.png?raw=true)
- ![#ff0000](https://placehold.it/15/ff0000/000000?text=+) - ```E``` i ```Op```, symbole nieterminalne, rozpoczynajace się dużymi literami np. A, B, C. Znak nieterminalny jest **symboliczny** tzn. za pomocą niego opisujemy inne symbole do, których może się on rozwinąć (na przykładach poniżej wszystko się wyjaśni)
- ![#0000ff](https://placehold.it/15/0000ff/000000?text=+) - ```int```, ```()```, ```+```, ```-```, ```*```, ```/```, symbole terminalne, rozpoczynajace się małymi literami np. e, f, g. Znak terminalny jest **dosłowny** tzn. podczas parsowania nic nie rozwijamy i szukamy dokładnie tego co jest reprezentowane przez ten znak (będą to uprzednio utworzone przez skanera tokeny).
- ![#737373](https://placehold.it/15/737373/000000?text=+) - symbol dowolny (symbol terminalny lub symbol nieterminalny), zwykle oznaczany małymi literami greckimi np. α, γ. Ktoś mógłby zapytać po co wprowadzać kolejną reprezentację skoro mamy już symbol nieterminalny, który potencjalnie może składać się z innych symboli? Jest to bowiem przydatne przy opisie gramatyki (o tym później). Niektóre małe litery alfabetu greckiego zarezerwowane są dla specjanych przypadkow jak np. epsilon ε oznaczający pustego stringa.

Gramatykę bezkontekstową opisujemy za pomocą [notacji BNF](https://en.wikipedia.org/wiki/Backus%E2%80%93Naur_form), w której istnieją dwa sposoby rozwijania symboli przez drzewo parsujące *(parse tree)* nazywane derywacją *(derivation)*. Poniżej przedstawiono: *Leftmost Derivation* (derywacja lewostronna) tj. w każdym kolejnym kroku rozwijany jest pierwszy z lewej symbol nieterminalny, *Rightmost derivation*  (derywacja prawostronna) tj. w każdym kolejnym kroku rozwijany jest pierwszy z prawej symbol nieterminalny. Proszę zwróćcie uwagę na różnice w jakiej opisywana jest zasada i derywacja. [Na pierwszej ilustracji dotyczącej CFG](https://github.com/devmichalek/Kompilacja/raw/master/assets/1.2.1_0.png?raw=true) strzałka po lewej stronie jest zdecydowanie chudsza niż ta po prawej są to szczegóły jednak wciąż bardzo istotne. Na ich podstawie wiemy kiedy autor miał na myśli ukazanie derywacji (konstrukcji), a kiedy ukazanie opisu zasady (definicji). Poniżej grubsza strzałka opisująca proces derywacji.
![Derywacja](https://github.com/devmichalek/Kompilacja/blob/master/assets/1.2.1_2.png?raw=true)<br>

![CFG](https://github.com/devmichalek/Kompilacja/blob/master/assets/1.2.1_1.png?raw=true)<br>
Warto wiedzieć, że sładnia wyrażeń regularnych nie może zostać użyta do opisu gramatyki bezkontekstowej.
Przykładowo Kleene Closure ```*``` w wyrażeniach regularnych oznacza 0 lub więcej wystąpień, dla ```a*b```, dopasowane zostałyby słowa ```ab```, ```b```, ```aaaaab```, taka funkcjonalność nie jest jednak wspierana przez CFG. Głównym powodem jest brak konieczności istnienia takiego operatora (notacja BNF zapewnia nam możliwość użycia rekurencji).

### Niejednoznaczność
Podczas rozwijania struktur CFG natrafić możemy na niejednoznaczność *(ambiguity)*. Tak jak w przypadku niedeterministycznego automatu skończonego nie wiemy, która czynność zostanie wykonana tutaj nie wiemy jak nasza struktura drzewa zostanie zbudowana tj. może zostać zbudowana na kilka rożnych sposobów (w ten sposób mogą powstać dwa kompletnie różne drzewa). Sytuacja jest na tyle nieciekawa, że nie ma konkretnego algorytmu na rozwiązanie tego typu problemu. W tym przypadku mamy dwie opcję, pierwsza z nich to zmiana gramatyki języka, druga to określenie, która zasada gramatyczna ma **pierwszeństwo**.

#### Niejednoznaczność Priorytetu
Rozważmy proste działanie matematyczne ```34 - 3 * 42```, oraz zdefiniowane przez nas zasady gramatyki:<br>
![Zasady](https://github.com/devmichalek/Kompilacja/blob/master/assets/1.2.2.0_0.png?raw=true)<br>
Rozwijając strukturę z lewej strony otrzymamy zły wynik oraz następujące drzewo:<br>
![Drzewo](https://github.com/devmichalek/Kompilacja/blob/master/assets/1.2.2.0_1.png?raw=true)<br>
Rozwijając strukturę z prawej strony otrzymamy:<br>
![Drzewo](https://github.com/devmichalek/Kompilacja/blob/master/assets/1.2.2.0_2.png?raw=true)<br>
Przy derywacji lewostronnej najpierw wykonana zostanie operacja odejmowana a dopiero później mnożenia (co jest oczywiście błędem). Zmieniając gramatykę języka na taką, w której użycie nawiasów jest obowiązakowe wykluczamy tym samym błędne rozgałęzienie drzewa:<br>
![Zasady](https://github.com/devmichalek/Kompilacja/blob/master/assets/1.2.2.0_3.png?raw=true)<br>
Często jednak zmiana gramatyki nie wchodzi po prostu w grę. Innym sposobem na pozbycie się problemu jest zadeklarowanie najpierw operacji mnożenia i dzielenia (to one zostaną najpierw dopasowane), a później dodawania i odejmowania. Dodam, iż zasada gramatyczna często nazywana jest **produkcją** ponieważ **produkuje** ona stringa za pomocą derywacji.<br>
Rozważmy produkcję ```A -> (A)``` gdzie ```A``` to symbol nieterminalny, ```(``` i ```)``` to symbole nieterminalne. Taki rodzaj produkcji jest **niepoprawny**, gramatyka ta nie generuje żadnego stringa. Dzieje się ponieważ zasada gramatyczna definiuje strukturę rekurencyjną składającą się wyłącznie z symbolu, który występuje rekurencyjnie tj. w produkcji brakuje innego symbolu nieterminalnego, który często nazywany jest jako *base case*. Produkcja ta nie posiada symbolu na którym może bazować przez co każda derywacja narażona jest na nieskończoną pętlę.

#### Wiszący If-Else
Spójrzcie proszę na poniższą gramatykę:<br>
![Zasady](https://github.com/devmichalek/Kompilacja/blob/master/assets/1.2.2.1_0.png?raw=true)<br>
Z wyrażenia ```if (1) if (0) other else other``` (przyjmujemy, że *other* to część kodu wykonywana w ifach) otrzymujemy następujące drzewa:<br>
![Drzewo](https://github.com/devmichalek/Kompilacja/blob/master/assets/1.2.2.1_1.png?raw=true)
![Drzewo](https://github.com/devmichalek/Kompilacja/blob/master/assets/1.2.2.1_2.png?raw=true)<br>
Który z nich jest poprawny zależy od sposobu interpretacji przez nas samych:
```
if (1)				if (1)
	if (0) {}	vs.		if (0) {}
	else {}			else {}
```
Wspomniany problem to tzw. *dangling else problem*, polegający na trudności w rozróżnieniu do którego wyrażenia *if* dany *else* należy.

#### Gramatyka Niejednoznaczna
Gramatyka może być niejednoznaczna mimo to wciąż tworzyć unikalne struktury drzewa tzn. taka, w której algorytm parsujący dla derywacji lewostronnej i prawostronnej tworzy różne struktury drzewa jednak dające ten sam rezultat (w literaturze określane jako *inessential ambiguity*) **na etapie koncowym**. Taka gramatyka dotyczy w szczególności operacji, w których kolejność nie ma znaczenia. Najprostszymi przykładami będą operacje dodawania i mnożenia. W przypadku, w którym niejednoznaczność nie gra roli interesuje nas tak naprawdę efekt końcowy (wyliczona wartość z operacji mnożenia). Zauważmy, że mogą istnieć kompletnie dwie różne struktury drzewa ```(a * b) * c = a * (b * c)``` wciąż dające ten sam rezultat. Niemniej jednak algorytm parsujący powinien rozróżniać tego typu przypadki i stosować się tylko do jednej z wymienionych struktur uniemożliwiając tym samym powstawanie innych.

### Drzewo składniowe
Podczas parsowania utworzone zostaje drzewo składniowe, tzw. Concrete Syntax Tree *(CST)* to drzewo odzwierciedlające nasz kod źródłowy w postaci tokenów, bardziej przydatnym typem drzewa okazuje się być Abstract Syntax Tree *(AST)*, które zachowuje najbardziej przydatne informacje w celu dalszej obróbki również w postaci tokenów, różnica pomiędzy CST i AST jest taka, że AST stara się na bieżąco wyliczać wartości oraz usuwać niepotrzebne tokeny.
```
Kod źrodłowy: a[index] = 4 + 2;
CST: a [ index ] = 4 + 2; -> drzewo dokładnie odzwierciedlające nasz kod źródłowy
AST: a index = 6 -> drzewo z wyliczonymi wartościami, nawiasy zostały pominięte ponieważ token T_Index sam w sobie reprezentuje komórkę w tablicy
```

### Hierarchia Chomsky’ego
Zanim przejdziemy do algorytmów parsujących chciałbym dodać kilka słów na temat hierarchi Chomsky'ego. Naszą gramatykę języka reprezentujemy za pomocą notacji BNF lub EBNF (wersja rozszerzona, która dodatkowo zawiera np. wyrażenia opcjonalne), są to niezwykle przydatne i potężne narzędzia, jednak równie ważne jest by znać ich ograniczenia. Widzieliśmy przykłady, w których gramatyka może być niejednoznaczna produkując tym samym nieoczekiwaną strukturę drzewa. Tym razem postaram się przytoczyć inne przypadki, w których gramatyka może być zbyt rozbudowana lub niemożliwa wręcz do skonstruowania przez parsera. Warto zadać sobie pytanie, w jakich przypadkach użycie notacji BNF jest potrzebne, a w jakich należałoby użyć wyrażeń regularnych. Gramatyka bezkontekstowa jest w stanie wyrazić konkatenację, powtórzenie oraz wybór tak samo jak wyrażenia regularne. W ten sposób możliwe jest użycie regexów do konkatenacji np. łączenia cyfr w liczbę lub łączenia znaków w słowo (tak jak to dotychczas było robione, zostawiamy zadanie sklejania stringów dla regexów). Wykorzystując wyrażenia regularne ułatwiamy sobie zapis zasad gramatyki (prosty przykład poniżej):
```
REGEX:
digit = 0|1|2|3|4|5|6|7|8|9
number = digit digit*

BNF:
digit -> 0|1|2|3|4|5|6|7|8|9
number -> number digit | digit
```
Gramatyka, którą możemy wyrazić za pomocą wyrażen regularnych nazywana jest gramatyką regularną *(regular grammar)* (typem 3 według hierarchi Chomsky’ego). Co za tym idzie parsowanie takiej gramatyki może zostać przeprowadzone przez sam skaner gdyż użycie parsera nie było by konieczne. Mimo wszystko brak parsera to zły pomysł, rozsądniejszym byłaby implementacja parsera chociażby w celu odróżnienia procesu aktualnie przeprowadzanego przez kompilator. Sytuacja zmienia się wtedy gdy język posiada kontekst, który w wiekszości języków programowania występuje. Gramatyka bezkontekstowa, co ona tak naprawdę znaczy? Symbole nieterminalne reprezentują "zasady pozbawione kontekstu" (zaraz wyjaśnię), z których wynika, że token *T_Token* może zostać zamieniony na symbol nieterminalny wszędzie tam gdzie pasuje on do produkcji. Z drugiej strony moglibyśmy zdefiniować produkcję, która byłaby wrażliwa na kontekst *(context-sensitive grammar rule)*. Taki rodzaj produkcji jest dużo bardziej skomplikowany i nie możliwy obecnie do przeprowadzenia przez parser. Poniżej przykład gramatyki wrażliwej na kontekst:
```
{
	...
	int x = 5;
	...
	...x...
	...
}
```
Chodzi o sytuację, w której parser zdaję sobie sprawę z tego, że x to zmienna typu int zadeklarowana gdzieś wyżej. Gdybyśmy chcieli za pomocą notacji BNF stworzyć parser zależny od kontekstu musielibyśmy w jakiś sposób załączać np. nazwy zmiennej do zasad tak aby były one rozróżnialne. Po drugie musielibyśmy znać długość nazwy takiej zmiennej, która w wielu językach nie jest zdefiniowana, przez co potencjalnie liczba możliwych identyfikatorów jest nieskończona (nieskończona liczba zasad nie brzmi ciekawie). Nawet jeśli maksymalna długość identyfikatora wynosiłaby dwa, daje nam to setki nowych zasad gramatycznych. Zakres możliwości parsera jest ograniczony stąd też parser nie powinien przejmować się tego typu problemami. Ta część kompilacji przeprowadzana jest w analizie semantycznej, w której parser uprzednio zdefiniował, że istnieje identyfikator x (potencjalnie jakiejś zmiennej), zadeklarowana w tym danym miejscu. Gramatyka języka, która jest poza możliwościami parsera jednak wciąż możliwa do sprawdzenia przez kompilator nazywana jest semantyką statyczną języka *(static semantics of the language)* (w skład semantyki statycznej wchodzi statyczne sprawdzanie typów). Wspomniana gramatyka nazywana jest gramatyką kontekstową *(context grammar)* (typem 1 według hierarchi Chomsky’ego). Istnieje również typ 0 tzw. gramatyka rekurencyjnie przeliczalna (według polskiego tłumaczenia) *(unrestricted grammar)* równoważna automatowi Turinga, którą nie będę tutaj opisywał. Warto wspomnieć o osobie takiej jak Noam Chomsky, który opracował każdy z wymienionych typów gramatyki. Każda z tych gramatyk opisuję również jaki poziom mocy obliczeniowej potrzebny jest do jej opisu. Gramatyka regularna równoważna jest automatowi skończonemu, natomiast gramatyka bezkontekstowa równoważna jest automatowi ze stosem *(pushdown automaton)*.

### Analiza zstępująca
Analiza zstępująca zaczyna się niemalże bez informacji tj. pierwszy symbol od którego zaczynamy, pasuje do każdego zestawu symboli. Skąd zatem wiemy, który zestaw symboli to ten którego szukamy? Nie wiemy... Jedynie co możemy zrobić to zgadywać, jeśli natomiast się pomylimy to wracamy po węzłach. Skoro musimy zgadywać to w jaki sposób? Spróbuję teraz omówić dwa podstawowe algorytmy z "nawrotami" *(backtracking)* (nie śmiejcie się, tak podaje [wikipedia](https://pl.wikipedia.org/wiki/Algorytm_z_nawrotami)). Zacznijmy od potraktowania naszego wejścia składającego się z tokenów jako zdanie składające z symboli terminalnych i nieterminalnych oraz proces parsowania jako przeszukiwanie po węzłach. Węzłem będzie **symbol dowolny**, natomiast przejście z węzła na węzeł możliwe będzie wtedy gdy napotkamy symbol, istnieje w zdaniu opisującym symbol dowolny, a tak po ludzku, chodzi o taką sytuację:
![Przyklad](https://github.com/devmichalek/Kompilacja/blob/master/assets/1.2.5_0.png?raw=true)

#### Przeszukiwanie wszerz
Zacznijmy od pierwszego, rzadziej używanego algorytmu *Breadth-First Search* lub *BFS*. Algorytm polega na przeszukiwaniu wszerz zaczynając od symbolu najbardziej na lewo lub prawo. Algorytm wymaga zapamiętywania wszystkich węzłów w danej odległości od korzenia. Standardowe przejście po grafie wygląda następująco:<br>
![BFS](https://github.com/devmichalek/Kompilacja/blob/master/assets/1.2.5.0_0.png?raw=true)<br>
Od razu wspomnę, że próba szukania od symbolu najbardziej na prawo jest raczej **błędem**. Dlaczego? Zaczynając od symbolu najbardziej na prawo często niepotrzebnie rozwijamy symbol nieterminalny. Poniższa animacja ilustruje powyższy schemat przejść po drzewie tj. pierwszy rozwijany jest symbol najbardziej na lewo:
![BFS](https://github.com/devmichalek/Kompilacja/blob/master/assets/1.2.5.0_1.gif?raw=true)<br>
Jak można łatwo zauważyć szukanie klucza może potrwać naprawdę długo, dodatkowo szukanie zajmuje mnóstwo pamięci. Najbardziej nieprzydatną rzeczą okazuję się **rozwijanie symbolu nieterminalnego, który potencjalnie zawiera inne symbole nieterminalne z niepasującym kluczem**. Co można by było ulepszyć? Wyobraźmy sobie, że szukamy rozwiązania dla λ = ```int + int```. Zakładamy również, że posiadamy zasadę mówiącą, że κ = **a**B (κ - symbol dowolny, **a** - symbol terminalny, B - symbol nieterminalny), w przypadku gdy **a** nie jest tokenem ```int``` nie ma sensu dalej rozwijać symbolu nieterminalnego B. Unikając niepotrzebnego rozwijania (fachowo nazywane *branching factor*) symboli nieterminalnych znacznie zyskujemy na czasie. Oczywiście gdy nasz ciąg symboli składa się z wielu symboli nieterminalnych to rozwijanie wciąż może zając sporo czasu. Poniżej przedstawiam zestawy symboli po których będziemy się poruszać (opis gramatyki):<br>
![CFG](https://github.com/devmichalek/Kompilacja/blob/master/assets/1.2.5.0_2.png?raw=true)<br>
Animacja przedstawiająca poprawiony algorytm (przeszukujemy zaczynając od symbolu najbardziej na lewo), szukane zdanie to ```int + int```:<br>
![BFS](https://github.com/devmichalek/Kompilacja/blob/master/assets/1.2.5.0_3.gif?raw=true)<br>
Teraz wspomniany problem gdy pierwszym symbolem z lewej jest symbol nieterminalny (szukanie klucza rośnie wykładniczo), najpierw nasze zasady:<br>
![CFG](https://github.com/devmichalek/Kompilacja/blob/master/assets/1.2.5.0_4.png?raw=true)<br>
Szukane zdanie to ```caaaaaaaaaa```, animacja przedstawiająca problem:<br>
![BFS](https://github.com/devmichalek/Kompilacja/blob/master/assets/1.2.5.0_5.gif?raw=true)

#### Przeszukiwanie wgłąb
Drugim algorytmem, który postaram się omówić jest *Deep-First Search* lub *DFS*. Przeszukiwanie wgłąb polega na rozpatrywaniu jednej gałęzi i przechodzenia na kolejny węzęł *w jednej linii* w przypadku pasujących symboli, w przypadku niepasujących symboli wracamy się *do góry* po grafie. Algorytm w każdym momencie wymaga zapamiętania ścieżki od korzenia do bieżącego węzła. Schemat przejść po drzewie wygląda następująco (zaczynając od symbolu najbardziej na lewo):<br>
![DFS](https://github.com/devmichalek/Kompilacja/blob/master/assets/1.2.5.1_0.png?raw=true)<br>
Kilka zalet w stosunku do *BFS*:
- mniejsze zużycie pamięci (rozpatrywana jest jedna gałąź w danym momencie, nie trzymamy wskaźników na węzły znajdujące się w innych gałęziach a jedynie wskaźnik na dzieci)
- wysoka wydajność w stosunku do *BFS* (dla dobrze napisanej gramatyki)
- łatwy w implementacji

Animacja przedstawiająca szukanie rozwiązania dla ```int + int``` zaczynając od symbolu najbardziej na lewo:<br>
![DFS](https://github.com/devmichalek/Kompilacja/blob/master/assets/1.2.5.1_1.gif?raw=true)<br>
Problemy z przeszukiwaniem wgłąb podczas szukania rozwiązania dla ```c``` zaczynając od symbolu najbardziej na lewo wpadamy w nieskończoną rekurencję:<br>
![DFS](https://github.com/devmichalek/Kompilacja/blob/master/assets/1.2.5.1_2.png?raw=true)<br>

Przeszukiwanie wszerz | Przeszukiwanie wgłąb
--- | --- 
Działa dla każdej gramatyki | Działa dla gramatyki pozbawionej lewostronnej rekurencji (graf musi być skończony)
Złożoność pamięciowa w najgorszym wypadku - wykładnicza | Złożoność pamięciowa w najgorszym wypadku - liniowa
Czas wyszukiwania w najgorszym wypadku wykładniczy | Czas wyszukiwania w najgorszym wypadku wykładniczy

#### LL(1)
Poprzednie algorytmy zajmowały się wyszukiwaniem zgadując czy dane wyrażenie pasuje do rozwiązania, a w przypadku błędu wracały się po uprzednio utworzonej ścieżce. Istnieje również inna kategoria algorytmów parsujących tzw. algorytmów przewidujących. Zacznijmy od tego, że parsery, które przewidują swoje następne przejście po drzewie są po prostu szybsze. Dodatkowo, często wspierane są tablicą wypełnioną przejściami (o których za chwilę wspomnę) z poszczególnych węzłów na kolejny zyskując dzięki temu większą wydajność. Niestety ten rodzaj parsowania nie jest w stanie wyszukać rozwiązania dla każdej gramatyki. Zaczynając od pierwszego symbolu w jaki sposób jesteśmy w stanie stwierdzić, której produkcji użyć (do którego węzła przeskoczyć)? Podczas podejmowania decyzji parser sprawdza aktualny **oraz następny** token tzw. *lookahead token* (LL(n), L - skanujemy od lewej do prawej, L - derywacja lewostronna, n - liczba dodatkowo sprawdzanych tokenów *w przód*) w celu podjęcia decyzji. Warto zauważyć, że zwiększając liczbe dodatkowo sprawdzanych tokenów jesteśmy w stanie szukać rozwiązań dla bardziej złożonych gramatyk z drugiej strony im większa liczba sprawdzanych tokenów nasz parser staje się bardziej skompilowany. Poniżej przykład, w którym parser mając do dyspozycji jeden *lookahead token* wyszukuje rozwiązanie dla ```int + (int + int)```<br>
![LT](https://github.com/devmichalek/Kompilacja/blob/master/assets/1.2.5.2_0.gif?raw=true)<br>

#### LL(1) Parse Tables
Podczas parsowania LL(1) wszystkie nasze decyzje dotyczące rozwijania symboli nieterminalnych są niejako **wymuszone** poprzez rozpatrywanie następnego tokenu. W nastęnym nagłówku postaram się nieco przybliżyć zastosowanie tablic umożliwiających szybsze wyszukiwanie rozwiązania oraz **detekcje błędów** gramatycznych. Na poniższych ilustracjach przedstawiono kolejno: opis naszej gramatyki oraz tablicę przejść, pierwsza z lewej kolumna to wszystkie zdefiniowane przez nas symbole nieterminalne, natomiast w pierwszym wierszu znajdują się wszystkie symbole terminalne występujące w naszej gramatyce.<br>
![PT](https://github.com/devmichalek/Kompilacja/blob/master/assets/1.2.5.3_0.png?raw=true)<br>
Jak to wszystko działa? Tym razem podczas parsowania zdania ```(int + (int * int))``` zaznaczmy gdzie się ono kończy poprzez wstawienie dodatkowego symbolu, który nie występuje w naszym języku, symbolicznie będzie to znak dolara ```$``` (w wielu językach ułatwiono zadanie parserom poprzez wstawianie znaku średnika na końcu linii ```;```, w przypadku Pythona liczone są znaki białe), od teraz nasze zdanie to ```(int + (int * int))$```, takie dodatkowe wstawienie symbolu ułatwi nam w dużym stopniu detekcje błędów. Poniżej ilustracja procesu parsowania tablicą przejść.<br>
![PT](https://github.com/devmichalek/Kompilacja/blob/master/assets/1.2.5.3_1.png?raw=true)<br>
Wykrywanie błędów gdy posiadamy znak kończący zdanie oraz tablice przejść:<br>
![WB](https://github.com/devmichalek/Kompilacja/blob/master/assets/1.2.5.3_2.png?raw=true)<br>
![WB](https://github.com/devmichalek/Kompilacja/blob/master/assets/1.2.5.3_3.png?raw=true)<br>
Spróbujmy podsumować algorytm działania parsera LL(1):
- Zaczynając od symbolu dowolnego **S** oraz tablicy przejść **T** inicjujemy stos **S$**
- Powtarzamy dopóki stos nie jest pusty:
  - Pobierz aktualnie rozpatrywany token ```t```
  - Jeśli na samym wierzchu znajduję się symbol terminalny ```r```
    - Gdy ```r != t``` zwróć błąd
    - W przeciwnym wypadku pobierz token ```t``` oraz usuń ```r``` ze stosu
  - Jeśli na samym wierzchu znajduje się symbol nieterminalny ```A```
    - Jeśli ```T[A, t]``` jest niezdefiniowane, zwróć błąd
    - W przeciwnym wypadku zamień pierwszy element stosu na ```T[A, t]```

#### FIRST, FOLLOW
W ostatnim podpunkcie postaram się krótko przybliżyć w jaki sposób tworzone są tablice przejść dla parsera LL(1). Podczas parsowania chcielibyśmy wiedzieć czy dany symbol nieterminalny może zostać rozwinięty do symbolu terminalnego występującego na wejściu, aby to zrobić niezbędna jest tablica FIRST, która reprezentuje wszystkie **możliwe symbole terminalne, które mogą wystąpić przed symbolem nieterminalnym** przy jego rozwijaniu oraz tablica FOLLOW, która reprezentuje wszystkie **możliwe symbole terminalne, które mogą wystąpić po danym symbolu nieterminalnym**. Przykład poniżej przedstawia utworzone wspomniane wcześniej tablice (dla jasności symbol ```ε``` oznacza pusty symbol):<br>
![FIRSTFOLLOW](https://github.com/devmichalek/Kompilacja/blob/master/assets/1.2.5.4_0.png?raw=true)<br>
Omówiony wcześniej parser LL(1) jest parserem wspomaganym tablicą *(table-driven LL(1))*, jednak istnieje również inne równie szybkie podejście algorytmiczne tego parsera. Tak zwany *recursive-descent LL(1)* to parser, w którym każdy symbol nieterminalny zdefiniowany jest jako osobna funkcja, znając następny token wołana jest ta odpowiednia.

### Analiza wstępująca
Przez jakiś czas zastanawiałem się nad podziałem materiału dotyczącej analizy wstępującej, jest to bowiem bardziej obszerny temat, dzieje się tak ponieważ ten typ analizy jest niejako częściej implementowany w współczesnych parserach. Pomimo możliwości użycia BFS i DFS w tworzeniu parsera, analiza zstępująca jest raczej **rzadko** spotykana. Podczas analizy zstępującej najpierw rozpatrywany był symbol dowolny, to od niego parser zaczynał **rozwijać** kolejne symbole nieterminalne, w przypadku analizy wstępującej sprawa wygląda nieco inaczej, w tym przypadku parser **redukuje** tokeny konwertując je na symbole nieterminalne tak aby z czasem dojść do symbolu dowolnego (osobiście uważam, że takie rozwiązanie jest bardziej logiczne). Oto jak analiza wstępująca wygląda:<br>
![BOTTOM-UP](https://github.com/devmichalek/Kompilacja/blob/master/assets/1.2.6_0.png?raw=true)<br>
![BOTTOM-UP](https://github.com/devmichalek/Kompilacja/blob/master/assets/1.2.6_1.gif?raw=true)<br>

#### Redukcje, Przesunięcia, Uchwyty
Zacznijmy od uchwytów w angielskim nazywane *handle* oznacza uzupełnioną grupę symboli znajdujących się po lewej stronie gotową do redukcji. Podczas analizy wstępującej zajmować się będziemy wyszukiwaniem uchwytów w sposób kierunkowy (skanując od lewej do prawej) patrząc o jeden token do przodu, taki typ parsowania nazywamy kierunkowym. Istnieje również druga grupa parserów wstępujących tj. [bezkierunkowych](https://en.wikipedia.org/wiki/CYK_algorithm). Spójrzcie proszę na poniższy przykład, jak znaleziony został uchwyt dla zdania:
```
1 + 2 * 3 => int + int * int
```
![Zly uchwyt](https://github.com/devmichalek/Kompilacja/blob/master/assets/1.2.6.0_0.png?raw=true)<br>
Jak widać powyższy uchwyt jest błędny, dochodzimy do wniosku, że redukcja lewostronna **nie zawsze** da nam poprawny uchwyt. Liczba ```2``` została zinterpretowana jako **int => T => F** co jest błędem, oczekiwaliśmy interpretacji **int => T => F * T**. W tym momencie powinniśmy sobie zadać kilka pytań:.

#### Gdzie są uchwyty?
Parser, który rozpatrzymy to LL(1) tj. parser rozpatrujący o jeden token wprzód. Pomysł polega na podzieleniu zdania na wejściu na dwie części. Lewa strona to nasza "strefa robocza", wszystkie uchwyty muszą się tam znajdować, prawa strona zawiera wejście, które nie zostało jeszcze przeczytane (składa się tylko i wyłącznie z symboli terminalnych). Stopniowo będziemy zajmować się przesuwaniem symboli teminalnych z prawej strony na strefę roboczą po lewej stronie. Rozpatrzmy zdanie ```int + int * int + int```:<br>
![Przesuniecia](https://github.com/devmichalek/Kompilacja/blob/master/assets/1.2.6.1_0.gif?raw=true)<br>
Skoro redukcja przeprowadzana jest po prawej stronie strefy roboczej, nigdy nie przesuniemy symbolu z lewej do prawej. Ważne aby od tego momentu traktować lewą strone jako stos do którego wrzucamy symbole terminalne z prawej strony. Dochodzimy do wniosku, że uchwytem nazywamy element znajdujący się na górze stosu (uchwyty znajdują się na górze stosu).

#### Jak szukamy uchwytów?
Podczas parsowania metodą przesuń/zredukuj za każdym razem jesteśmy zobowiązani zdecydować jaką akcję chcemy podjąć. Czy zredukować symbol? Czy być może pobrać więcej symboli z prawej strony? Skąd wiemy co należy wykonać? Wiemy, że uchwyt pojawi się zawsze na końcu zdania po lewej stronie, gdybyśmy w jakiś sposób znaleźli wzór na rozpoznawanie uchwytów będziemy wiedzieć kiedy wykonać redukcję a kiedy przesunięcie. Ponownie rozpatrzmy zdanie ```int + int * int + int``` tym razem z innej perspektywy:<br>
![Redukcja](https://github.com/devmichalek/Kompilacja/blob/master/assets/1.2.6.2_0.gif?raw=true)<br><br>
Śledząc naszą pozycję wyglądałoby to w następujący sposób:<br>
![Sledzenie](https://github.com/devmichalek/Kompilacja/blob/master/assets/1.2.6.2_1.gif?raw=true)<br><br>
W dowolnym momencie generacja zawartości po lewej stronie może zostać opisana w następujący sposób:
 - Zaczynając od symbolu startowego S śledź produkcje, które nie są jeszcze kompletne (produkcje, które nie zostały jeszcze zredukowane do końca) oraz to gdzie w danej produkcji się znajdujemy (na powyższej animacji zaznaczone jest to znakiem kropki **·**)
 - Dla każdej produkcji, w kolejności, zredukuj kolejne symbolu do punktu, w którym się znajdujemy. Inaczej mówiąc redukuj na bieżąco to co znajduje się po naszej lewej stronie kropki **·**

Posiadając algorytm do generacji lewej strony czy jesteśmy w stanie zbudować mechanizm do "rozpoznawania lewej strony"? W każdym momencie parsowania śledzimy, w której produkcji się znajdujemy oraz jak daleko jesteśmy w tej produkcji. W każdym momencie próbujemy dopasować symbol po prawej stronie jako nowego kandydata na symbol po lewej stronie lub jeśli jest to symbol terminalny próbujemy zgadnąć, której produkcji użyć. Projektanci kompilatora doszli do wniosku, że istnieje skończona liczba produkcji, w której istnieje skończona liczba pozycji w jakiej możemy się znaleźć. W każdym momencie jesteśmy zobowiązani śledzić gdzie się znajdujemy tylko w jednej produkcji. Dlaczego się nad tym zastanawiać? Do tego typu zadań świetnie nadają się automaty skończone. Naszym pierwszym celem było szukanie uchwytów. Podczas działania automatu gdy kiedykolwiek znajdziemy się w produkcji w takim miejscu gdzie **·** znajduję się na końcu ![Kropka](https://github.com/devmichalek/Kompilacja/blob/master/assets/1.2.6.2_2.png?raw=true) to prawdopodobnie będzie to uchwyt. Póki co pozwolę sobie pominąć jak generowane są automaty skończone do szukania uchwytów (swoją drogą tego typu mechanizm wbudowany jest generatorze bison).

#### LR(0)
Nasz automat wskaże nam miejsca, w których potencjalnie znajduję się uchwyt, jednakże potrzebujemy jakiegoś sposobu na potwierdzenie tej informacji. Do tego celu użyjemy parsera rozpatrującego **(0)** tokenów w przód, skanującego wejście od **l**ewej do prawej z derywacją **p**rawostroną  *(**r**ightmost derivation)*. Parsery LR(0) zwykle reprezentowane są za pomocą tabeli *action* i tabeli *goto*. Tabela akcji przypisuje każdemu stanowi określoną akcję tj. przesunięcie lub redukcję. Tabela goto mapuje następny stan każdemu z stanów (symboli). Dla chętnych poniżej zostawiam grafikę z wygenerowaną tablica i automatem skończonym.<br>
![Automat](https://github.com/devmichalek/Kompilacja/blob/master/assets/1.2.6.3_0.png?raw=true)<br>
![Tabela](https://github.com/devmichalek/Kompilacja/blob/master/assets/1.2.6.3_1.png?raw=true)<br><br>
Niestety tak jak poprzednie techniki parsowania również i LR(0) nie jest w stanie rozpatrzyć każdej gramatyki dlatego krótko postaram się przybliżyć konflikty, które występują pomiędzy przesunięciami a redukcjami.
 - Konflikt przesunięcie/redukcja - konflikt, w którym nie jesteśmy w stanie stwierdzić czy należy pobrać więcej symboli z wejścia czy zredukować aktualnie pobrane symbole. Występuje zwykle gdy dwie produkcje nakładają się na siebie.
 - Konflikt redukcja/redukcja - konflikt, w którym nie jesteśmy w stanie stwierdzić, którą redukcje przeprowadzić. Powodem może okazać się "niejednoznaczność" gramatyki.
 
Na koniec warto zauważyć, że konflikt przesunięcie/przesunięcie nigdy nie wystąpi. Dlaczego LR(0) jest słaby? LR(0) akceptuje gramatyki, w której uchwyt pozbawiony jest prawego kontekstu tzn. nasz parser wyszukuje uchwytów jedynie po stronie lewej.

#### LR(1)
Znacznie potężniejszym parserem jest LR(1). Decyzja w szukaniu uchwytów bazuję na rozpatrywaniu jednego tokena w przód.

### Bison
Na koniec chciałbym przedstawić generator parserów o nazwie Bison, poniżej znajduje się lista świetnych tutoriali odnośnie tego generatora:<br>
[Flex and Bison - Aquamentus](https://aquamentus.com/flex_bison.html)<br>
[Flex and Bison in C++ - Jonathan Beard](http://www.jonathanbeard.io/tutorials/FlexBisonC++)<br>
[Using Flex and Bison - Mactech](http://preserve.mactech.com/articles/mactech/Vol.16/16.07/UsingFlexandBison/index.html)

## Analiza semantyczna
Podczas analizy semantycznej na podstawie wcześniej sprawdzonej i utworzonej struktury drzewa następuję sprawdzanie poprawności typów, instrukcji i programu jako całości (analiza ta sprawdza czy program ma jakikolwiek sens). Ilość zadań i poziom skomplikowania podczas tej analizy zależy w głównej mierze od specyfikacji języka.

## Generacja IR

## Optymalizaja IR

## Generacja kodu

## Optymalizacja

## Źródła
[Avanced C and C++ Compiling](https://doc.lagout.org/programmation/C/Advanced%20C%20and%20C%20%20%20Compiling%20%5BStevanovic%202014-04-28%5D.pdf)<br>
[Compiler Construction Principles and Practice](https://csunplugged.files.wordpress.com/2012/12/compiler-construction-principles-and-practice-k-c-louden-pws-1997-cmp-2002-592s.pdf)<br>
[Linkers and Loaders](http://www.becbapatla.ac.in/cse/naveenv/docs/LL1.pdf)<br>
[Anatomy of Program in Memory](https://manybutfinite.com/post/anatomy-of-a-program-in-memory/)<br>
[An Introduction to GCC](https://tfetimes.com/wp-content/uploads/2015/09/An_Introduction_to_GCC-Brian_Gough.pdf)<br>
[Beginner's Guide to Linkers](https://www.lurklurk.org/linkers/linkers.html)<br>
[Position Independent Code and x86-64 libraries](https://www.technovelty.org/c/position-independent-code-and-x86-64-libraries.html)<br>
[Depth First and Breadth First Tree Walking](https://nick.balestrafoster.com/2015/depthFirst-vs-breadthFirst/)<br>
[Version Script](https://ftp.gnu.org/old-gnu/Manuals/ld-2.9.1/html_node/ld_25.html)<br>
[Calling Conventions Demystified](https://www.codeproject.com/Articles/1388/Calling-Conventions-Demystified)<br>
[The History of Calling Conventions](https://devblogs.microsoft.com/oldnewthing/20040102-00/?p=41213)<br>
[Name Mangling](https://en.wikipedia.org/wiki/Name_mangling)<br>
[Binary File Descriptor](https://en.wikipedia.org/wiki/Binary_File_Descriptor_library)<br>
[Stanford CS143 About Compilation](https://web.stanford.edu/class/archive/cs/cs143/cs143.1128/lectures/)<br>
