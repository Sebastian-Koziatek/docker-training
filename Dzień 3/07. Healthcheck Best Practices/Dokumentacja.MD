# Skrót zasad

1. Użyj Healtcheck, aby określić, czy Twój kontener jest wolny od zagrożeń.
2. Użyj instrukcji HEALTHCHECK w pliku Docker 
```docker
HEALTHCHECK –interval=5m –timeout=3s \
CMD curl -f http://localhost/ || exit 1
```

3. Upewnij się, że masz zdefiniowany działający CMD lub ENTRYPOINT
4. Nie używaj Shell form dla polecenia HEALTHCHECK
5. Sprawdź, czy aplikacja nasłuchuje na właściwym porcie
6. Przetestuj rzeczywistą funkcjonalność swojej aplikacji, a nie tylko jej proces
7. Unikaj używania curl, chyba że jest to absolutnie konieczne
8. Use –retries flag
9. Uruchom wiele poleceń za pomocą &&

<br>
</br>

# Docker Healthcheck Best Practices

1. Użyj Healtcheck, aby określić, czy Twój kontener jest wolny od zagrożeń.

Jeśli używasz kontenerów Docker w produkcji, ważne jest, aby móc określić stan swoich kontenerów. Kontrola zdrowia to świetny sposób, aby to zrobić. Domyślnie docker nie sprawdza kondycji twoich kontenerów. Oznacza to, że jeśli Twój kontener ulegnie awarii, nie otrzymasz powiadomienia.

Kontrola stanu pomoże Ci tego uniknąć, okresowo sprawdzając stan kontenerów i zgłaszając wszelkie problemy. W ten sposób możesz mieć pewność, że Twoje kontenery zawsze działają płynnie.

Istnieje wiele sposobów na skonfigurowanie kontroli stanu, ale najważniejsze jest upewnienie się, że obejmuje ona wszystkie ważne aspekty Twojego kontenera. Na przykład możesz chcieć sprawdzić stan swojej aplikacji, bazy danych i sieci.

Możesz również użyć kontroli stanu, aby monitorować zasoby swojego kontenera. Jest to szczególnie ważne, jeśli używasz wielu kontenerów na tym samym hoście. Monitorując zasoby swoich kontenerów, możesz mieć pewność, że nie przeciążają hosta.

Na koniec możesz użyć kontroli stanu, aby uruchomić akcję po wykryciu problemu. Na przykład możesz wysłać e-mail lub wiadomość Slack do zespołu. W ten sposób możesz mieć pewność, że ktoś zawsze jest świadomy problemu i może podjąć działania, aby go naprawić.

2. Użyj instrukcji HEALTHCHECK w pliku Docker

Instrukcja HEALTHCHECK mówi Dockerowi, jak przetestować kontener, aby sprawdzić, czy nadal działa. Jest to ważne, ponieważ nawet jeśli kontener działa, niekoniecznie oznacza to, że jest zdrowy.

Jeśli nie używasz instrukcji HEALTHCHECK, Docker nie ma możliwości sprawdzenia, czy Twój kontener jest zdrowy. Może to prowadzić do problemów w przyszłości, na przykład podczas próby skalowania aplikacji i tworzenia nowych kontenerów z niezdrowego obrazu.

Używając instrukcji HEALTHCHECK, musisz określić dwie rzeczy:
- Polecenie, które zostanie użyte do przetestowania kontenera
- Interwał, w jakim polecenie będzie uruchamiane

```docker
HEALTHCHECK –interval=5m –timeout=3s \
CMD curl -f http://localhost/ || exit 1
```

3. Upewnij się, że masz zdefiniowany działający CMD lub ENTRYPOINT

Jeśli nie masz zdefiniowanego działającego CMD lub ENTRYPOINT, Twój kontener nie będzie mógł się poprawnie uruchomić. Dzieje się tak, ponieważ silnik Dockera potrzebuje sposobu, aby wiedzieć, jakie polecenie wykonać podczas uruchamiania kontenera. Bez zdefiniowanego CMD lub ENTRYPOINT silnik Dockera nie będzie wiedział, co robić, a kontener nie uruchomi się.

Ponadto ważne jest, aby upewnić się, że CMD lub ENTRYPOINT jest rzeczywiście wykonywalny. Jeśli tak nie jest, kontener również się nie uruchomi.

