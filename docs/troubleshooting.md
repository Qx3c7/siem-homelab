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

Na początku nie wiedziałem, dlaczego `apt` nie działa. Sprawdziłem interfejsy za pomocą `ip a` . Po dodaniu NAT Ubuntu dostało adres IPv4, a następnie sprawdziłem trasę sieciową za pomocą `ip route`.


## Ubuntu nie mógł pingować Windowsa

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
