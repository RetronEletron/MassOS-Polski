# MassOS i wolne oprogramowanie
Ten artykuł wyjaśnia, czym jest wolne oprogramowanie, jak wolne oprogramowanie jest powiązane z MassOS oraz jakie są potencjalne problemy z wolnym oprogramowaniem i MassOS.

**UWAGA:** To nie jest porada prawna.

# Co to jest wolne oprogramowanie?
Mówiąc najprościej, wolne oprogramowanie to oprogramowanie, które gwarantuje wolność swoim użytkownikom. Zapewnia użytkownikom cztery podstawowe wolności, którymi są:

- Wolność korzystania z oprogramowania w dowolnym celu.
- Swoboda studiowania kodu źródłowego oprogramowania i zmieniania go tak, aby działał tak, jak chcesz.
- Wolność redystrybucji dokładnych kopii oprogramowania.
- Wolność redystrybucji zmodyfikowanych kopii oprogramowania.

Wolne oprogramowanie nie oznacza, że ​​jest ono bezpłatne. Chociaż większość programów wolnego oprogramowania jest również bezpłatna, termin „wolne oprogramowanie” wyraźnie odnosi się do wolności użytkowników, jak wspomniano powyżej. Program, który jest bezpłatny, niekoniecznie musi być wolnym oprogramowaniem. Program jest uważany za wolne oprogramowanie, jeśli gwarantuje wszystkie powyższe wolności. Programy, które nie są uważane za niewolne lub zastrzeżone.

## A co z open source?
Fizycznie wolne oprogramowanie i open source (prawie zawsze) odnoszą się do tego samego oprogramowania, jednak mają różne poglądy etyczne. Open source zgadza się z praktycznymi korzyściami wolnego oprogramowania, takimi jak umożliwienie użytkownikom naprawiania błędów bez polegania na deweloperze, jednak ignoruje kwestie etyczne związane z oprogramowaniem własnościowym.

# Darmowe oprogramowanie w MassOS
MassOS to zbiór wielu różnych programów, które razem tworzą działający system operacyjny.

Wszystkie skrypty i narzędzia znajdujące się w repozytoriach MassOS są objęte licencją GNU General Public License w wersji 3 lub nowszej (GPLv3+).

Staramy się, aby oprogramowanie firm trzecich wchodzące w skład systemu MassOS było wolnym oprogramowaniem. Jednym ze sposobów, w jaki to robimy, jest egzekwowanie wymogu, że każde oprogramowanie domyślnie zawarte w MassOS musi być przeznaczone dla domeny publicznej lub chronione licencją zgodną z GPL. Na naszym poziomie zaangażowania staramy się odrzucać programy objęte licencjami niezgodnymi z GPL (z nielicznymi wyjątkami wymienionymi w poniższej sekcji). Dwa znane przykłady to cdrtools, zestaw narzędzi do pracy z obrazami CD i ISO, oraz FDK-AAC, biblioteka do pracy z formatem audio AAC. W tych przykładach zastąpiliśmy obydwa alternatywami zgodnymi z GPL — cdrkit i FAAD2.

Jeśli zauważysz jakiekolwiek inne oprogramowanie niezgodne z GPL, dołączone domyślnie w MassOS (którego **nie jest** wymienione w sekcji „Wyjątki” poniżej), daj nam znać, [otwierając zgłoszenie](https:// github.com/MassOS-Linux/MassOS/issues). Postaramy się to naprawić tak szybko, jak to możliwe, najlepiej poprzez znalezienie alternatywy zgodnej z GPL lub przez usunięcie jej całkowicie w mało prawdopodobnym przypadku, gdy będziemy musieli.

# Wyjątki
Chociaż staramy się, aby system MassOS pozostał wolny, istnieje bardzo niewiele wyjątków od wymogu wolnego oprogramowania, które wymieniono tutaj.

**UWAGA:** Niezależnie od tych wyjątków, **nie** chcemy ani nie planujemy prosić o umieszczenie w wykazie/zatwierdzenie przez projekt GNU.

## Oprogramowanie układowe
Wiele elementów sprzętu, zwłaszcza nowoczesne karty bezprzewodowe, dźwiękowe i graficzne, do działania wymaga zastrzeżonego oprogramowania układowego. Bez tych „blobów” nie będzie możliwa obsługa tych urządzeń, co w niektórych przypadkach może poważnie utrudnić korzystanie z komputera. Aby rozwiązać ten problem, udostępniamy dwa obrazy Live ISO dla MassOS. Domyślny zawiera niewolne oprogramowanie układowe wymagane do obsługi większości niewygodnych urządzeń, a alternatywny (z dodaną etykietą „-nofirmware”) nie zawiera zastrzeżonego oprogramowania układowego. Jeśli wiesz, że twój sprzęt nie wymaga oprogramowania układowego i/lub nie chcesz go używać, możesz pobrać ISO "-nofirmware". Należy jednak zgłaszać problemy dotyczące sprzętu tylko w przypadku korzystania z obrazu z oprogramowaniem układowym.

Podstawowy system (wytwarzany przez system kompilacji MassOS) nie zawiera niewolnego oprogramowania układowego (w tym tarball rootfs). Oprogramowanie układowe jest dodawane tylko na etapie tworzenia Live ISO.

## Problem ze znakiem towarowym Mozilla Firefox
**UWAGA:** Ten problem może, ale nie musi dotyczyć także programu Mozilla Thunderbird. Obecnie nie wiemy, czy tak jest, czy nie.

Przeglądarka internetowa Firefox, opracowana przez Mozilla Foundation, jest wolnym oprogramowaniem, wydanym na licencji Mozilla Public License (MPL) w wersji 2. Chociaż oprogramowanie jest bezpłatne, Mozilla egzekwuje osobną licencję na znak towarowy, która określa warunki, w jakich branding „Firefox” i jego logo może/nie może być używany w odniesieniu do czterech wolności określonych przez Free Software Foundation.

Licencja stwierdza, że ​​pliki binarne dostarczone przez Mozillę (które są używane w MassOS), a także wszelkie inne pliki binarne wyprodukowane z niezmodyfikowanych źródeł Firefoksa, mogą być swobodnie rozpowszechniane pod nazwą „Firefox” i używać oficjalnego logo Firefoksa. Jednak licencja wymaga, aby pliki binarne utworzone ze zmodyfikowanego kodu źródłowego zawierały inną nazwę