Podsumowując, upewnienie się, że masz zdefiniowaną działającą CMD lub ENTRYPOINT, jest jedną z najważniejszych najlepszych praktyk w zakresie kontroli kondycji Dockera.


4. Nie używaj Shell form dla polecenia HEALTHCHECK

Shell form to - dla polecenia HEALTHCHECK to po prostu łańcuch znaków wykonywany przez powłokę. Oznacza to, że jeśli użyjesz formularza powłoki, a twoje polecenie się nie powiedzie, kontrola kondycji również się nie powiedzie.

Zamiast tego powinieneś użyć formularza exec, który wykonuje polecenie bezpośrednio, bez przechodzenia przez powłokę. W ten sposób, jeśli twoje polecenie się nie powiedzie, kontrola stanu niekoniecznie zakończy się niepowodzeniem.

Korzystanie z formularza exec ma również inne zalety. Na przykład możesz użyć wielu poleceń w formularzu exec, podczas gdy w formularzu powłoki możesz użyć tylko jednego polecenia.

Ogólnie rzecz biorąc, użycie formularza exec dla polecenia HEALTHCHECK jest znacznie lepszą praktyką niż użycie powłoki dla healthcheck. 

5. Sprawdź, czy aplikacja nasłuchuje na właściwym porcie

Jeśli Twoja aplikacja nie nasłuchuje na właściwym porcie, nie będzie mogła akceptować połączeń z innych kontenerów. Może to prowadzić do wielu problemów, takich jak:

– Twoja aplikacja nie będzie mogła obsługiwać żądań.
– Inne kontenery nie będą mogły połączyć się z Twoją aplikacją.
– Twoja aplikacja nie będzie mogła łączyć się z innymi kontenerami.

Sprawdzając, czy aplikacja nasłuchuje na właściwym porcie, można uniknąć tych problemów.

6. Przetestuj rzeczywistą funkcjonalność swojej aplikacji, a nie tylko jej proces

Kiedy testujesz tylko proces, nie sprawdzasz, czy aplikacja faktycznie robi to, co powinna. Załóżmy na przykład, że masz aplikację internetową, która musi połączyć się z bazą danych. Jeśli testujesz tylko proces aplikacji internetowej, nie będziesz wiedział, czy połączenie z bazą danych działa. Ale jeśli przetestujesz rzeczywistą funkcjonalność aplikacji, próbując uzyskać dostęp do bazy danych, będziesz w stanie stwierdzić, czy występują jakieś problemy.

Jest to ważne, ponieważ często zdarza się, że aplikacja działa dobrze, gdy jej proces się kończy, ale w rzeczywistości nie robi tego, co powinna. Testując rzeczywistą funkcjonalność aplikacji, możesz mieć pewność, że naprawdę działa tak, jak powinna.


7. Unikaj używania curl, chyba że jest to absolutnie konieczne

Curl to świetne narzędzie, ale nie jest przeznaczone do użytku w kontrolach stanu. Jest to narzędzie ogólnego przeznaczenia, które nie zostało stworzone z myślą o kontrolach stanu, więc nie ma żadnych funkcji, które czynią go idealnym do użycia w tym kontekście.

Na przykład curl nie ma wbudowanej logiki ponawiania prób, więc jeśli witryna nie działa lub reaguje wolno, curl będzie po prostu próbował, dopóki nie upłynie limit czasu. Może to powodować fałszywe alarmy w wynikach kontroli stanu, co może prowadzić do niepotrzebnych ponownych uruchomień lub, co gorsza, pominiętych awarii.

Ponadto curl nie radzi sobie dobrze z przekierowaniami. Jeśli witryna przekierowuje na inną stronę, curl podąży za przekierowaniem i zwróci zawartość nowej strony zamiast oryginalnej strony, którą próbujesz sprawdzić. Może to również prowadzić do fałszywych alarmów lub pominiętych awarii.

Wreszcie, curl jest domyślnie bardzo rozwlekły, więc wyświetli wiele informacji, których prawdopodobnie nie potrzebujesz podczas sprawdzania stanu. Może to zaśmiecać dzienniki i utrudniać debugowanie problemów.

Z tych powodów ogólnie najlepiej jest unikać zawijania w kontrolach kondycji dokera, chyba że jest to absolutnie konieczne. Istnieje wiele innych narzędzi zaprojektowanych specjalnie do użytku w kontrolach stanu, takich jak HTTPie, które w większości przypadków mogą być lepszym wyborem.

