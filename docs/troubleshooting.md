# Troubleshooting

## Ubuntu nie miało dostępu do Internetu

### Objaw

`apt` nie mógł rozwiązać `archive.ubuntu.com`.

### Diagnostyka

Sprawdzono:
- `ip a`
- `ip route`
- `ping 1.1.1.1`
- `ping archive.ubuntu.com`

### Przyczyna

Pierwsza karta sieciowa była podłączona tylko do `Internal Network`, więc Ubuntu nie miało dostępu do Internetu.

### Rozwiązanie

Dodano drugą kartę sieciową w trybie NAT.

### Czego się nauczyłem

Na początku nie wiedziałem, dlaczego `apt` nie działa. Sprawdziłem interfejsy za pomocą `ip a`. Po dodaniu NAT Ubuntu dostało adres IPv4, a następnie sprawdziłem trasę sieciową za pomocą `ip route`.


## Ubuntu nie mogło pingować Windowsa

### Objaw

Agent (Windows) mógł pingować serwer (Ubuntu), a serwer nie mógł pingować agenta.

### Diagnostyka

Zweryfikowałem komunikację w obie strony.

Windows -> Ubuntu działa  
Ubuntu -> Windows nie działa

Na agencie przejrzałem reguły zapory i zauważyłem, że reguły pozwalające na odpowiedź na ICMPv4 były wyłączone.

### Przyczyna

Firewall blokował przychodzące żądania ICMP Echo Request, ponieważ reguła pozwalająca na to była wyłączona 

### Rozwiązanie

Włączenie reguły pozwalającej na odpowiedź na żądania ICMP Echo Request.

### Czego się nauczyłem

Brak odpowiedzi na ping nie oznacza jednoznacznie, że pingowane urządzenie jest wyłączone albo nie jest podłączone do sieci. Komunikacja może być blokowana przez reguły firewalla dla konkretnego protokołu.

## Ubuntu miało za mało miejsca na dysku 

### Objaw

Dysk Ubuntu wykazuje rozmiar 11,5 GB oraz 5,5 GB wolnego miejsca 

### Diagnostyka

Weryfikacja woluminu logicznego (LV) oraz grupy woluminów (VG) i porównanie ich. Wykazało to, że LV ma rozmiar 11,5 GB  mimo że VG ma rozmiar 23 GB.

### Przyczyna

Część miejsca nie została domyślnie przypisana do woluminu logicznego używanego przez system. 

### Rozwiązanie

Ręcznie zwiększono wielkość woluminu logicznego do wielkości grupy woluminów komendą 
```text
sudo lvextend -r -l +100%FREE /dev/ubuntu-vg/ubuntu-lv
```

### Czego się nauczyłem

Mała ilość pokazanego miejsca nie zawsze świadczy o tym, że dysk jest zapełniony. W przypadku LVM część miejsca może być dostępna w grupie woluminów, ale nie przypisana do woluminu logicznego 



