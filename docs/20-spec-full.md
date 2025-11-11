# Specifikacija – puna verzija aplikacije za BHT javne nabavke

## 1. Svrha
Puna verzija aplikacije implementira kompletan proces javnih nabavki prema internom uputstvu BHT, uključujući provjere, odobravanja, rad sa dobavljačima, evaluaciju ponuda, realizaciju i izvještavanje. Ovaj dokument proširuje PoC i definiše šta se radi u fazi produkcijske izrade.

## 2. Obuhvat

### 2.1 Registri i šifarnici
- **Organizacione jedinice (OJ)**: šifra, naziv, opis.
- **Dobavljači**: naziv, kontakt podaci, PIB, status (aktivan/neaktivan).
- **Korisnici i uloge**: korisnički nalozi, role (npr. OPNZ, Nabavke, Direktor, NO).
- **Pomoćne šifre**: valute, vrste postupaka.

### 2.2 Zahtjev / OPNZ provjera
- Podnošenje zahtjeva za nabavku od strane ovlaštenog podnosioca (OPNZ).
- Polja: predmet nabavke, opis, procijenjena vrijednost, OJ, tražilac, rok.
- Provjera potpunosti zahtjeva od strane službe (nabavke/EFP).
- Mogućnost **vraćanja na doradu** uz bilješku.
- Audit ko je vratio i zašto.

### 2.3 Plan i sredstva
- Oznaka da je nabavka u planu ili da su sredstva osigurana.
- Polje “plan provjeren” (da/ne) + datum + korisnik.
- Validacija: bez provjere plana ne može se ići na izbor postupka.

### 2.4 Odabir postupka nabavke
- Postupci: **direktni**, **hitni**, **pozivni**, **pregovarački**.
- Pravilo po procijenjenoj vrijednosti (interno BHT pravilo).
- Evidentiranje razloga ako je izabran pregovarački ili hitni postupak.
- Veza sa pozivima (pozivni → min. 3 dobavljača).

Sistem treba automatski da predloži postupak na osnovu procijenjene vrijednosti:

- do **8.500 KM** → prijedlog: **direktni postupak**
- do **8.500 KM** i označeno “hitno” → prijedlog: **hitna nabavka**
- od **8.501 KM** do **100.000 KM** → prijedlog: **pozivni postupak** (minimalno 3 dobavljača)
- izvan ovih pragova ili uz poseban razlog (npr. ekskluzivni dobavljač) → **pregovarački postupak** (uz obavezno obrazloženje)

Korisnik može ručno promijeniti postupak, ali sistem mora sačuvati i prikazati upozorenje ako izabrani postupak nije u skladu s pragom.


### 2.5 Pozivi i tenderska dokumentacija
- Kreiranje poziva za dostavu ponude na osnovu nabavke.
- Dodavanje i čuvanje tenderske dokumentacije (link ili upload).
- Odabir dobavljača kojima se poziv šalje.
- Status poziva: `nacrt`, `poslan`, `dostavljen`, `bez_odgovora`.
- Evidencija datuma slanja.

### 2.6 Q&A / komunikacija sa dobavljačima
- Mogućnost da dobavljač ili služba postavi pitanje vezano za nabavku ili konkretan poziv.
- Odgovor službe, uz mogućnost da odgovor bude:
    - interni (vidi samo BHT)
    - vidljiv svim pozvanim dobavljačima
    - vidljiv samo jednom dobavljaču
- Threaded komentari (pitanje → odgovor).
- Evidencija vremena i autora komunikacije.

### 2.7 Ponude
- Zaprimanje ponuda od pozvanih dobavljača.
- Veza ponude na:
    - nabavku
    - dobavljača
    - poziv (ako postoji)
- Polja: iznos, valuta, datum prijema, napomena, status ponude.
- Statusi ponude: `zaprimljena`, `evaluirana`, `odbijena`, `prihvacena`.
- Mogućnost unosa više ponuda za istu nabavku (različiti dobavljači).

### 2.8 Evaluacija i izvještaj
- Uporedni pregled ponuda (cijena, rok, uslovi).
- Polje “preporučeni dobavljač”.
- Generisanje izvještaja o ocjeni ponuda / izboru.
- Čuvanje verzije izvještaja uz nabavku (pdf/link).

### 2.9 Odobravanje
- Višestepeno odobravanje prema vrijednosti nabavke (npr. šef, direktor, NO).
- Evidencija: ko je odobrio, kada, koju verziju izvještaja.
- Status nabavke prelazi u `odobreno` tek kada je zadnji nivo odobrio.
- Mogućnost “vrati na doradu” sa komentarom.

### 2.10 Narudžbenica / ugovor
- Na osnovu odobrene nabavke generiše se narudžbenica/ugovor.
- Sadrži: predmet, količinu/stavke, cijenu, rok, mjesto isporuke, izabranog dobavljača.
- Evidentira se broj narudžbenice/ugovora i datum.

### 2.11 Realizacija i tehnički prijem
- Unos isporuke (datum, količina, napomena).
- Tehnički prijem / potvrda da je roba/usluga primljena i ispravna.
- Nakon tehničkog prijema nabavka se može označiti kao `zatvoreno`.

### 2.12 Audit i izvještavanje
- Dnevnik svih promjena statusa i važnih polja.
- Izvještaji:
    - po postupku nabavke
    - po organizacionoj jedinici
    - po periodu
    - po dobavljaču
- Export u CSV/XLSX.

### 2.13 Notifikacije i ekstenzije
- Mail ili sistemska obavijest na događaje:
    - “stavljena nova ponuda”
    - “potrebno odobrenje”
    - “rok za ponude istekao”
- Oslonac na Laravel evente (npr. `ProcurementApproved`, `OfferReceived`) da bi drugi moduli mogli reagovati (npr. modul za audit ili modul za slanje maila).

## 3. Tehničke smjernice

### 3.1 Arhitektura
- Osnovne domenske tabele i modeli (nabavka, stavke, pozivi, ponude, odluke, isporuke, dobavljači, OJ) se nalaze u glavnoj Laravel aplikaciji.
- Napredne funkcije (notifikacije, audit, integracije) mogu biti izdvojene u vanjske pakete koji slušaju Laravel evente.
- Korištenje queue-a (Redis) za slanje mailova, generisanje dokumenata i slanje obavijesti.
- API sloj verzionisan: `/api/v1/...`

### 3.2 Sigurnost i prava
- Autentikacija (Laravel, npr. Sanctum).
- Uloge i dopuštenja: OPNZ, Nabavke, Direktor/NO, ReadOnly.
- Policies po entitetu: samo vlasnik/OJ može uređivati nabavku do određenog statusa.
- Log svih osjetljivih radnji.

### 3.3 Performanse i skalabilnost
- Eager loading za liste nabavki i ponuda.
- Paginacija na svim listama.
- Mogućnost kasnijeg dodavanja read-replica baze.
- File storage (dokumentacija) u S3/minio.

## 4. Granice PoC vs. puna verzija
- Sve što je označeno kao “provjera”, “odobravanje”, “Q&A”, “izvještaj” i “notifikacije” ulazi u punu verziju i ne mora postojati u prvom demou.
- PoC koristi isti model podataka, tako da se migracije mogu nadograditi, a ne mijenjati iz početka.

## 5. Dalje dopune
- Nakon detaljnijeg čitanja Uputstva BHT mogu se dodati specifična pravila za iznose i organe odobravanja.
- Dokument se vodi u `docs/` folderu i verzionira u git repozitoriju.