8. Używaj flag –interval i –timeout

Flaga –interval określa, jak często uruchamiane jest polecenie Healthcheck, a flaga –timeout określa, jak długo polecenie może działać, zanim zostanie uznane za niepowodzenie. Domyślnie flaga –interval jest ustawiona na 30 sekund, a flaga –timeout nie jest ustawiona, co oznacza, że polecenie Healthcheck nigdy nie przekroczy limitu czasu.

Ważne jest, aby używać obu tych flag, ponieważ dają one większą kontrolę nad tym, kiedy polecenie Healthcheck jest uważane za powodzenie lub niepowodzenie. Na przykład, jeśli masz polecenie sprawdzania kondycji, którego wykonanie zajmuje dużo czasu, możesz użyć flagi –timeout, aby upewnić się, że wykonanie polecenia nie zajmie zbyt dużo czasu.

Podobnie, jeśli masz polecenie sprawdzania kondycji, które czasami kończy się powodzeniem, możesz użyć flagi –interval, aby upewnić się, że polecenie jest uruchamiane częściej, aby szybciej wykrywać awarie.

9. Uruchom wiele poleceń za pomocą &&

Jeśli masz długo działające polecenie, które chcesz uruchomić w ramach kontroli stanu, ważne jest, aby dołączyć &&, aby następne polecenie w sekwencji było uruchamiane tylko wtedy, gdy poprzednie się powiedzie. Jest to szczególnie ważne w przypadku poleceń, które uruchamiają inne procesy, takie jak uruchamianie serwera WWW.

Załóżmy na przykład, że chcesz uruchomić serwer WWW w ramach kontroli stanu. Możesz pokusić się o zrobienie czegoś takiego:

CMD my_web_server.py

Jeśli jednak my_web_server.py nie uruchomi się z jakiegoś powodu, kontrola kondycji nigdy nie dojdzie do punktu, w którym będzie mogła sprawdzić stan serwera WWW. Dodając &&, możesz upewnić się, że drugie polecenie w sekwencji (w tym przypadku curl) działa tylko wtedy, gdy pierwsze powiedzie się:

CMD my_web_server.py && curl -f http://localhost:8080/

Jest to o wiele solidniejszy sposób na napisanie kontroli stanu i zaoszczędzi Ci wielu bólów głowy na drodze.

10. Użyj flagi –retries

Po uruchomieniu kontenera uruchamiane jest polecenie Healthcheck w celu określenia, czy kontener jest w dobrej kondycji. Jeśli polecenie Healthcheck zwróci niezerowy kod wyjścia, kontener jest uznawany za w złej kondycji i jest uruchamiany ponownie. Jednak czasami polecenie Healthcheck może zwrócić niezerowy kod zakończenia, nawet jeśli kontener jest w dobrej kondycji.

Na przykład, jeśli polecenie Healthcheck sprawdza plik, który nie został jeszcze utworzony, zwróci niezerowy kod wyjścia. W takim przypadku nie chcesz, aby kontener był ponownie uruchamiany, ponieważ jest właściwie zdrowy i czeka tylko na utworzenie pliku.

W tym miejscu pojawia się flaga –retries. Pozwala określić, ile razy polecenie Healthcheck może zwrócić niezerowy kod wyjścia, zanim kontener zostanie uznany za w złej kondycji i zostanie ponownie uruchomiony. Domyślnie flaga –retries jest ustawiona na 3, ale możesz ją zmienić na dowolną wartość.

Załóżmy na przykład, że masz kontener, na którym działa serwer WWW. Polecenie Healthcheck sprawdza istnienie pliku index.html. Gdy kontener jest uruchamiany po raz pierwszy, plik index.html jeszcze nie istnieje, więc polecenie Healthcheck zwraca niezerowy kod wyjścia. Jednak gdy serwer WWW utworzy plik index.html, polecenie Healthcheck zwróci zerowy kod wyjścia, a kontener zostanie uznany za zdrowy.

Jeśli nie użyłeś flagi –retries, kontener zostałby zrestartowany trzykrotnie, zanim zostałby uznany za sprawny. Jeśli jednak ustawisz flagę –retries na 5, kontener zostanie ponownie uruchomiony pięć razy, zanim zostanie uznany za sprawny.

Używanie flagi –retries jest najlepszą praktyką, ponieważ zapobiega niepotrzebnemu ponownemu uruchamianiu kontenerów.