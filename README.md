# Analiza sentymentu tweetów z MS Azure
> Projekt w ramach przedmiotu "Programowanie usług w chmurze", Politechnika Warszawska, Wydział Elektryczny, 2021Z

Autorzy:
1. Monika Osiak (291094),
2. Sebastian Kalisz (285072)

## Opis
### Wstęp
Głównym celem projektu była realizacja potrzeby biznesowej przy wykorzystaniu rozwiązań chmurowych Microsoft Azure.

Zdecydowaliśmy się na zrealizowanie aplikacji, która umożliwia analizę sentymentu tekstu. Jako dane do analizy 
wykorzystaliśmy tweety pobierane z Twittera za pomocą udostępnionego API. Pozwoliło nam to jednocześnie na wykorzystanie 
usług MS Azure, usług zewnętrznych, jak i napisanie własnego kodu (back-end).

Tweety są wyszukiwane po hashtagu. W zależności od treści tweeta, może on zostać określony jako pozytywny, neutralny lub 
negatywny.

### Rozwiązanie
Zdecydowaliśmy się na stworzenie prostej aplikacji webowej, dzięki której użytkownik może wpisać hasztag i wybrać
przedział czasowy, w którym mają zawierać się analizowane tweety. W odpowiedzi aplikacja pokaże informacje
o sentymencie znalezionych tweetów w postaci wykresów czytelnych dla użytkownika, a także przykładowe tweety.

### Zbiór danych
Aplikacja będzie korzystać z API Twittera w celu pobrania tweetów w języku polskim w zadanym okresie czasu.
Do analizy emocji związanych z wpisami użyjemy modelu dostępnego w Text Analytics API z Microsoft Azure.

### Stos technologiczny
1. Python 3.x:
    1. Flask — aplikacja webowa,
    2. biblioteki do analizy danych: pandas, seaborn, etc.
2. Svelte — framework JavaScript

## Funkcjonalności
1. Użytkownik podaje hasztag i okres, z jakiego mają pochodzić dane. 
Jako wynik dostaje:
    1. wykres kołowy, który pokazuje ilość tweetów pozytywnych, neutralnych i negatywnych — pokazujący ogólny sentyment,
    2. graficzną prezentację sentymentu w czasie w postaci wykresu liniowego,
    3. 3 przykładowe tweety dla danego hasztagu,
    4. po 3 przykładowe tweety dla danego sentymentu.

## Architektura projektu
Użyte narzędzia Microsoft Azure:
1. Azure App Service — hostowanie aplikacji,
2. Text Analytics API — analiza nacechowania emocjonalnego tweetów,
3. *(niewykorzystane)* Azure Table Storage — nierelacyjna baza danych, w której przechowywane będą przez pewien czas już 
przeanalizowane wpisy z przypisanym sentymentem (ostatecznie stwierdziliśmy, że funkcjonalność ta nie jest niezbędna
do realizacji celu biznesowego),
4. *(niewykorzystane)* Azure Functions — cykliczne wywoływanie skryptu czyszczącego bazę danych (jak wyżej)

![](https://github.com/monika-osiak/azure-ml/blob/main/resources/components.png)

## Implementacja
Projekt jest dostępny pod adresem: [link](https://pucflask2021.azurewebsites.net/)

Prezentacja projektu jest dostępna pod adresem: [link](https://drive.google.com/file/d/1sHTmbu_Y1tIp0-83wYWgelCp2V2yDt6Y/view?usp=sharing)

## Wnioski
* Podczas pracy nad projektem Microsoft dokonał znacznych zmian w usłudze Text Analytics. Dzięki jednolitemu interfejsowi nie odczuliśmy zmian i nie musieliśmy wprowadzać modyfikacji w kodzie. Microsoft udowodnił tym samym, ze potrafi wprowadzać znaczące modyfikacje do swoich serwisów, nie obciązając tym swoich uzytkowników.
* Jednocześnie niejasne są niektóe poradniki deployowania usług. Według większości proces powinien zająć około minuty, a zdażyło nam się, że dopiero po około 30 minut aplikacja była publicznie dostępna.
* Najwygodniej korzystać chyba z Azure CLI, zamiast robić wsyzstko z przeglądarkowego GUI, sam proces deploymentu wydaje się wówczas szybszy.
* Z usług Azure można korzystac również w lokalnych projektach, wystarczy endpoint oraz klucz do uruchomionej usługi, aby wykorzystać ją poprzez API, co bardzo ułatwia testy w środowisku developerskim.