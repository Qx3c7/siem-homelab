# Konfiguracja sieci

## Topologia

Krótki opis:
- Ubuntu Server:
  - `enp0s3` -> Karta sieciowa ustawiona w tryb Internal Network, łączymy się przez nią z Agentem (Windows). 
  - `enp0s8` -> Karta sieciowa ustawiona w tryb NAT, łączymy się przez nią z Internetem
- Windows:
  - karta sieciowa -> Karta sieciowa ustawiona w tryb Internal Network, łączymy się przez nią z serwerem. 


## Adresacja

- Sieć SIEM-LAB: `10.10.10.0/24`
- Ubuntu: `10.10.10.1/24`
- Windows: `10.10.10.2/24`
- Sieć NAT: `10.0.3.0/24`
- Ubuntu `enp0s8`: `10.0.3.15/24`

gdzie /24 oznacza, że pierwsze 24 bity adresu identyfikują część sieciową, a pozostałe 8 bitów część hosta.

## Routing na Ubuntu
```text
default via 10.0.3.2 dev enp0s8 proto dhcp src 10.0.3.15 metric 1024
```
Wpis ten oznacza trasę domyślną. Ruch, który nie pasuje do żadnej szczegółowej trasy, wysyłany jest do bramy. 
Ruch idzie przez interfejs `enp0s8` do bramy `10.0.3.2`. 
Adres źródłowy Ubuntu to `10.0.3.15`.
Dzięki tej trasie Ubuntu ma dostęp do Internetu.

### Trasa do sieci lokalnej

```text
10.10.10.0/24 dev enp0s3 proto kernel scope link src 10.10.10.1
```
Wpis ten mówi, że poruszamy się po podsieci `10.10.10.0/24`.
Ruch idzie przez interfejs `enp0s3`.
Adres źródłowy Ubuntu to `10.10.10.1`.
Jest to sieć wewnętrzna, a brama nie jest wymagana do komunikacji w tej samej podsieci.
