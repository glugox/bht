# Specifikacija – PoC BHT nabavke

## 1. Svrha
PoC služi da BH Telecom vidi osnovni tok rada u aplikaciji za nabavke, u skladu s internim **Uputstvom o primjeni i provođenju Pravilnika o nabavkama** – u daljem tekstu: Uputstvo. Uputstvo u članu 1 definiše predmet i primjenu (koje postupke nabavke Društvo koristi, ko učestvuje, kako se odobrava, kako se objavljuje), a u članu 2 daje ključne definicije (OPNZ, ponuđač, dobavljač, TD, ugovor/narudžba, procijenjena vrijednost). :contentReference[oaicite:1]{index=1}

Cilj PoC-a je:
- pokazati da aplikacija može da isprati **osnovni put od zahtjeva OPNZ-a do odluke**, i
- da podrži **tri najvidljivija postupka** iz člana 18 Uputstva: direktni, pozivni i javno nadmetanje (otvoreni).

## 2. Obuhvat PoC-a

### 2.1 Kreiranje nabavke (nacrt)
- unos: broj nabavke, naziv, opis, organizaciona jedinica/OPNZ, tražilac, procijenjena vrijednost;
- OPNZ je ovlašten za podnošenje zahtjeva za nabavku (član 2 + poglavlje o OPNZ u čl. 10–12); :contentReference[oaicite:2]{index=2}
- početni status u aplikaciji: `nacrt`;
- zaduženi radnik u EFP-u u stvarnom procesu provjerava potpune podatke i može vratiti na korekciju (u app-u to ćemo podržati kroz komentare i status „vraćeno“);
- **referenca:** član 15 i 16 Uputstva (pokretanje i izrada zahtjeva) :contentReference[oaicite:3]{index=3}

### 2.2 Tri ključna postupka u PoC-u
U Uputstvu (član 18 – vrste postupaka nabavke) je jasno da Društvo koristi više postupaka: direktni, hitni, pozivni, pregovarački, javno nadmetanje, posebne i sl. Za PoC uzimamo tri koja su i u praksi najilustrativnija i dobro opisana u istom dokumentu: :contentReference[oaicite:4]{index=4}

1. **Direktni postupak**
    - primjena: pojedinačne nabavke do i uključivo 8.500,00 KM (bez PDV-a);
    - provodi ga nadležna organizaciona jedinica EFP-a **bez formiranja komisije**;
    - poziv se može uputiti jednom ili više ponuđača; prijem ponude može biti i e-mailom;
    - rokovi su kratki (2–5 dana za ponudu, 3 dana evaluacija);
    - završava se izvještajem sa preporukom i, u pravilu, narudžbenicom koja ima elemente ugovora.
    - **referenca:** dio Uputstva koji opisuje direktni postupak (član 19 – primjena i provođenje) :contentReference[oaicite:5]{index=5}

2. **Pozivni postupak**
    - poziv i TD se šalju **određenom broju ponuđača**, u pravilu ne manje od tri;
    - primjenjuje se za više vrijednosne pragove (8.501–100.000 KM) i može biti s objavom ili bez objave na web stranici Društva;
    - nakon zaprimljenih ponuda radi se ocjena/evaluacija, izrađuje se izvještaj i donosi odluka;
    - u aplikaciji to znači: na nabavci moramo moći odabrati 3+ dobavljača i označiti da im je poziv poslan.
    - **referenca:** Uputstvo, dio “Pozivni postupak” (član 26 i 27 – provođenje pozivnog postupka) :contentReference[oaicite:6]{index=6}

3. **Javno nadmetanje (otvoreni postupak)**
    - Uputstvo izričito navodi da je **javno nadmetanje postupak za koji se obavezno objavljuje obavještenje o nabavci na web stranici Društva** – to je ključna razlika u odnosu na pozivni;
    - učestvovati mogu svi zainteresovani ponuđači koji ispunjavaju uslove iz TD;
    - postupak provodi komisija ili zaduženi radnik prema rješenju o imenovanju; Komisija otvara blagovremeno pristigle ponude i vodi zapisnik;
    - nakon otvaranja i provjere (koverta 1 / formalno-tehnički dio) ide dio sa cijenom / nadmetanjem i na kraju se sačinjava izvještaj o radu i prijedlog odluke;
    - u aplikaciji to ćemo pojednostaviti u faze:
        1. objavljeno (obavještenje na webu),
        2. ponude zaprimljene,
        3. komisijsko otvaranje / provjera,
        4. javno nadmetanje u toku (opciono),
        5. izvještaj i odobrenje.
    - **referenca:** član 18 – vrste postupaka; dio Uputstva koji govori o obaveznoj objavi na web stranici Društva i radu komisije u postupcima s objavom :contentReference[oaicite:7]{index=7}

