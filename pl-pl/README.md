# Docker Cheat Sheet

**Chciałbyś pomóc w tworzeniu tego projektu? Zobacz sekcję [Wkład własny](#contributing)!**

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
"Z Dockerem, programiści są w stanie stworzyć dowolną aplikację, w dowolnym języku, używając dowolnego zestawu narzędzi. “Zdockeryzowane” aplikacje są w pełni przenośne i można je wszędzie odpalić - na laptopie z OS X, jak i z Windowsem, na serwerach QA działających na Ubuntu w chmurze, jak i data center z produkcji działający na VMkach z RadHatem.

Deweloperzy mogą szybko wystartować wraz z jedną, z ponad 13,000 aplikacji dostępnych na Docker Hubie. Docker zarządza i kontroluje zmiany i właściwości, ułatwiając administratorom systemu zrozumienie, jak działają aplikacje stowrzone przez deweloperów. Z Docker Hubem programiści mogą automatyzować pipeline-y oraz udostępniać artefakty wraz z wspracownikom poprzez publiczne i prywatne repozytoria.

Docker pomaga szybciej budować i dostarczać aplikacje o znacznie lepszej jakości." -- [What is Docker](https://www.docker.com/what-docker#copy1)

## Wstępne wymagania

Ja używam [Oh My Zsh](https://github.com/ohmyzsh/oh-my-zsh) razem z [Docker plugin](https://github.com/robbyrussell/oh-my-zsh/wiki/Plugins#docker) do autouzupełniania komend dockerowych. YMMV

### Linux

Kernel Linuxa 3.10.x jest [minimalnym wymogiem](https://docs.docker.com/engine/installation/binaries/#check-kernel-dependencies) żeby odpalić Dockera.

### MacOS

10.8 “Mountain Lion” lub nowszy.

### Windows 10

W BIOSie musimy włączyć opcję Hyper-V

Jeśli jest dostępne VT-D to również musi być włączone (Procesory Intela).

### Windows Server

Windows Server 2016 jest minimalnym wymogiem do zainstalowania dockera i docker-compose. Na tej wersji są jednak ograniczenia takie jak wielokrotne sieci wirtualne i kontenery Linuxa. Polecamy Windows Server 2019 i nowsze. 

