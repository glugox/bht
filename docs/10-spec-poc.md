# Specifikacija – PoC BHT nabavke

## 1. Svrha
PoC služi da BHT vidi osnovni tok rada u aplikaciji za javne nabavke, na osnovu njihovog internog uputstva, prije izrade kompletne produkcijske verzije.

## 2. Obuhvat PoC-a

1. **Kreiranje nabavke (nacrt)**
    - broj nabavke, naziv, opis, organizaciona jedinica, tražilac, procijenjena vrijednost, valuta
    - status pri kreiranju: `nacrt`

2. **Odabir postupka nabavke**
    - vrijednosti: `direktni`, `hitni`, `pozivni`, `pregovaracki`
    - ručni odabir je dovoljan u PoC-u

3. **Registar dobavljača (osnovno)**
    - unos naziva dobavljača, kontakt (email/telefon), aktivan/neaktivan
    - odatle se biraju dobavljači za pozive

4. **Slanje poziva dobavljačima**
    - na jednoj nabavci odabrati 1..N dobavljača
    - označiti da je poziv poslan
    - vidjeti status poziva: `nacrt`, `poslan`, `bez_odgovora`

5. **Zaprimanje ponuda**
    - ručno dodati ponudu za nabavku (i konkretnog dobavljača)
    - polja: dobavljač, iznos, valuta, datum prijema
    - status ponude: `zaprimljena`

6. **Promjena statusa nabavke**
    - osnovni workflow: `nacrt` → `pozivi_poslani` → `ponude_zaprimljene` → `odobreno`
    - svaka promjena se bilježi

7. **Osnovni audit**
    - zabilježiti: korisnik, datum/vrijeme, stari i novi status

8. **Komunikacija (Q&A) na tender**
    - mogućnost da korisnik (BHT) doda komentar/pitanje na konkretnu nabavku ili poziv
    - mogućnost da doda odgovor na postojeće pitanje (thread 2 nivoa je dovoljan)
    - polja: autor, tekst, datum/vrijeme
    - vidljivost u PoC-u može biti jednostavna: svi interni vide sve
    - cilj: pokazati BHT-u da sistem podržava pitanja/odgovore tokom postupka

## 3. Nefunkcionalni zahtjevi za PoC
- Laravel 10+ aplikacija
- korištenje JSON konfiguracije (postojeća šema) za generisanje entiteta: Nabavka, Dobavljac, PozivNabavke, PonudaNabavke, **KomunikacijaNabavke**
- jedna baza (MySQL/PostgreSQL)
- jednostavan web UI (Blade ili Vue/Inertia)
- seed podataka: min. 25 redova

## 4. Šta nije u PoC-u
- višestepeno odobravanje (direktor, NO…)
- integracije sa finansijama
- napredna vidljivost komentara po dobavljaču
- arhiva dokumenata
- notifikacije mailom
