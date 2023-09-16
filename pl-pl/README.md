# Docker Cheat Sheet

**Chciałbyś pomóc w tworzeniu tego projektu? Zobacz sekcję [Wkład własny](#contributing)!**

## Spis treści

* [Dlaczego Docker](#why-docker)
* [Wstępne wymagania](#prerequisites)
* [Instalacja](#installation)
* [Containers](#containers)
* [Images](#images)
* [Networks](#networks)
* [Registry and Repository](#registry--repository)
* [Dockerfile](#dockerfile)
* [Layers](#layers)
* [Links](#links)
* [Volumes](#volumes)
* [Exposing Ports](#exposing-ports)
* [Praktyki mistrzów](#best-practices)
* [Docker-Compose](#docker-compose)
* [Bezpieczeństwo](#security)
* [Rady](#tips)
* [Wkład własny](#contributing)

## Dlaczego Docker
"Z Dockerem, programiści są w stanie stworzyć dowolną aplikację, w dowolnym języku, używając dowolnego zestawu narzędzi. “Zdockeryzowane” aplikacje są w pełni przenośne i można je wszędzie odpalić - na laptopie z OS X, jak i z Windowsem, na serwerach QA działających na Ubuntu w chmurze, jak i data center z produkcji działający na VMkach z RadHatem.

Deweloperzy mogą szybko wystartować wraz z jedną, z ponad 13,000 aplikacji dostępnych na Docker Hubie. Docker zarządza i kontroluje zmiany i właściwości, ułatwiając administratorom systemu zrozumienie, jak działają aplikacje stowrzone przez deweloperów. Z Docker Hubem programiści mogą automatyzować pipeline-y oraz udostępniać artefakty wraz z wspracownikom poprzez publiczne i prywatne repozytoria.

Docker pomaga szybciej budować i dostarczać aplikacje o znacznie lepszej jakości." -- [What is Docker](https://www.docker.com/what-docker#copy1)

## Wstępne wymagania

Ja używam [Oh My Zsh](https://github.com/ohmyzsh/oh-my-zsh) razem z [Docker plugin](https://github.com/robbyrussell/oh-my-zsh/wiki/Plugins#docker) do autouzupełniania komend dockerowych. YMMV

### Linux

Kernel Linuxa 3.10.x jest [minimalnym wymogiem](https://docs.docker.com/engine/installation/binaries/#check-kernel-dependencies) żeby odpalić Dockera.

### MacOS

10.8 “Mountain Lion” lub nowszy.

### Windows 10

W BIOSie musimy włączyć opcję Hyper-V

Jeśli jest dostępne VT-D to również musi być włączone (Procesory Intela).

### Windows Server

Windows Server 2016 jest minimalnym wymogiem do zainstalowania dockera i docker-compose. Na tej wersji są jednak ograniczenia takie jak wielokrotne sieci wirtualne i kontenery Linuxa. Polecamy Windows Server 2019 i nowsze. 

## Instalacja 

### Linux

Odpal ten prosty i szybki skrypt instalacyjny stworzony przez Dockera:

```sh
curl -sSL https://get.docker.com/ | sh
```

Jeśli nie jesteś skłonny do odpalenia losowej komendy, zobacz [instrukcję instalacji](https://docs.docker.com/engine/installation/linux/) dla twojej dystrybucji Linuxa.

Jeśli jesteś początkującym w temacie Dockera, powinieneś w tym momencie przejrzeć [wstęp do Dockera](https://docs.docker.com/engine/getstarted/).

### macOS

Pobierz i zainstaluj [Docker Community Edition](https://www.docker.com/community-edition). if you have Homebrew-Cask, just type `brew install --cask docker`. lub Pobierz i zainstaluj [Docker Toolbox](https://docs.docker.com/toolbox/overview/).  [Docker For Mac](https://docs.docker.com/docker-for-mac/) jest spoko, ale nie jest jeszcze tak wykończony jak instalacje poprzez VirtualBoxa. [Zobacz porównanie](https://docs.docker.com/docker-for-mac/docker-toolbox/).

> **UWAGA** Docker Toolbox jest starszą wersją. Powinieneś używać Docker Community Edition, Zobacz [Docker Toolbox](https://docs.docker.com/toolbox/overview/).

Gdy już zainstalujesz Docker Community Edition, kliknij ikonę dockera na Launchpadzie, a następnie odpal kontener.

```sh
docker run hello-world
```

To tyle, masz działający kontener Dockera.

Jeśli jesteś początkującym w temacie Dockera, powinieneś w tym momencie przejrzeć [wstęp do Dockera](https://docs.docker.com/engine/getstarted/).

### Windows 10

Instalację Docker Desktop dla Windowsa możesz znaleźć [tutaj](https://docs.docker.com/desktop/windows/install/)

Gdy się zainstaluje, otwórz powershell jako administrator i wpisz komendy:

```powershell
# Wyświetl versję zainstalowanego Dockera:
docker version

# zpulluj, stwórz i załącz 'hello-world':
docker run hello-world
```

Żeby kontynuować z nauką z tego cheet sheetu, kilknij prawym przyciskiem myszy na ikonę Dockera na pasku zadań i otówrz ustawienia. W celu zamontoawania dysku, dysk C:/ musi być włączony w ustawieniach, aby informacje na jego temat były przekazywane do kontenerów (będą omówione później).

Żeby zmieniać między kontenerami na Windowsie i na Linuxie, kliknij prawym przyciskiem myszy ikonę na pasku zadań i wciśnij przycisk do zmiany systemu operacyjnego. Zrobienie tego zatrzyma aktualne działające kontenery i zrobi je niedostępne do czasu ponownej zmiany systemu.

Dodatkowo, jeśli masz zainstalowany na komputerze WSL lub WSL2, możesz chcieć mieć zainstalowany Kernel Linuxa dla systemu Windows. Jak to zrobić możesz znaleźć [tutaj](https://techcommunity.microsoft.com/t5/windows-dev-appconsult/using-wsl2-in-a-docker-linux-container-on-windows-to-run-a/ba-p/1482133). To wymaga zainstalowanego WSL-a. To umożliwi kontenerom bycie dostępnymi dla systemu operacyjnego WSL, jak również podniesie to ich efektywność uzyskaną z odpalania WSL-a w Dockerze. Polecane jest również używanie do tego aplikacji [Windows terminal](https://docs.microsoft.com/en-us/windows/terminal/get-started).

### Windows Server 2016 / 2019
Idź za instrukcją Microsoftu, która może być znaleziona [tutaj](https://docs.microsoft.com/en-us/virtualization/windowscontainers/deploy-containers/deploy-containers-on-server#install-docker)

Jeśli używasz najnowszej wersji z 2019 roku, przygotuj się, że będziesz pracował tylko w powershell-u, ponieważ nie ma aplikacji okienkowej. Po włączeniu komputera i zalogowaniu, włącz od razu okno powershella. Poleca się instalację edytora teksu oraz innych narzędzi używając [Chocolatey](https://chocolatey.org/install).

Po zainstalowaniu, powinny działać poniższe komendy:

```powershell
# Wyświetl versję zainstalowanego Dockera:
docker version

# zpulluj, stwórz i załącz 'hello-world':
docker run hello-world
```

Na systemie Windows Server 2016 nie ma możliwości włączenia obrazów Linuxa.

Na systemie Windows Server Build 2004 jest możliwość włączenia kontenerów Windowsa jak i Linuxa w tym samym czasie poprzez izolację Hyper-V. Gdy uruchamiasz kontenery, używaj flagi `--isolation=hyperv`, która zaizoluje kontener, używając osobnej instancji kernela/ 

### Sprawdź wersję
To bardzo ważne, żeby zawsze wiedzieć, z której wersji Dockera, w danym momencie korzystasz. Jest to bardzo pomocne, ponieważ wiesz, jakie cechy ma dana wersja. Dodatkowo wiesz, które kontenery z Docker store możesz odpalić, gdy próbujesz pobrać szablony kontenerów. Sprawdźmy jak poznać, z której wersji Dockera aktualnie korzystamy.

* [`docker version`](https://docs.docker.com/engine/reference/commandline/version/) pokazuje nam, z której wersji Dockera aktualnie korzystasz/

Żeby zobaczyć wersję na serwer:
```console
$ docker version --format '{{.Server.Version}}'
1.8.0
```

Możesz również wypisać wynik w formacie JSON:
```console
$ docker version --format '{{json .}}'
{"Client":{"Version":"1.8.0","ApiVersion":"1.20","GitCommit":"f5bae0a","GoVersion":"go1.4.2","Os":"linux","Arch":"am"}
```

## Containers (kontenery)

[Twoje podstawowe zaizolowane procesy Dockera](http://etherealmind.com/basics-docker-containers-hypervisors-coreos/). Kontenery są dla maszyn wirtualnych tym co wątki dla procesów. Możesz też o nich myśleć jako chrooty na sterydach.

### Cykl życia
* [`docker create`](https://docs.docker.com/engine/reference/commandline/create) tworzy kontener, ale go nie startuje.
* [`docker rename`](https://docs.docker.com/engine/reference/commandline/rename/) pozwala na zmianę nazwy kontenera.
* [`docker run`](https://docs.docker.com/engine/reference/commandline/run) tworzy i startuje kontener w jednej operacji.
* [`docker rm`](https://docs.docker.com/engine/reference/commandline/rm) usuwa kontener.
* [`docker update`](https://docs.docker.com/engine/reference/commandline/update/) aktualizuje limity zasobów w kontenerze.

Standardowo, gdy uruchamiamy kontener bez żadnych opcji, wystartuje i zatrzyma się chwilę później, chyba że użyjesz komendy `docker run -td container_id`. Komenda ta używa flagi `-t`, która alokuje sesję pseudo-TTY oraz flagi `-d`, która pozwala na uruchomienie kontenera w trybie "detach", czyli uruchomi go w tle i wypisze id kontenera.

Jeśli chciałbyś stworzyć kontener tymczasowy, użyj komendy `docker run --rm`, która usunie kontener zaraz po jego zatrzymaniu.

Jeśli chciałbyś, żeby jakiś folder z twojego komputera był dostępny również w kontenerze, możesz to zrobić używając komendy `docker run -v $HOSTDIR:$DOCKERDIR`. Jeśli chcesz, dowiedzieć się o tym więcej zobacz rozdział [Volumes](https://github.com/wsargent/docker-cheat-sheet/#volumes).

Jeśli chcesz, żeby poza kontenerem usunął się również volume, który mu przeznaczyłeś, musisz do komendy usuwania, dodać flagę `-v` - `docker rm -v`.

Możesz również używać [sterownika do logowania](https://docs.docker.com/engine/admin/logging/overview/) do każdego kontenera od wersji dockera 1.10. Żeby skonfigurować niestandardowy sterownik do logów (np. syslog), użyj komendy `docker run --log-driver=syslog`.

Kolejną pożyteczną opcją jest `docker run --name twoja_nazwa docker_image`, ponieważ jeśli ustawisz flagę `--name` w komendzie `docker run` pozwoli Ci to na uruchamianie i zatrzymywanie kontenera po wpisanej nazwie, a nie po jego ID.

### Uruchamianie i zatrzymywanie 

* [`docker start`](https://docs.docker.com/engine/reference/commandline/start) uruchom kontener.
* [`docker stop`](https://docs.docker.com/engine/reference/commandline/stop) zatrzymaj kontener.
* [`docker restart`](https://docs.docker.com/engine/reference/commandline/restart) zatrzymaj i uruchom kontener.
* [`docker pause`](https://docs.docker.com/engine/reference/commandline/pause/) zapauzuj uruchomiony kontener, "zamraża" go w miejscu.
* [`docker unpause`](https://docs.docker.com/engine/reference/commandline/unpause/) odpauzuj kontener.
* [`docker wait`](https://docs.docker.com/engine/reference/commandline/wait) zablokuj dopóki uruchomiony kontener się nie zastopuje.
* [`docker kill`](https://docs.docker.com/engine/reference/commandline/kill) wyślij sygnał SIGKILL do uruchomionego kontenera.
* [`docker attach`](https://docs.docker.com/engine/reference/commandline/attach) połącz się z uruchomionym kontenerem.

Jeśli chcesz pozostawić kontener w trybie "detach" to będąc w nim użyj `Ctrl+p, Ctrl+q`.
Jeśli chcesz zintegrować kontener z [host process managerem](https://docs.docker.com/engine/admin/host_integration/), wystartuj daemon z flagą `-r=false`, a później użyj `docker start -a`.

Jeśli chcesz wystawić porty kontenera poprzez hosta, zobacz sekcję [exposing ports](#exposing-ports).

Zasady ponownego uruchamiania w przypadku uszkodzonych instancji dockera są [omówione tutaj](http://container42.com/2014/09/30/docker-restart-policies/).

#### Ograniczanie CPU 

Możesz ograniczyć liczbę przekazanych rdzeni CPU do kontenera, używając procentu wszystkich rdzeni lub wpisując wybrane rdzenie.

Dla przykładu możesz przekazać flagę [`cpu-shares`](https://docs.docker.com/engine/reference/run/#/cpu-share-constraint). Ta flaga może być trochę dziwna -- 1024 oznacza 100% możliwych rdzeni w procesorze, więc gdy chcesz żeby kontener miał tylko 50% wszystkich to powinieneś mu dać wartość 512. Zobacz <https://goldmann.pl/blog/2014/09/11/resource-management-in-docker/#_cpu> po więcej informacji

```sh
docker run -it -c 512 agileek/cpuset-test
```

Poza tym możesz wybrać których rdzeni chcesz używać używając flagi [`cpuset-cpus`](https://docs.docker.com/engine/reference/run/#/cpuset-constraint). Zobacz <https://agileek.github.io/docker/2014/08/06/docker-cpuset/>, żeby zobaczyć szczegóły oraz fajne filmy:

```sh
docker run -it --cpuset-cpus=0,4,6 agileek/cpuset-test
```

Uwaga: Docker dalej będzie widział **wszystkie** twoje rdzenie w kontenerze -- po prostu nie będzie ich używał. Zobacz <https://github.com/docker/docker/issues/20770> po więcej szczegółów.

#### Ograniczenia pamięci 

W Dockerze możesz również tworzyć [ograniczenia pamięci](https://docs.docker.com/engine/reference/run/#/user-memory-constraints):

```sh
docker run -it -m 300M ubuntu:14.04 /bin/bash
```

#### Możliwości

Możliwości systemu Linux można dodawać za pomocą flag `cap-add` oraz `cap-drop`. Zobacz <https://docs.docker.com/engine/reference/run/#/runtime-privilege-and-linux-capabilities> po więcej informacji. Powinieneś tego używać, w celu zwiększenia bezpieczeństwa.

Żeby zamontować system plików bazowany na FUSE, musisz użyć zarówno flag `--cap-add`, jak i `--device`:

```sh
docker run --rm -it --cap-add SYS_ADMIN --device /dev/fuse sshfs
```

Przyznaj dostęp do pojedyńczego urządzenia:

```sh
docker run -it --device=/dev/ttyUSB0 debian bash
```

Przyznaj dostęp do wszystkich urządzeń:

```sh
docker run -it --privileged -v /dev/bus/usb:/dev/bus/usb debian bash
```

Więcej informacji na temat uprzywilejowanych kontenerów możesz znaleźć [tutaj](https://docs.docker.com/engine/reference/run/#runtime-privilege-and-linux-capabilities).

### Info na temat kontenerów

* [`docker ps`](https://docs.docker.com/engine/reference/commandline/ps) wypisuje działające kontenery.
* [`docker logs`](https://docs.docker.com/engine/reference/commandline/logs) pobiera logi z kontenera.  (Możesz używać własnego starownika logów, ale ta komenda jest jedynie dostępna dla `json-file` i `journald` w wersji 1.10).
* [`docker inspect`](https://docs.docker.com/engine/reference/commandline/inspect) wypisuje wszystkie informacje o kontenerze (włączając aresy IP).
* [`docker events`](https://docs.docker.com/engine/reference/commandline/events) pobiera zdarzenia z kontenera w czasie rzeczywistym.
* [`docker port`](https://docs.docker.com/engine/reference/commandline/port) wypisuje publiczny port kontenera.
* [`docker top`](https://docs.docker.com/engine/reference/commandline/top) wypisuje aktualnie działający proces w kontenerze.
* [`docker stats`](https://docs.docker.com/engine/reference/commandline/stats) pokazuje statystyki zużycia zasobów poprzez działające kontenery.
* [`docker diff`](https://docs.docker.com/engine/reference/commandline/diff) wypisuje zmienione pliki w systemie plików kontenera.

`docker ps -a` wypisuje wszystkie kontenery (te zastopowane też).

`docker stats --all` wypisuje listę wszystkich kontenerów.

### Importowanie / Eksportowanie

* [`docker cp`](https://docs.docker.com/engine/reference/commandline/cp) kopiuje pliki lub foldery między kontenerem a lokalnym systemem plików.
* [`docker export`](https://docs.docker.com/engine/reference/commandline/export) tworzy w STDOUT plik archiwizujący typu `.tar` z system plików kontenera.

### Wywoływanie komend

* [`docker exec`](https://docs.docker.com/engine/reference/commandline/exec) żeby wywołać komendę w kontenerze.

Żeby wejść do działającego kontenera, dołącz nowy proces powłoki do działającego kontenera nazwanego foo, użyj: `docker exec -it foo /bin/bash`

## Images

Obrazy są po prostu [szablonami dla kontenerów](https://docs.docker.com/engine/understanding-docker/#how-does-a-docker-image-work).

### Cykl życia

* [`docker images`](https://docs.docker.com/engine/reference/commandline/images) pokazuje wszystkie obrazy.
* [`docker import`](https://docs.docker.com/engine/reference/commandline/import) tworzy obraz z pliku `.tar`.
* [`docker build`](https://docs.docker.com/engine/reference/commandline/build) tworzy obraz z Dockerfile.
* [`docker commit`](https://docs.docker.com/engine/reference/commandline/commit) tworzy obraz z kontenera, pauzując go chwilowo, jeśli działa.
* [`docker rmi`](https://docs.docker.com/engine/reference/commandline/rmi) usuwa obraz.
* [`docker load`](https://docs.docker.com/engine/reference/commandline/load) ładuje obraz z archiwum typu `.tar` jako STDIN, zarówno obrazy, jak i tagi.
* [`docker save`](https://docs.docker.com/engine/reference/commandline/save) zapisuje obraz jako archiwum typu `.tar`, jako potok STDOUT ze wszystkimi warstwami, tagami i wersjami.

### Informacje

* [`docker history`](https://docs.docker.com/engine/reference/commandline/history) pokazuje historię obrazu.
* [`docker tag`](https://docs.docker.com/engine/reference/commandline/tag) przypisuje nazwę do obrazu (lokalnego lub w rejestrze).

### Czyszczenie

Możesz używać komendy `docker rmi` do usunięcia specyficznego obrazu, ale jest narzędzie [docker-gc](https://github.com/spotify/docker-gc), które bezpicznie oczyści obrazy, które nie są używane przez żaden z kontenerów. Dodane w dockerze 1.13 `docker image prune` również umożliwia usuwanie nieużywanych obrazów. Zobacz [Prune](#prune).

### Załaduj/Zapisz obraz

Załaduj obraz z pliku:

```sh
docker load < my_image.tar.gz
```

Zapisz istniejący obraz:

```sh
docker save my_image:my_tag | gzip > my_image.tar.gz
```

### Importuj/Eksportuj kontener

Importuj kontener jako obraz z pliku:

```sh
cat my_container.tar.gz | docker import - my_image:my_tag
```

Eksportuj istniejący kontener:

```sh
docker export my_container | gzip > my_container.tar.gz
```

### Różnica między ładowaniem zapisanego obrazu a importowaniem i eksportowaniem kontenera jako obraz

Ładowanie obrazu używając komendy `load` tworzy nowy obraz wraz z jego historią.
Importowanie kontenera jako obraz używając komendy `import` tworzy nowy obraz bez historii, co skutkuje mniejszym rozmiarem obrazu w porównaniu do załadowania obrazu.

## Layers

Wersjonowany system plików w Dockerze opiera się na warstwach. Warstwy są jak [commity w git'cie albo jak changesety dla systemów plików](https://docs.docker.com/engine/userguide/storagedriver/imagesandcontainers/).

