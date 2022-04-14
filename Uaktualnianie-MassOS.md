# Ręczna instalacja MassOS
MassOS ma teraz obraz ISO Live CD, który pozwala wypróbować MassOS i łatwiej go zainstalować. Jeśli wolisz zainstalować go ręcznie, na tej stronie znajdziesz starsze instrukcje, jak to zrobić.
## Pobieranie systemu MassOS Rootfs
Najnowszą wersję MassOS można pobrać ze strony [Wydania](https://github.com/MassOS-Linux/MassOS/releases).

Upewnij się, że pobierasz plik rootfs tar.xz, a nie plik ISO, jeśli chcesz zainstalować go ręcznie.

Jeśli chcesz zainstalować kompilację gałęzi deweloperskiej, możesz pobrać rzadkie kompilacje z [tego folderu Dysku Google](https://go.thesonicmaster.net/qk9) lub [samodzielnie zbudować MassOS](building.md), jeśli te kompilacje są nieaktualne.
## Partycjonowanie dysku
Jak każdy inny system operacyjny, MassOS musi być zainstalowany na partycji. Obecnie obsługiwane są tylko systemy plików EXT4, BTRFS i XFS, a przetestowano tylko EXT4.

Musisz mieć główny system plików o wielkości co najmniej 8 GB i musi być sformatowany jako EXT4, BTRFS lub XFS.

Opcjonalnie możesz mieć partycję Swap. Działa to jako kopia zapasowa, aby zapobiec awariom systemu, jeśli zabraknie mu pamięci.

Jeśli używasz UEFI, musisz także mieć małą partycję systemową EFI do przechowywania plików programu ładującego UEFI. Partycja systemowa EFI musi być w systemie FAT32. Jeśli inny system operacyjny używa istniejącej partycji EFI, MassOS może jej również użyć, bez konfliktów i nie trzeba jej formatować.

Możesz partycjonować swoje dyski za pomocą edytora partycji wiersza poleceń, takiego jak `fdisk` lub edytora partycji GUI, takiego jak Gparted.
## Formatowanie partycji
Aby sformatować główny system plików, uruchom następujące polecenie, zastępując `XY` aktualną partycją, na przykład `a1` dla `/dev/sda1`:
```
sudo mkfs.ext4 /dev/sdXY
```
Jeśli zdecydujesz się utworzyć partycję wymiany, sformatuj również to:
```
sudo mkswap /dev/sdXY
```
Jeśli używasz UEFI i nie masz istniejącej partycji systemowej EFI, sformatuj nową:
```
sudo mkfs.fat -F32 /dev/sdXY
```
**NIE uruchamiaj tego polecenia, jeśli masz istniejący ESP używany przez inny system operacyjny!**
## Montaż przegród
Utwórz czysty katalog dla partycji MassOS, która ma zostać zamontowana:
```
sudo mkdir -p /mnt/massos
```
Zamontuj główny system plików ext4/btrfs/xfs:
```
sudo mount /dev/sdXY /mnt/massos
```
Jeśli używasz partycji swap, włącz ją:
```
sudo swapon /dev/sdXY
```
Jeśli korzystasz z UEFI, utwórz niezbędny katalog i zamontuj ESP:
```
sudo mkdir -p /mnt/massos/boot/efi
sudo mount /dev/sdXY /mnt/massos/boot/efi
```
## Instalacja systemu podstawowego
Uruchom to polecenie, aby zainstalować system podstawowy na partycji MassOS:
```
sudo tar -xJpf massos-<WERSJA>-rootfs-x86_64.tar.xz -C /mnt/massos
```
**UWAGA:** Upewnij się, że zastąpiłeś <VERSION> aktualną wersją MassOS, którą instalujesz.
## Generowanie pliku /etc/fstab
Plik `/etc/fstab` zawiera listę systemów plików, które mają być automatycznie montowane podczas uruchamiania.

Musisz edytować plik `/mnt/massos/etc/fstab` i wprowadzić poprawne wpisy dla swoich partycji. Dla każdej potrzebnej partycji odkomentuj wiersz rozpoczynający się od `UUID=`, a następnie zastąp znaki zastępcze `x` identyfikatorem UUID dysku. **Możesz użyć `sudo blkid`, aby wyświetlić UUID partycji.**

Przykładowy plik /etc/fstab dla starszego systemu bez wymiany może wyglądać tak (zauważ, że tylko potrzebne partycje mają odkomentowaną linię UUID):
```
# Główny system plików:
UUID=539db496-6dfc-4c80-91b6-11cd278ba43c / ext4 defaults 1 1

# Swap (opcjonalny):
#UUID=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx swap swap pri=1 0 0

# System EFI (tylko UEFI):
#UUID=xxxx-xxxx /boot/efi vfat umask=0077 0 1
```
Przykładowy plik /etc/fstab dla systemu UEFI z funkcją swap może wyglądać tak:
```
# Główny system plików:
UUID=539db496-6dfc-4c80-91b6-11cd278ba43c / ext4 defaults 1 1

# Swap (opcjonalny):
UUID=6d31d057-df1e-4784-a287-019b310992a8 swap swap pri=1 0 0

# System EFI (tylko UEFI):
UUID=2712-B165 /boot/efi vfat umask=0077 0 1
```
Po zakończeniu zapisz i zamknij plik. Warto sprawdzić, czy wpisy są poprawne, uruchamiając `cat /mnt/massos/etc/fstab`, aby je wyświetlić. Błędy w tym pliku mogą uniemożliwić uruchomienie systemu.
## Wejście do środowiska chroot
Chociaż twój system ma wbudowane narzędzie `chroot`, powinieneś użyć `mass-chroot` z tego repozytorium. Jest to opakowanie wokół `chroot`, które zapewnia, że wszystkie wirtualne systemy plików jądra są poprawnie zamontowane.
```
wget -nc https://raw.githubusercontent.com/MassOS-Linux/MassOS/main/utils/mass-chroot
chmod 755 mass-chroot
sudo ./mass-chroot /mnt/massos
```
## Ustaw ścieżkę poprawnie.
Wejście do chroot domyślnie zachowuje tę samą ŚCIEŻKĘ, z której korzysta system hosta. Może to być niepoprawne dla MassOS, więc ustaw teraz poprawnie ścieżkę:
```
export PATH=/usr/local/bin:/usr/local/sbin:/usr/bin:/usr/sbin
```
## Ustawianie lokalizacji systemu i układu klawiatury
Listę lokalizacji można znaleźć w pliku `/etc/locales`. Edytuj ten plik za pomocą `nano` lub `vim` i odkomentuj linie wszystkich potrzebnych ustawień regionalnych. Pamiętaj, że jeśli jesteś użytkownikiem ze Stanów Zjednoczonych, który wymaga tylko ustawienia regionalnego `en_US.UTF-8`, nie musisz edytować tego pliku, ponieważ `en_US.UTF-8` jest domyślnie odkomentowany.

Wygeneruj ustawienia regionalne, uruchamiając następujące polecenie:
```
mklocales
```
Jeśli chcesz, aby domyślne ustawienia regionalne były inne niż `en_US.UTF-8`, edytuj plik `/etc/locale.conf` i zastąp `LANG=en_US.UTF-8` wybraną lokalizacją.

Domyślny układ klawiatury to `us`, co jest idealne dla mieszkańców USA. Jeśli potrzebujesz innego układu, edytuj plik `/etc/vconsole.conf` i zastąp `KEYMAP=us` dowolną inną mapą klawiszy. Pełną listę dostępnych map klawiszy można znaleźć za pomocą następującego polecenia:
```
ls /usr/share/keymaps/i386/qwerty | sed 's/.map.gz//'
```
Na przykład: Mapa klawiszy dla brytyjskich użytkowników to `uk`, więc wpis będzie miał postać `KEYMAP=uk`.
## Ustawianie strefy czasowej
Możesz uruchomić `tzselect`, aby znaleźć swoją strefę czasową w formacie *Region/Miasto*. Zada Ci kilka pytań o to, gdzie mieszkasz, i zwróci Twoją strefę czasową.

Ustaw strefę czasową za pomocą następującego polecenia:
```
ln -sf /usr/share/zoneinfo/Region/City /etc/localtime
```
Na przykład: dla *America/New_York*:
```
ln -sf /usr/share/zoneinfo/America/New_York /etc/localtime
```
## Ustawianie zegara sprzętowego
```
hwclock --systohc
```
## Ustawianie hasła roota
Wybierz bezpieczne hasło i ustaw je za pomocą następującego polecenia:
```
passwd
```
Aby uzyskać bezpieczne hasło, zaleca się kombinację litr, cyfr i symbol oraz 8+ kodowanie. Jeśli nie możesz wymyślić bezpieczne hasło, `pwmake 64` wygeneruje w bezpieczne hasło.
## Dodanie nowego użytkownika
Dodanie użytkownika go użytkownik jest użytkownik, który chce używać komputera na komputerze stacjonarnym, ponieważ zalogowanie się jako root może być włączonym do używania sprzętu, który nie jest używany. Dodaj użytkownika za pomocą następującego interaktywnego programu:
```
adduser
```
Zada Ci kilka pytań, w tym, czy konto powinno być administratorem, czy nie. Jeśli jesteś użytkownikiem systemu, to teraz tutaj 'y'. By defaults są dodawani do grupy `wheel`, `netdev` i `lpadmin`. Użytkownicy `wheel` mogą uruchamiać komendy jako root za pomocą `sudo`. Użytkownicy w `netdev` mogą interfejsami sieciowymi i połączeniami za pomocą NetworkManagera. Użytkownicy w `lpadmin` mogą oglądać drukowaniem za pomocą CUPS.
## Włączanie obsługi Bluetooth
Jeśli Twoje urządzenie spotkało się z Bluetooth, narzędziem graficznym i graficznym **Blueman** musiał pomóc w rozwiązaniu interfejsu graficznego. Jeśli chcesz, aby aplet Blueman w zasobniku systemowym uruchamiał się automatycznie podczas logowania, uruchamiaj polecenie:
```
blueman-autostart enable
```
Nie musisz (i prawdopodobnie nie powinieneś) uruchamiać tego polecenia, jeśli twój system nie obsługuje Bluetooth lub nie chcesz korzystać z funkcji Bluetooth. Jeśli wcześniej ją włączyłeś, a teraz chcesz ją wyłączyć, po prostu zamień `enable` na `disable` w powyższym poleceniu.
## Instalowanie dodatkowego oprogramowania
Niektóre urządzenia, takie jak karty bezprzewodowe lub karty graficzne, mogą wymagać niewolnego oprogramowania układowego „blobs” do prawidłowego działania. Jeśli jesteś właścicielem takiego urządzenia, możesz zainstalować najpopularniejsze niewolne oprogramowanie układowe za pomocą następujących poleceń:
```
pushd /usr/lib/firmware
git clone https://git.kernel.org/pub/scm/linux/kernel/git/firmware/linux-firmware.git .
rm -rf .git
popd
```
### Dodano obsługę mikrokodu procesora.
Nowoczesne procesory Intel i AMD obsługują mikrokod. Chociaż są one zwykle ładowane przez oprogramowanie układowe systemu BIOS, załadowanie go do systemu operacyjnego może umożliwić posiadanie zaktualizowanych wersji, których BIOS może nie mieć.

**Ładowanie mikrokodu w systemie operacyjnym nie jest wymagane, system powinien nadal działać bez niego.**

Jeśli masz procesor AMD, powyższe polecenia instalacji niewolnego oprogramowania układowego automatycznie zainstalują oprogramowanie układowe AMD Microcode.

Jeśli masz procesor Intel, musisz ręcznie zainstalować mikrokod procesora, uruchamiając następujące polecenia:
```
MVER=20210608
pushd /tmp
wget https://github.com/intel/Intel-Linux-Processor-Microcode-Data-Files/archive/microcode-$MVER.tar.gz
tar -xf microcode-$MVER.tar.gz
install -d /usr/lib/firmware/intel-ucode
install -m644 Intel-Linux-Processor-Microcode-Data-Files-microcode-$MVER/intel-ucode{,-with-caveats}/* /usr/lib/firmware/intel-ucode
rm -rf Intel-Linux-Processor-Microcode-Data-Files-microcode-$MVER microcode-$MVER.tar.gz
popd
unset MVER
```
## Generowanie initramfs
Initramfs to tymczasowy system plików używany do ładowania niezbędnych sterowników i montowania prawdziwego głównego systemu plików. Wygeneruj initramfs, uruchamiając to polecenie:
```
mkinitramfs $(ls /usr/lib/modules)
```
Jeśli zainstalowałeś oprogramowanie układowe i/lub mikrokod w powyższym kroku, to polecenie automatycznie uwzględni je podczas generowania initramfs.
## Instalowanie bootloadera GRUB
**OSTRZEŻENIE: Nieprawidłowa konfiguracja GRUB-a może uniemożliwić rozruch systemu. Upewnij się, że masz dostępne zapasowe urządzenie rozruchowe, aby móc odzyskać system w takim przypadku.**
### Starsze systemy BIOS
W starszych systemach uruchom następującą komendę, aby zainstalować bootloader GRUB, gdzie `X` to twój rzeczywisty dysk twardy (NIE pojedyncza partycja):
```
grub-install /dev/sdX
```
W przypadku starszych systemów BIOS nie są wymagane żadne dalsze kroki. Przejdź do sekcji „Generowanie pliku grub.cfg” poniżej.
### Systemy UEFI
**UWAGA: Bezpieczny rozruch musi być wyłączony w ustawieniach oprogramowania układowego UEFI, aby móc uruchomić system MassOS.**

W systemach UEFI musisz najpierw upewnić się, że `/sys/firmware/efi/efivars` jest zamontowany w środowisku chroot. Jeśli tak nie jest, poniższe polecenie `grub-install` nie powiedzie się:
```
mountpoint -q /sys/firmware/efi/efivars || (mount -t efivarfs efivarfs /sys/firmware/efi/efivars && touch /tmp/beforemounted)
```
Teraz zainstaluj GRUB za pomocą następującego polecenia:
```
grub-install
```
Spowoduje to zainstalowanie programu ładującego UEFI `EFI\massos\grubx64.efi` na partycji systemowej EFI i utworzenie wpisu kolejności rozruchu UEFI o nazwie `massos`.

Alternatywnie (lub równie dobrze) możesz zainstalować GRUB w lokalizacji rezerwowej, `EFI\BOOT\BOOTX64.EFI`. Jest to potrzebne tylko wtedy, gdy instalujesz MassOS na dysku wymiennym lub jeśli oprogramowanie układowe UEFI jest wadliwe i nie obsługuje zmiennych kolejności rozruchu UEFI. Nie uruchamiaj tego, jeśli inny system operacyjny zależy od awaryjnego bootloadera:
```
grub-install --removable
```
## Generowanie grub.cfg
Możesz dostosować swój program ładujący GRUB, edytując plik `/etc/default/grub`. Komentarze w tym pliku wyjaśniają, co robią opcje. Ewentualnie zostaw to i użyj zalecanych ustawień domyślnych MassOS.

Wygeneruj `/boot/grub/grub.cfg`, uruchamiając następujące polecenie:
```
grub-mkconfig -o /boot/grub/grub.cfg
```
## Odmontowywanie i ponowne uruchamianie
Najpierw odmontuj `/sys/firmware/efi/efivars`, jeśli został ręcznie zamontowany podczas "Instalowania programu ładującego GRUB":
```
test ! -f /tmp/przed zamontowaniem || (umount /sys/firmware/efi/efivars && rm -f /tmp/beforemounted)
```
Wyjdź z chroota:
```
exit
```
Teraz odmontuj systemy plików:
```
sudo umount -R /mnt/massos
```
Opcjonalnie usuń katalog punktów montowania:
```
sudo rmdir /mnt/massos
```
Teraz zrestartuj system, graficznie lub za pomocą następującego polecenia:
```
sudo shutdown -r now
```
Gratulacje, MassOS jest zainstalowany! Mamy nadzieję, że spodoba ci się korzystanie z MassOS!
## Co następne?
Aby uzyskać ogólne informacje o tym, jak najlepiej wykorzystać nową instalację, zajrzyj do [Post-Installation](https://github.com/MassOS-Linux/MassOS/wiki/Post-Installation). Zawiera informacje na temat wykonywania takich czynności, jak instalowanie oprogramowania, dostosowywanie pulpitu, a także inne przydatne wskazówki.
