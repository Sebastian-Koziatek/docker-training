# Skanowanie luk w zabezpieczeniach obrazów

Polecenie `docker scan` umożliwia skanowanie istniejących obrazów platformy Docker przy użyciu nazwy obrazu lub identyfikatora. Na przykład uruchom następujące polecenie, aby przeskanować obraz hello-world:
```bash
docker scan hello-world
```
Możesz uzyskać szczegółowy raport ze skanowania obrazu Dockera, dostarczając plik Dockerfile użyty do utworzenia obrazu. Składnia jest
```bash
docker scan --file Dockerfile <nazwa_obrazu>
```

Aby uzyskać więcej darmowych możliwości skanowania zarejestruj się poprzez użycie:

```bash
docker scan --file Dockerfile docker-scan:e2e
```

Więcej informacji na temat docker scan znajdziesz w dokumentacji - https://docs.docker.com/engine/scan/. 

Jeżeli posiadasz konto docker z planem conajmniej pro który oferuje aktualnie 300 skanów, to takie skany możesz przeprwoadzać bezpośrednio w aplikacji `docker desktop` oraz na przykład w swoim repozytorium

# Skanowanie za pomocą aplikacji firm trzecich

Uruchamiając docker desktop możemy przejść do "extensions" gdzie mamy dostęp do wielu rozszerzeń możliwości naszego dockera. Pobierz rozszerzenie o nazwie `trivy`. Rozszerzenie to pozwala również na skanowanie obrazów. 
<br>
</br>
# Docker - Najlepsze praktyki w zakresie bezpieczeństwa kontenerów platformy

When using Docker containers, you should use the following practices to ensure maximum security.
1. Unikaj uprawnień `roota`

Uruchamianie kontenera Dockera z uprawnieniami roota może być najłatwiejszym sposobem na jego prawidłowe działanie, ponieważ nie musisz zajmować się skomplikowanym zarządzaniem uprawnieniami. Istnieje jednak niewiele uzasadnień dla uruchamiania kontenerów jako root w środowisku produkcyjnym.

Kontenery Docker nie działają domyślnie jako root, więc nie musisz zmieniać domyślnej konfiguracji, ale powinieneś unikać nadawania uprawnień root. Korzystając z Kubernetes, możesz zwiększyć bezpieczeństwo, tworząc zasady bezpieczeństwa pod z dyrektywą MustRunAsNonRoot — to jawnie blokuje administratorom możliwość uruchamiania kontenerów z uprawnieniami roota.

2. Korzystaj z bezpiecznych `repozytorów`

Rejestry obrazów umożliwiają łatwe pobieranie obrazów kontenerów z centralnego repozytorium, co czyni je zarówno wygodnymi, jak i potencjalnie ryzykownymi. Z tego powodu lepiej jest trzymać się zaufanych rejestrów, takich jak Docker Trusted Registry. Powinieneś ocenić bezpieczeństwo każdego używanego rejestru i zainstalować go za zaporą ogniową, aby chronić się przed naruszeniami internetowymi.

Nie pozwól nikomu pobierać ani przesyłać obrazów kontenerów z Twojego rejestru. Zaimplementuj kontrolę dostępu opartą na rolach (RBAC), aby określić, którzy użytkownicy mogą uzyskiwać dostęp do jakich zasobów, i zablokuj dostęp innym osobom. Zarządzanie i konfigurowanie reguł dostępu dla nowych użytkowników może być niewygodne, ale może pomóc w zapobieganiu naruszeniom rejestru.


3. Ogranicz `zużycie zasobów`

Docker umożliwia ustawienie limitów zasobów dla każdego kontenera, ograniczając jego zużycie pamięci i zasobów procesora. Przydziały zasobów pomagają utrzymać wydajność środowiska Docker, zapobiegając zużywaniu zbyt wielu zasobów systemowych przez pojedynczy kontener. Zwiększają również bezpieczeństwo, blokując ataki, które mają na celu zakłócenie twoich usług poprzez blokowanie dużej liczby zasobów.

4. Zeskanuj `swoje obrazy`
   
Dbaj o bezpieczeństwo obrazów platformy Docker dzięki regularnym skanom pod kątem luk w zabezpieczeniach. Ważne jest, aby skanować wszystkie obrazy podczas ich pobierania i kontynuować skanowanie w celu zmniejszenia ekspozycji. Okresowe skanowanie pozwala aktualizować obrazy i kontrolować krytyczne katalogi i pliki.

5. Zbuduj `swoje sieci`

Kontenery Docker używają sieci do komunikowania się ze sobą. Ta komunikacja umożliwia prawidłowe działanie kontenerów, ale wymaga również odpowiedniego monitorowania i polityki bezpieczeństwa. Należy zaprojektować te zasoby, aby ułatwić monitorowanie i umożliwić szybkie blokowanie naruszeń.

6.  `Monitorowanie` kontenerów Dockera

Monitorowanie jest istotną częścią każdej strategii bezpieczeństwa. W aplikacji kontenerowej monitorowanie może być trudne ze względu na dużą liczbę ruchomych części i użycie niezmiennych komponentów. Wyspecjalizowane narzędzia do monitorowania kontenerów mogą pomóc w uzyskaniu wglądu i możliwości obserwowania obciążeń kontenerowych.