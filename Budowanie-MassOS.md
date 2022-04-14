# Budowanie MassOS
Ten przewodnik pokazuje, jak używać skryptów w tym repozytorium do kompilowania MassOS ze źródeł.

Ten przewodnik jest skierowany do **programistów** (patrz poniżej). Większość użytkowników nie będzie chciała samodzielnie kompilować MassOS. Jeśli nie jesteś programistą i chcesz normalnie zainstalować MassOS, spójrz na [Instalowanie MassOS](https://github.com/MassOS-Linux/MassOS/wiki/Installing-MassOS).
# BARDZO WAŻNE uwagi dotyczące budowania MassOS
Przeczytaj wszystkie **uważnie** przed podjęciem decyzji o skompilowaniu MassOS:
- Budowanie MassOS zajmuje **bardzo dużo czasu**, a szybkość będzie się znacznie różnić w zależności od procesora. Kompilacja MassOS dla najszybszych procesorów zajmie kilka godzin, jednak kompilacja może zająć **kilka dni** na wolniejszych systemach. Chociaż nie możemy określić czasu, jaki zajmie każdy procesor, oto przykład tego, ile czasu zajęło różnym testowanym procesorom skompilowanie MassOS (wszystkie są szacunkowe):
  - Intel Core i5-4300U (2,9 GHz, 4 wątki): **~13 godzin**
  - Intel Core i5-1035G4 (3,7 GHz, 8 wątków): **~7 godzin**
  - Intel Xeon E-2286G (4,9 GHz, 12 wątków): **~4 godziny**
- Minimalna ilość pamięci RAM potrzebna do skompilowania MassOS zależy od tego, ile wątków ma twój procesor. Jako ogólną wytyczną, powinieneś mieć dwa razy więcej pamięci RAM niż wątków procesora. Na przykład, jeśli masz procesor z 4 wątkami, powinieneś mieć co najmniej 8 GB pamięci RAM. Uruchom `nproc` jeśli nie jesteś pewien ile wątków ma twój procesor. Przestrzeń wymiany może być używana jako alternatywa dla pamięci RAM, jednak proces kompilacji może zostać znacznie spowolniony, jeśli maszyna będzie musiała się zamienić.
- Druga część kompilacji (Etap 2) musi być uruchamiana z **dostępem root**, ponieważ używa środowiska chroot. Alternatywy dla chroot w przestrzeni użytkownika, takie jak PRoot, nie są obecnie obsługiwane.
- MassOS musi być zbudowany z istniejącej instalacji systemu GNU/Linux. Każda nowoczesna dystrybucja z podstawowymi narzędziami do budowania powinna być obsługiwana, jednak zalecamy budowanie z samego MassOS, aby uzyskać najczystszą możliwą kompilację. Chociaż system budowania wymaga ładowania początkowego w celu zapewnienia niezawodności, budowanie z MassOS zapewnia, że ​​nie ma absolutnie żadnego możliwego wpływu zewnętrznego z innej dystrybucji.

# Jak zbudować MassOS
Teraz, gdy przeczytałeś wszystkie uwagi i jesteś w pakowaniu tego, co się możesz, możesz wbudować budowanie MassOS.
### Sklonuj repozytorium
Aby zbudować MassOS, musisz najpierw sklonować lokalnie repozytorium GitHub. Aby to zrobić, uruchom następujące polecenia:
```
git clone https://github.com/TheSonicMaster/MassOS.git
cd MassOS
```
Spowoduje to pobranie stabilnej (obecnie wydanej) wersji MassOS. Jest to zalecane dla początkujących budowniczych, jednak jeśli jesteś programistą, możesz chcieć budować z **gałęzi deweloperskiej**, która zawiera niepublikowane zmiany, które są obecnie opracowywane dla następnej wersji MassOS. Jeśli (i **tylko jeśli**) chcesz skompilować wersję rozwojową, przejdź do gałęzi rozwojowej za pomocą następującego polecenia:
```
git checkout development
```
**UWAGA:** Gałąź deweloperska jest NIESTABILNA i nie ma gwarancji, że w ogóle się pomyślnie skompiluje. Jeśli jesteś twórcą po raz pierwszy, zalecamy najpierw zbudowanie stabilnej wersji. Jeśli napotkasz problem z kompilacją w gałęzi deweloperskiej, [otwórz problem](https://github.com/MassOS-Linux/MassOS/issues), aby go zgłosić.
### Pobierz źródła:
Zanim będziesz mógł rozpocząć proces kompilacji MassOS, musisz najpierw pobrać tarballe źródłowe całego oprogramowania, które tworzy system MassOS. Na szczęście to polecenie automatycznie pobierze je wszystkie za Ciebie:
```
./retrieve-sources.sh
```
**UWAGA:** Upewnij się, że wszystko zostało pomyślnie pobrane. Jeśli ostatni komunikat mówi coś w stylu „Niektóre pobierania mogły się nie powieść”, NIE WOLNO kontynuować, dopóki wszystkie pobrania nie powiodły się. Możesz ponownie uruchomić skrypt retrieve-sources.sh, aby ponowić próbę pobrania brakujących plików (nie spowoduje to ponownego pobrania niczego, co zostało już pomyślnie pobrane). Czasami pobieranie kończy się niepowodzeniem, ponieważ serwer hostujący archiwum źródłowe nie działa. W takim przypadku często możesz edytować niewłaściwy adres URL w pliku `source-urls` na serwer lustrzany, który również obsługuje ten sam plik.
### Zbuduj tymczasowy system ładowania początkowego:
Pierwsza część procesu budowania polega na zbudowaniu zestawu tymczasowych narzędzi ładowania początkowego. Musi to być zrobione w systemie hosta, ponieważ aby skompilować kompilator, potrzebujesz kompilatora. Tymczasowe narzędzia zbudowane na tym etapie mogą być następnie użyte do zbudowania pełnego systemu MassOS. Zbuduj tymczasowe narzędzia za pomocą następującego polecenia:
```
./stage1.sh
```
### Zbuduj pełny system MassOS:
Teraz tymczasowe narzędzia zostały pomyślnie zbudowane, możesz przystąpić do budowy pełnego systemu MassOS. To musi być uruchomione jako root, ponieważ korzysta ze środowiska chroot:
```
sudo ./stage2.sh
```
### Kończąc:
Kiedy system MassOS jest całkowicie skompilowany i ukończony, zostanie utworzony archiwum wyjściowe oznaczone jako `massos-<WERSJA>-rootfs-x86_64.tar.xz`. ** Zdecydowanie zaleca się zmianę właściciela końcowego archiwum wyjściowego na pierwotnego użytkownika. Możesz to zrobić za pomocą następującego polecenia:**
```
sudo chown $(whoami):$(whoami) massos-$(cat utils/massos-release)-rootfs-x86_64.tar.xz
```
Jeśli chcesz utworzyć obraz ISO Live CD dla swojego archiwum rootfs, zobacz repozytorium [livecd-installer](https://github.com/MassOS-Linux/livecd-installer), które zawiera instrukcje, jak to zrobić.
