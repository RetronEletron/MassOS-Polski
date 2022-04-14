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
Jeśli używasz partycji wymiany, włącz ją:
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
Plik `/etc/fstab` zawiera listę systemów plików, które mają być automatycznie montowane podczas startu systemu.

Musisz edytować plik `/mnt/massos/etc/fstab` i wprowadzić poprawne wpisy dla swoich partycji. Dla każdej potrzebnej partycji odkomentuj wiersz rozpoczynający się od `UUID=`, a następnie zastąp znaki zastępcze `x` identyfikatorem UUID dysku. **Możesz użyć `sudo blkid`, aby wyświetlić UUID partycji.**

Przykładowy plik /etc/fstab dla starszego systemu bez wymiany może wyglądać tak (zauważ, że tylko potrzebne partycje mają odkomentowaną linię UUID):
```
# Główny system plików:
UUID=539db496-6dfc-4c80-91b6-11cd278ba43c / ext4 domyślne 1 1

# Zamiana (opcjonalnie):
#UUID=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx zamiana pri=1 0 0

# System EFI (tylko UEFI):
#UUID=xxxx-xxxx /boot/efi vfat umask=0077 0 1
```
Przykładowy plik /etc/fstab dla systemu UEFI z funkcją swap może wyglądać tak:
```
# Główny system plików:
UUID=539db496-6dfc-4c80-91b6-11cd278ba43c / ext4 domyślne 1 1

# Zamiana (opcjonalnie):
UUID=6d31d057-df1e-4784-a287-019b310992a8 swap swap pri=1 0 0

# System EFI (tylko UEFI):
UUID=2712-B165 /boot/efi vfat umask=0077 0 1
```
Po zakończeniu zapisz i zamknij plik. Warto dwukrotnie sprawdzić, czy wpisy są poprawne, uruchamiając `cat /mnt/massos/etc/fstab`, aby je wyświetlić. Błędy w tym pliku mogą uniemożliwić uruchomienie systemu.
## Wejście do środowiska chroot
Chociaż twój system ma wbudowane narzędzie `chroot`, powinieneś użyć `mass-chroot` z tego repozytorium. Jest to opakowanie wokół `chroot`, które zapewnia prawidłowe zamontowanie wszystkich wirtualnych systemów plików jądra.
```
wget -nc https://raw.githubusercontent.com/MassOS-Linux/MassOS/main/utils/mass-chroot
chmod 755 masowy chroot
sudo ./mass-chroot /mnt/massos
```
## Ustaw ścieżkę poprawnie.
Wejście do chroot domyślnie zachowuje tę samą ŚCIEŻKĘ, z której korzysta system hosta. Może to być niepoprawne dla MassOS, więc ustaw teraz poprawnie ścieżkę:
```
export PATH=/usr/local/bin:/usr/local/sbin:/usr/bin:/usr/sbin
```
## Ustawianie lokalizacji systemu i układu klawiatury
Listę lokalizacji można znaleźć w pliku `/etc/locales`. Edytuj ten plik za pomocą `nano` lub `vim` i odkomentuj linie wszystkich potrzebnych ustawień regionalnych. Pamiętaj, że jeśli jesteś użytkownikiem ze Stanów Zjednoczonych, który wymaga tylko lokalizacji `en_US.UTF-8`
