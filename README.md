```
This README document is written in Polish language according to hackathon requirments. 
This tutorial would be used in High School, and it's goal is to be as simple to understand
as it is possible. 

If you have any specific question about code please create an issue.
```

# Wstęp
## Tło
### Android
Android – system operacyjny z jądrem Linux dla urządzeń mobilnych takich jak telefony komórkowe, smartfony, tablety i netbooki oraz imteligentne zegarki. W 2013 roku był najpopularniejszym systemem mobilnym na świecie. Wspomniane jądro oraz niektóre inne komponenty, które zaadaptowano do Androida opublikowane są na licencji GNU GPL. Android nie zawiera natomiast kodu pochodzącego z projektu GNU. Cecha ta odróżnia Androida od wielu innych istniejących obecnie dystrybucji Linuksa (określanych zbiorczo mianem GNU/Linux). Początkowo był rozwijany przez firmę Android Inc. (kupioną później przez Google), następnie przeszedł pod skrzydła Open Handset Alliance.

Android zrzesza przy sobie dużą społeczność deweloperów piszących aplikacje, które poszerzają funkcjonalność urządzeń. W sierpniu 2014 było dla tego systemu dostępnych ponad 1,3 miliona aplikacji w Google Play (wcześniej Android Market)[5].

Od kwietnia 2017 roku Android ma największe udziały na rynku systemów operacyjnych.

Android w odróżnieniu od konkurencji w postaci iOS(Apple), Windows Phone (Microsoft) boryka się ze znaczną fragmentacją wersji systemów zainstalowanych na urządzeniach. Zjawisko fragmentacji wpływa na sposób rozwoju oprogramowania, gdyż należy uwzględnić nie tylko najnowsze i najpotężniejsze urządzenia ale też urządzenia starsze i te z nieaktualnym systemem operacyjnym.

### libGDX

(Tu krótki opis czym jest libGDX i dlaczego wykorzystujemy)

### Space Invaders (Najeźdźcy z Kosmosu)

(Tu opis plus screeny space invaders)

![Space Invaders](./static/space-invaders.png)

## Organizacja Tutoriala

Niniejszy kurs podzielony został na lekcje. Każda z lekcji pomoże Ci zrozumieć kolejne aspekty programowania.

* Lekcja 1 - Szablon i struktura projektu
* Lekcja 2 - ...
...

Każda z lekcji ma przypisany odpowiedni TAG w repozytorium, po to żebyś mógł przywrócić wersję programu do stanu początkowego dla danej wersji. Dzięki temu nie będziesz się martwić, gdy coś zajdzie zadaleko w niewłaściwym kierunku.

W celu ustawienia stanu zerowego dla danej lekcji wprowadź w linii komend:

'''
#> git checkout tags/Lekcja-1
#> git reset --hard origin/master
#> git clean -xf
'''

Ale jeżeli linia komend jest dla Ciebie przerażająca przygotowaliśmy specjalnie skrypty w katalogu '''githelp''', które pomogą Ci w przechodzeniu pomiędzy lekcjami:

''' .githelp/lekcja.bat Lekcja-1 '''

# Lekcje dla Liceum im. Kasprzaka

## Lekcja 0 - Szablon projektu

### Co robimy
Najpierw tytułem wstępu wyjaśnimy Ci co chcemy osiągnąć naszym kursem.
Będziemy ćwiczyć programowanie, poprzez implementacje klasyki gier komputerowych: Space Invaders.

Reguły jakie chcemy spełnić:
1. Wrogowie znajdują się w trzech rzędach na środku ekranu
2. Statek znajduje się na dole ekranu
3. Statek porusza się w lewo i prawo w płaszyźnie ziemi ( pojawia się w miejscu dotknięcia, lub można go przeciągać)
4. Statek może strzelać do kosmitów (przez dotknięcie powyżej 1/3 ekranu)
5. Kosmici strzelają w stronę ziemi

### Struktura projektu:

```
|_ core
|_ android
|_ desktop
```

android - moduł zawiera małą aplikacje, która uruchamia naszą grę napisaną w libGDX

desktop - moduł zawiera równie małą aplikację, która uruchamia naszą gre napisaną w libGDX

core - to jest najważniejszy moduł wszystkie zmiany będziemy wprowadzać w tym module, jest to serce naszej implementacji

#### Komponenty

