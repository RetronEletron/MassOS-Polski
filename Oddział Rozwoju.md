# Oddział rozwoju MassOS
Ten artykuł dotyczy rozwoju MassOS i gałęzi rozwojowej MassOS.
# Jaka jest branża deweloperska?
Krótko mówiąc, gałąź rozwojowa jest gałęzią repozytorium MassOS, która zawiera zmiany dla następnej wersji MassOS, która jest obecnie w fazie rozwoju. Wszystkie nowe funkcje, aktualizacje i wkłady trafiają do gałęzi deweloperskiej. Gdy zostanie wydana następna wersja MassOS, zmiany w gałęzi rozwojowej zostają scalone w główną (stabilną) gałąź.
# Testowanie gałęzi deweloperskiej
Nie zalecamy używania kompilacji gałęzi deweloperskiej jako codziennego sterownika, jednak zachęcamy do przetestowania jej na zapasowym komputerze lub na maszynie wirtualnej.

W określonych momentach w procesie rozwoju (czas od rozpoczęcia rozwoju do wydania kolejnej wersji) tworzymy kompilacje rozwojowe MassOS do celów testowych. Te kompilacje są przechowywane w [tym folderze Dysku Google](https://go.thesonicmaster.net/qk9). Jeśli chcesz tylko przetestować kompilację gałęzi deweloperskiej i nie możesz (lub nie chcesz) samodzielnie zbudować MassOS, to są te, których możesz użyć.

**UWAGA:** Tylko tarballe rootfs są wydawane dla kompilacji deweloperskich. Jeśli chcesz utworzyć obraz ISO, musisz zrobić to ręcznie, używając skryptów w repozytorium [livecd-installer](https://github.com/MassOS-Linux/livecd-installer).

Dostarczone kompilacje deweloperskie są prawdopodobnie przez jakiś czas nieaktualne, ponieważ zmiany są dość często wrzucane do gałęzi deweloperskiej i nie jesteśmy w stanie utworzyć nowej wersji dla każdego zatwierdzenia. Dlatego zalecamy samodzielne [Building MassOS](https://github.com/MassOS-Linux/MassOS/wiki/Building-MassOS), jeśli chcesz mieć najnowsze zmiany. Ponieważ system budowania jest dużą częścią rozwoju MassOS, budując go samodzielnie, możesz również otwierać zgłoszenia, aby zgłaszać wszelkie występujące awarie, co bardzo nam pomoże.
# Aktualizacja do/z wersji rozwojowej
Jeśli używasz stabilnej wersji MassOS i chcesz zaktualizować ją do gałęzi deweloperskiej, możesz użyć narzędzia do aktualizacji i przekazać archiwum rootfs deweloperskie jako argument, w następujący sposób:
```
sudo ./massos-upgrade massos-development-rootfs-x86_64.tar.xz
```
Jeśli użyłeś skryptów w repozytorium [livecd-installer](https://github.com/MassOS-Linux/livecd-installer) do utworzenia obrazu ISO, możesz zaktualizować go za pomocą funkcji **Odśwież** instalatora. Zobacz [Aktualizacja MassOS]([livecd-installer](https://github.com/MassOS-Linux/MassOS/wiki/Upgrading-MassOS), aby uzyskać więcej informacji.

Jeśli korzystasz z kompilacji gałęzi deweloperskiej i chcesz uaktualnić do następnej stabilnej wersji, możesz jak zwykle postępować zgodnie z przewodnikiem [Upgrading MassOS](https://github.com/MassOS-Linux/MassOS/wiki/Upgrading-MassOS) . **Pamiętaj, że jeśli bieżąca stabilna wersja jest starsza niż twoja wersja rozwojowa, będzie to downgrade.**