> Napomena: “Pregovarački postupak” (čl. 28–30) postoji i u Uputstvu, ali u PoC-u ga samo ponudimo kao vrijednost u polju `postupak_nabavke`, bez implementacije kompletnog toka. To je dovoljno da pokažemo da aplikacija može dodavati nove tokove. :contentReference[oaicite:8]{index=8}

### 2.3 Odabir postupka u formi
- forma u aplikaciji ima polje:  
  `postupak_nabavke` = {`direktni`, `pozivni`, `javno_nadmetanje`, `pregovaracki`}
- OPNZ / zaduženi radnik bira postupak *uz obavezu da vodi računa o interesima Društva* – to je eksplicitno u članu 18 Uputstva (“OPNZ prilikom odabira vrste postupka mora voditi računa o najvišim interesima Društva”).
- **referenca:** član 18 – predmet i vrste postupaka nabavke :contentReference[oaicite:9]{index=9}

### 2.4 Registar dobavljača (osnovno)
- Uputstvo traži da se primijeni politika upravljanja dobavljačima i da se može provjeriti ko se poziva, ko se isključuje, i da se to evidentira; app zato mora imati minimalnu tabelu dobavljača;
- u PoC-u: `naziv`, `kontakt`, `aktivan` (da/ne);
- koristi se za: izbor dobavljača kod direktnog i pozivnog, kao i za povezivanje ponuda;
- **referenca:** član 6 i 7 – primjena politike upravljanja dobavljačima i godišnje izvještavanje :contentReference[oaicite:10]{index=10}

### 2.5 Slanje poziva / objava
- **direktni**: poziv ide jednom ili više ponuđača, može e-mailom; u app-u – dugme “poziv poslan” + datum;
- **pozivni**: mora biti 3 (u pravilu ne manje od tri) ponuđača → možemo validirati da su odabrana min. 3 dobavljača prije slanja;
- **javno nadmetanje**: mora postojati evidencija da je obavještenje objavljeno na web stranici Društva; u PoC-u to ćemo svesti na checkbox/polje `objavljeno_na_webu_at` + naziv dokumenta/URL;
- **referenca:** član 26 i 27 (pozivni), te dio člana 18 gdje se javno nadmetanje navodi kao poseban postupak koji se objavljuje :contentReference[oaicite:11]{index=11}

### 2.6 Zaprimanje ponuda
- ručni unos ponude za konkretnog dobavljača i konkretnu nabavku;
- polja: `dobavljac_id`, `iznos`, `valuta`, `datum_prijema`, `napomena`, `broj_ponude`;
- za javno nadmetanje dodati polje tipa `komisijsko_otvaranje_zapisnik` (tekst) da bismo ispoštovali da komisija vodi zapisnik o otvaranju i da se zna koje su ponude blagovremeno pristigle;
- **referenca:** dio Uputstva o kontroli zahtjeva i vraćanju na korekciju (član 16), te dio o provođenju postupaka gdje komisija razmatra i ocjenjuje ponude (pozivni i javno nadmetanje) :contentReference[oaicite:12]{index=12}

### 2.7 Promjena statusa nabavke
PoC workflow:

1. `nacrt` – OPNZ popunjava i šalje (član 16)
2. `na_provjeri` – zaduženi radnik EFP provjerava i može vratiti na korekciju (član 16 – primjedbe/sugestije ili povrat) :contentReference[oaicite:13]{index=13}
3. `pozivi_poslani` – za direktni/pozivni ili `objavljeno` za javno nadmetanje
4. `ponude_zaprimljene` – najmanje jedna ponuda postoji
5. `javno_nadmetanje_u_toku` – **samo za javno nadmetanje**, da označimo komisijsku fazu/nadmetanje
6. `odobreno` – nakon izvještaja / zapisnika

