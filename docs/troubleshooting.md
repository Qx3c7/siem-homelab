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