![diagram komponentow](http://uml.mvnsearch.org/github/sratatata/space-invaders/blob/master/static/core-components.puml)

## Lekcja 1 - Dziedziczenie

Temat poświęcony dziedziczeniu rozpoczniemy od prostego przykładu, który być może niektórzy z Was 
widzieli już wielokrotnie w różnego rodzaju publikacjach poświęconych językom obiektowym jakim bez 
wątpienia jest język Java. 
Zanim jednak zajmiemy się przykładem zapoznajmy się z encyklopedyczną definicją:

    Dziedziczenie (ang. inheritance) – mechanizm współdzielenia funkcjonalności między klasami. 
    Klasa może dziedziczyć po innej klasie, co oznacza, że oprócz swoich własnych atrybutów oraz zachowań, 
    uzyskuje także te pochodzące z klasy, z której dziedziczy.

Dlaczego chcielibyście współdzielić funkjonalności pomiędzy klasami? 
Odpowiedź jest prosta, żeby powtarzające się, wspólne zachowania dla różnych klas były implementowane 
tylko jeden raz. Jak mogą Was niektórzy autorzy przekonywać, wcale nie chodzi tutaj o lenistwo programistów. 
Pozwolę tu sobie na jeszczę jedną mądrość ludową

    Kod, który z całą pewnością nie zawiera błędów to ten kod, który nie istnieje.
    
Już z spieszę z wyjaśnieniem, otóż pisząć jakąś funkjonalność 2, 3 a może i więcej razy, łatwo jest 
popełnić, w jednej z implementacji błąd. Zatem jeżeli możemy uniknąć powtarzania takiego samego 
kodu możemy uniknąć niepotrzebnych błędów. No ale przecież używam copy-paste, móglibyście odpowiedzić,
wtedy nie zrobię żadnej literówki. Tak zgadza się, ale co jeżeli ta kod, który skopiowaliście do 5, 
może 20 różynych klas zawiera błąd? Już nie jest to takie oczywiste jak to poprawić i nie wprowadzić
dodatkowych błędów, copy-paste już nie pomoże. Dlatego właśnie języki obiektowe zyskały tak ogromną 
popularność, poprzez koncept dziedziczenia pozwalaja unikać zbędnych powtórzeń. W ogólności powyższe
zagadnienie opisuje zasadę DRY (Don't repeat yourself). 

Dobrze, więc wyjaśniwszy sobie sens dziedziczenia, przejdźmy do naszego sztampowego przykładu - zwierzątek. 
Wyobraźmy sobie zwierzę (_Animal_), zwierzę może być scharakteryzowane w jakiś sposób, weźmy na ten 
przykład wielkość, a właściwie bardzej precyzyjnie to jego masę, mięcho w sensie. 

```java
class Animal{
    int weight; //in kilograms
}
```

To jeżeli już mamy materialne zwierzę w jakiś tam sposób scharakteryzowane, to co takie zwierze robi? 
No jeżeli oglądaliście Shreka to takie zwierze może gadać! 

```java
class Animal{
    int weight; 
    
    String noise(){
        return "Ja latam, gadam, pełny serwis!!!";
    }
}
```

No dobrze więc jak już pewnie wiecie z poprzednich lekcji to taką klasę jak powyżej to możemy sobie
i zmaterializować, w sensie zrobić sobie obiekt tej klasy i zapisać jego referencję do zmiennej,
następnie możemy temu zwierzakowi udzielić głosu i wywołać metodę `noise()`.

```java
Animal animal = new Animal();
String noise = animal.noise();
System.out.println(noise);

//: Ja latam, gadam, pełny serwis!!!
```

No dobra nic odkrywczego, czekajcie. Nadal nie wiemy co to za zwierzę jest, pewnie się domyślacie:

```java
class Donkey{
    int mass;
    
    void noise(){
        System.out.println("Daleko jeszcze?! Iooo Iooo");
    }
}
```

no dobra to sprawdźmy naszego osła:

```java
Donkey donkey = new Donkey();
donkey.noise();

//: Daleko jeszcze?! Iooo Iooo
```

Super! Dobra to to jak mamy jakieś zwierzę i mamy osła to może tak zamkniemy je w jednej głośnej zagrodzie:

```java
List farm = new ArrayList(); 
farm.add(animal);
farm.add(donkey);



```




//Space invaders - dziedziczenie: przesłanianie metod, enkapsulacja, interfejsy
## Lekcja 2 - Enkapsulacja, interfejsy

###Enkapsulacja

Enkapsulacja, inaczej hermetyzacja jest ukrywaniem szczegółów implementacji oraz uniemożliwieniem zmiany
stanu obiektu przez inne klasy. Gwarantuje nam, że jedynym obiektem odpowiedzialnym za zmianę stanu jest sam obiekt
i nie ma możliwości modyfikacji jego stanu z zewnątrz. 

Jednym z oczywistych sposobów częściowego uzyskania takiego zachowania jest stosowanie 
modyfikatorow dostępu jak najniższego poziomu, tzn. zaczynamy pisać kod tak, że
wszystkie pola i metody sa private i dokonujemy ich modyfikacji dopiero gdy planujemy użyć 
danego pola/metody w innym miejscu. Niestety jest to tylko puntk wyjscia dla poprawnie enkapsulowanej klasy, 
ponieważ istnieje możliwość, że z jakiegoś powodu chcemy udostępnić stan obiektu, który to może zostać
zmodyfikowany. Przykład takiej niepoprawnej enkapsulacji możemy znaleźć w listingu klasy
`Shoot` z naszego projektu poniżej:

```java
public abstract class Shoot {
    protected Rectangle position;
    
    protected GraphicsManager.Graphics graphics;
    

    public Shoot(GraphicsManager.Graphics graphics, float originX, float originY) {
        this.graphics = graphics;
        position = new Rectangle();
        position.x = originX;
        position.y = originY;
        position.height = 10;
        position.width = 10;
    }
    


    public abstract boolean isOutsideScreen();

    public abstract void updateState();
    

    public void render(SpriteBatch batch, float animationTime) {
        TextureRegion shotFrame = graphics.frameToRender(animationTime);
        batch.draw(shotFrame, position.x, position.y);
    }
    

    public Rectangle position(){
        return position;
    }
    
}
```

#####Zadanie - zidentyfikować i poprawić błąd związany z enkapsulacją.

###Interfejsy
Interfejs jest zbiorem metod wyznaczających pewną, powiązaną ze sobą funkcjonalność. Do javy 8 był to 
tylko zbiór nagłówków metod, bez implementacji, przykłądem nieche będzie `Screen` z libgdx:

```java
/** <p>
 * Represents one of many application screens, such as a main menu, a settings menu, the game screen and so on.
 * </p>
 * <p>
 * Note that {@link #dispose()} is not called automatically.
 * </p>
 * @see Game */
public interface Screen {
	
	/** Called when this screen becomes the current screen for a {@link Game}. */
	public void show ();
	
	/** Called when the screen should render itself.
	 * @param delta The time in seconds since the last render. */
	public void render (float delta);
	

	/** @see ApplicationListener#resize(int, int) */
	public void resize (int width, int height);
	

	/** @see ApplicationListener#pause() */
	public void pause ();
	

	/** @see ApplicationListener#resume() */
	public void resume ();
	

	/** Called when this screen is no longer the current screen for a {@link Game}. */
	public void hide ();
	

	/** Called when this screen should release all resources. */
	public void dispose ();
	
}
```

Od javy 8 interfejsy mogą mieć domyslną implementację metod:
```java
public interface A {
    default void foo(){
       System.out.println("Calling A.foo()");
    }
}
```

Jeśli klasa implementuje interfejs musi ona zapewnić implementację każdej metody w nim zawartej 
(wyjątkiem jest klasa abstrakcyjna). Każda klasa może implementować dowolną liczbę interfejsów. Jest 
to jedna z głównych różnic między klasą abstrakcyjną a interfejsem. Drugą jest ograniczenie, że interfejs
nie może zawierać pól. Interfejsy ułatwiają enkapsulację, w klasie możemy mieć pole będące interfacem,
i posługiwać się nim bez wiedzy jakiej konkretnie klasy faktycznej używamy. Interesuje nas czynność jaką
interfejs umożliwia wykokanie, a nie sama implementacja. Poprawne używanie interfejsów sprawia, żę kod jest
czeytelniejszy i bardizej przejrzysty, oraz posiada mniej zależności pomiędzy klasami.

Dobrym przykładem interfejsu jest ``Screen`` z powyższego listingu. Mamy klasa `Game`, która zarządza 
poszczególnymi ekranami, za pomocą metody ``setScreen()``, dzięki czemu mamy wydzieloną logikę każdego 
ekranu w osobnym miejscu. Natomiast samej klasy ``Game`` nie interesuje implementacja każdego ekranu z oosbna.
Jest to szczegół implementacyjny na poziomie pojedynczego ekranu.

#####Zadanie - napisać i podpiąć ekran po wygraniu gry.

###Elementy libgdx

####BitmapFont 
Służy do rysowania tekstu. Podstawowe użycie wygląda następująco:

```java
font = new BitmapFont();
font.getData().setScale(3); //mozna skalowac
font.draw(spaceInvaders.batch, "Touch screen to restart", 10, (Gdx.graphics.getHeight()/2)-50);
```
https://github.com/libgdx/libgdx/wiki/Bitmap-fonts do pobrania nardzedzie Hiero do konwertowania czcionek
#####Zadanie - podpiąć własną czcionkę do aplikacji

## Lekcja 3 - Animacja postaci

Przez postać rozumiemy rakiety, obcych ale także pociski.

W naszym przykładzie wykorzystamy technike stosowaną przez naszych dziadków ;) zwaną Sprite animation. Podejście to polega na przechowywaniu grafiki w plikach typy mapa bitowa, w tym wypadku png. Kolejne klatki są zapisane w tym samym pliku w postaci kolumn i wierszy. Technika ta polega na indeksowaniu i wybieraniu kolejnych sekwencji z pliku.

![Sprite z Rakieta](/space-invaders/graphics/rakieta.png)

//todo wczytywanie sprite, przyklad

## Lekcja 4 - Poruszanie postacią

## Lekcja 5 - Strzelanie

## Lekcja 6 - Wykrywanie kolizji

## Lekcja 7 -

# Słownik
* Repozytorium - odnosi się do systemów kontroli wersji, w tym wypadku GIT. Repozytorium przypomina bibliotekę z wszystkimi wersjami napisanego programu.
* TAG - znacznik wyróżniający konkretną wersję programu w repozytorium.

# Bibliografia
[1] https://pl.wikipedia.org/wiki/Android_(system_operacyjny)
[2] Android Podręcznik Hackera: Joshua J. Drake, Pau Oliva Fora, Zach Lanier, Collin Mulliner, Stephen A. Ridley, Georg Wicherski
