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