- za direktni: nakon `odobreno` generiše se narudžbenica.
- **referenca:** čl. 19 (direktni – poziv, prijem, evaluacija, izvještaj, narudžba) + čl. 26–27 (pozivni – obavještenje, slanje TD, komisijsko razmatranje) + dio o javnom nadmetanju iz čl. 18 (objava + komisija) :contentReference[oaicite:14]{index=14}

### 2.8 Komunikacija (pitanja i odgovori) na tender
- Uputstvo eksplicitno kaže da zaduženi radnik može dostavljati primjedbe/sugestije ili vratiti zahtjev (član 16); to je odličan oslonac da u app-u imamo **Komentare/Q&A** na nivou nabavke;
- entitet: `KomunikacijaNabavke` (nabavka_id, autor_id, tekst, datum, parent_id);
- koristi se i za unutrašnju komunikaciju OPNZ ↔ EFP ↔ Direktor prije odobravanja;
- **referenca:** član 16 – otklanjanje nedostataka i komunikacija sa OPNZ-om :contentReference[oaicite:15]{index=15}

### 2.9 Osnovni audit
- zapisati: korisnik, vrijeme, stari_status, novi_status, napomena
- razlog: u dijelu o odgovornostima (čl. 12) jasno stoji ko je za šta odgovoran, pa je logično da app vodi trag;
- **referenca:** član 12 – odgovornosti u procesu nabavke :contentReference[oaicite:16]{index=16}

## 3. Tehničke pretpostavke za PoC
- Laravel 10+
- generisanje entiteta preko postojeće JSON konfiguracije: **Nabavka**, **Dobavljač**, **PozivNabavke**, **PonudaNabavke**, **KomunikacijaNabavke**
- jedna baza (MySQL/PostgreSQL)
- seed: min. 25 redova (OPNZ, 3 dobavljača, 3 nabavke po postupku)
- UI: Blade ili Vue/Inertia
- rokovi iz Uputstva (2–4 dana) samo kao polja, ne automatizujemo ih u PoC-u
- **referenca:** organizacija postupka u čl. 16 i 27 – rok 4 dana za pokretanje, 1 dan za slanje TD, itd. :contentReference[oaicite:17]{index=17}

## 4. Van obuhvata PoC-a
- višestepeno odobravanje po iznosu (član 13 – ovlašteni organi)
- online nabavke (član 22–25)
- reklamno-promotivne nabavke (član 21)
- posebni/pregovarački postupci u punom obimu (član 28–30)
- integracije sa OWIS/PS modulom – u PoC-u ručni unos

---

# Specifikacija razvoja aplikacije za BHT javne nabavke (PoC fokus)

## 1. Uloge (roles)

### 1.1 OPNZ
- kreira i šalje zahtjev (`nacrt` → `na_provjeri`)
- akcije: `procurement.create_draft`, `procurement.update_draft`, `procurement.submit_for_check`, `procurement.add_qa`
- ne može slati pozive, ni odobravati

### 1.2 NABAVKE (EFP / zaduženi radnik)
- provjerava usklađenost sa Uputstvom
- bira postupak (`procurement.select_procedure`) iz skupa: direktni, pozivni, javno_nadmetanje, pregovaracki
- šalje pozive / označava objavu
- prima ponude
- akcije: `procurement.return_for_fix`, `procurement.mark_plan_checked`, `procurement.select_procedure`, `procurement.send_invitations`, `procurement.receive_offer`, `procurement.change_status`, `procurement.add_qa`
- ne može konačno odobriti

### 1.3 DIREKTOR / ovlašteno lice
- završno odobravanje
- akcije: `procurement.approve`, `procurement.reject`, `procurement.add_qa`

### 1.4 READ_ONLY
- samo pregled

## 2. Akcije (kontrolerski pozivi)
- `procurement.create_draft`
- `procurement.update_draft`
- `procurement.submit_for_check`
- `procurement.return_for_fix`
- `procurement.mark_plan_checked`
- `procurement.select_procedure`
- `procurement.send_invitations`
- `procurement.receive_offer`
- `procurement.change_status`
- `procurement.add_qa`
- `procurement.approve`
- `procurement.reject`
