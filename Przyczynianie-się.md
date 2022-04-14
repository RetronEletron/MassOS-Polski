# Przyczynianie się
Z zadowoleniem przyjmujemy wszelkiego rodzaju wkłady do MassOS. Ten dokument wyjaśnia niektóre sposoby, w jakie możesz wnieść swój wkład w MassOS.
# Sposoby, w jakie możesz wnieść swój wkład
- Przenoszenie MassOS na inne architektury (np. i386, ARM).
- Przenoszenie dużego oprogramowania do MassOS, którego ludzie mogą chcieć, takiego jak alternatywne środowiska graficzne.
- Znajdowanie i zgłaszanie [problemów] (https://github.com/MassOS-Linux/MassOS/issues).
- Jeszcze lepiej: Naprawianie problemów zgłoszonych przez ludzi.
- Pisanie dokumentacji dla wiki MassOS.
- Tłumaczenie MassOS i dokumentacji na twój język ojczysty.

# Wytyczne dotyczące składek
Staraj się przestrzegać tych wskazówek, wnosząc wkład do MassOS:
## Ogólne wytyczne
- Wysyłaj tylko modyfikowanie i wysyłanie żądań ściągnięcia do **gałęzi deweloperskiej**. Nie będziemy akceptować pull requestów do głównego oddziału.

## Dodawanie oprogramowania do MassOS
Te wymagania dotyczą zarówno żądań funkcji, jak i żądań ściągnięcia:

- Jak stwierdzono w [licencji](https://github.com/MassOS-Linux/MassOS/blob/main/LICENSE), będziemy dodawać do MassOS tylko oprogramowanie, które jest albo w domenie publicznej, albo na licencji GPL- kompatybilna licencja. Nie będziemy akceptować niewolnych programów ani programów licencjonowanych na podstawie licencji niezgodnych z GPL, takich jak CDDL.
- Jako wyjątek od powyższego, niektóre niewolne oprogramowanie będą akceptowane. Przeczytaj [MassOS i wolne oprogramowanie](https://github.com/MassOS-Linux/MassOS/wiki/MassOS-and-Free-Software), aby uzyskać więcej informacji.
- Jeśli zmodyfikujesz system kompilacji w celu dodania oprogramowania, zapoznaj się z sekcją „System kompilacji” poniżej.

## Zbuduj system
Modyfikowanie systemu kompilacji to najlepszy sposób na dodanie dodatkowych pakietów do MassOS. System kompilacji składa się z następujących plików:

- `source-urls`: Zawiera źródłowe adresy URL archiwum tar.
- `retrieve-sources.sh`: Pobiera wszystkie paczki źródłowe z `source-urls`.
- `stage1.sh`: Buduje zestaw tymczasowych narzędzi ładowania początkowego, używanych później do zbudowania pełnego systemu MassOS. Powinno to być modyfikowane tylko w niezwykle rzadkich przypadkach.
- `stage2.sh`: Konfiguruje środowisko chroot, wywołuje `build-system.sh` i tworzy ostateczną paczkę rootfs.
- `build-system.sh`: Buduje pełny system MassOS w środowisku chroot. To jest główny skrypt, który będzie modyfikowany podczas dodawania dodatkowych pakietów.
- `patches`: Folder zawierający różne poprawki do pakietów.

Postępuj zgodnie z tymi wytycznymi podczas modyfikowania/dodawania do systemu kompilacji:

- Zachowaj źródłowe adresy URL w kolejności alfabetycznej zgodnie z nazwą pliku w `source-urls`.
- Nie używaj adresów URL pobierania Sourceforge; spróbuj znaleźć serwer lustrzany lub poproś nas ładnie o umieszczenie archiwum źródłowego na archive.massos.org.
- Nie umieszczaj łatek w `source-urls`; zamiast tego umieść je w katalogu `patches`.
- Umieść zewnętrzne jednostki systemd wymagane dla pakietów w katalogu `utils/systemd-units`. Zostaną one automatycznie skopiowane do katalogu systemowego w czasie kompilacji.
- W `build-system.sh`, buduj pakiety po wszystkich wymaganych przez nich zależnościach, a przed jakimkolwiek innym pakietem, dla którego mogą służyć jako opcjonalna zależność. Jeśli inny pakiet może używać twojego pakietu jako opcjonalnej zależności, ale domyślnie tego nie robi, rozważ zmodyfikowanie flag konfiguracji/kompilacji drugiego pakietu, aby mógł z niego korzystać.
- MassOS używa zunifikowanej struktury systemu plików (`/bin`, `/lib` i `/sbin` są połączone symbolicznie ze swoimi odpowiednikami użytkownika). Dlatego prefiksem instalacyjnym dla pakietów może być zawsze `/usr`.
- Niektóre dystrybucje łączą `sbin` z `bin` i `libexec` z `lib`. MassOS **nie**; `/usr/bin`, `/usr/sbin`, `/usr/lib` i `/usr/libexec` są oddzielnymi katalogami.
- Dwukrotne budowanie pakietów w celu obejścia zależności kołowych (nawet opcjonalnych) jest w porządku.
- Zawsze instaluj wszystkie pliki licencyjne zawarte w pakiecie. Zazwyczaj są one nazywane „KOPIOWANIE”, „LICENCJA” lub ich odmiany. Zainstaluj je po skompilowaniu za pomocą polecenia takiego jak `install -t /usr/share/licenses/<nazwa-pakietu> -Dm644 COPYING`.
- Preferuj Meson i CMake nad Autotools, jeśli pakiet obsługuje jeden z systemów kompilacji.
## Dokumentacja
Możesz wnieść swój wkład w tę wiki dokumentacji, ulepszając istniejące strony lub dodając dodatkowe strony, jednak postępuj zgodnie z następującymi wytycznymi:

- Używaj dobrego angielskiego - nie spodziewaj się błędów gramatycznych.
- Jeśli edytujesz istniejące strony, możesz dodać ulepszenia/ulepszenia, ale nie zmieniaj głównej treści.
- Jeśli dodajesz nowe strony, upewnij się, że dotyczą one własnego tematu, a nie tylko przeróbki tego, co zostało już wyjaśnione na innych stronach.
