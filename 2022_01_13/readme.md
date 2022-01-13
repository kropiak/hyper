# Koło naukowe HYPER - spotkanie 13.01.2022
# Omówienie i przykłady wykorzystania wybranych klas API Unity

Wybór klas będzie w pewnym stopniu pokrywał się z najważniejszymi wybranymi klasami w manualu Unity dostępnym pod adresem: https://docs.unity3d.com/Manual/ScriptingImportantClasses.html


> TODO
> * wybranie kilku klas
> * przeglądnięcie i wybranie przykładów z dokumentacji
> * określenie konkretnego celu jaki chcę zrealizować tym warsztatem np. stworzenie skryptów pozwalających na przemieszczanie obiektów po scenie w określony sposób
> * pokazanie jak można obsługiwać własne skrypty przez okno Inspektora
> * użycie Lerp

## 1. Klasa [MonoBehaviour](https://docs.unity3d.com/ScriptReference/MonoBehaviour.html)

Każdy skrypt w silniku Unity dziedziczy po klasie MonoBehaviour. Klasa ta dostarcza funkcji pozwalających na umieszczanie kodu koniecznego do wykonania w określonych momentach cyklu życia gry. W trakcie inicjalizacji obiektu gry, w każdej klatce animacji, w każdej klatce obliczeń silnika fizycznego, dostarcza obsługi kolizji itp.

Przydatna grafika prezentująca cykl życia i zależności wszystkich elementów silnika Unity jest zaprezentowana pod adresem: https://docs.unity3d.com/Manual/ExecutionOrder.html

## 2. Klasa [GameObject](https://docs.unity3d.com/ScriptReference/GameObject.html)


## 3. Klasa [Transform](https://docs.unity3d.com/ScriptReference/Transform.html)

Komponent typu Transform jest jedynym komponentem, który występuje w każdym obiekcie gry silnika Unity. Ten komponent zawiera trzy składowe każdego obiektu gry: pozycję, skalę oraz rotację.


# Warsztat

Najlepszą drogą do nauki jest nauka przez "robienie" (ang. learning by doing), więc spróbujemy.

Cele, które chcę osiągnąć:
* umiejętność stworzenia nowego projektu Unity
* dodanie nowej sceny
* dodanie elementów do sceny
* dodanie nowego skryptu
* podpięcie skryptu pod obiekt gry
* interakcja z poziomu skryptu z obiektem gry
  * pobieranie wartości cech obiektu oraz jego komponentów
  * zmiana wartości wybranych cech
  * zmiana pozycji, skali i rotacji obiektów z poziomu skryptów

**_Listing 1_** - zmiana pozycji obiektu w postaci ilości jednostek na klatkę
```csharp
using UnityEngine;

public class ChangePosition : MonoBehaviour
{
    // Start is called before the first frame update
    void Start()
    {
        // wyświetlimy info o podpiętym obiekcie gry - tylko raz
        Debug.Log($"Podpięty obiekt gry {gameObject.name}");
    }

    // Update is called once per frame
    void Update()
    {
        // będziemy przesuwać obiekt (któremu podpięto ten komponent) o
        // określoną liczbę pikseli w każdej klatce animacji (chociaż to nie
        // jest dobry pomysł)
        // prędkość poruszania się obiektu będzie uzależniona od ilości FPS czyli
        // częstotliwości wywołania metody Update()

        // odkomentuj/zakomentuj
        transform.position = transform.position + Vector3.back;
        // wolniej ale wciąż ten sam problem
        // transform.position = transform.position + new Vector3(0,0,0.001f);
    }
}
```
Ruch powinien jednak odbywać się niezależnie od aktualnej wartości FPS. Ptrzebujemy informacji o upływającym czasię między kolejnymi klatkami animacji. Korzystamy w tym celu z klasy [Time](https://docs.unity3d.com/Manual/TimeFrameManagement.html).


**_Listing 2_** - zmiana pozycji obiektu w postaci ilości jednostek na sekundę
```csharp
using UnityEngine;

public class ChangePosition : MonoBehaviour
{
    // Start is called before the first frame update
    void Start()
    {
        // wyświetlimy info o podpiętym obiekcie gry - tylko raz
        Debug.Log($"Podpięty obiekt gry {gameObject.name}");
    }

    // Update is called once per frame
    void Update()
    {
        // będziemy przesuwać obiekt (któremu podpięto ten komponent) o
        // określoną liczbę pikseli w każdej klatce animacji (chociaż to nie
        // jest dobry pomysł)
        // prędkość poruszania się obiektu będzie uzależniona od ilości FPS czyli
        // częstotliwości wywołania metody Update()

        // odkomentuj/zakomentuj
        // transform.position = transform.position + Vector3.back;
        // wolniej ale wciąż ten sam problem
        // transform.position = transform.position + new Vector3(0,0,0.001f);
        
        // powinniśmy to uniezależnić poprzez wykorzystanie klasy Time i określenie
        // prędkości w danej jednostce czasu - tu ruch jednostajny

        transform.position = transform.position + Vector3.back * Time.deltaTime;
    }
}
```

Możemy dodać do tego skryptu dodatkowy parametr `speed`.

**_Listing 3_** - zmiana pozycji obiektu w postaci ilości jednostek na sekundę + prędkość
```csharp
using UnityEngine;

public class ChangePosition : MonoBehaviour
{
    // Start is called before the first frame update
    private float speed = 1.0f;
    void Start()
    {
        // wyświetlimy info o podpiętym obiekcie gry - tylko raz
        Debug.Log($"Podpięty obiekt gry {gameObject.name}");
    }

    // Update is called once per frame
    void Update()
    {
        // będziemy przesuwać obiekt (któremu podpięto ten komponent) o
        // określoną liczbę pikseli w każdej klatce animacji (chociaż to nie
        // jest dobry pomysł)
        // prędkość poruszania się obiektu będzie uzależniona od ilości FPS czyli
        // częstotliwości wywołania metody Update()

        // odkomentuj/zakomentuj
        // transform.position = transform.position + Vector3.back;
        // wolniej ale wciąż ten sam problem
        // transform.position = transform.position + new Vector3(0,0,0.001f);
        
        // powinniśmy to uniezależnić poprzez wykorzystanie klasy Time i określenie
        // prędkości w danej jednostce czasu - tu ruch jednostajny

        transform.position = transform.position + Vector3.back * Time.deltaTime * speed;
    }
}
```

**Zadanie 1**  
Zmodyfikuj parametr `speed` na `speed_z` i dodaj kolejne dwa parametry `speed_y` oraz `speed_x`. Zmodyfikuj aktualny skrypt tak, żeby te parametry miały wpływ na prędkość poruszania się w danej płaszczyźnie.

**Zadanie 2**  
Stwórz nowy skrypt, w którym wykorzystując metodę `MoveTowards` klasy `Vector3` osiągniesz efekt pozwalający na przypisanie obiektu (targetu), do którego nasz cube będzie się poruszał. Ten target powinien być określany poprzez inspektora poprzez wybranie istniejącego obiektu na scenie.