# Specifikacija – PoC BHT nabavke

## 1. Svrha
PoC služi da BH Telecom vidi osnovni tok rada u aplikaciji za nabavke, u skladu s internim **Uputstvom o primjeni i provođenju Pravilnika o nabavkama** (član 1) i definicijama učesnika u postupku (član 2).

## 2. Obuhvat PoC-a

### 2.1 Kreiranje nabavke (nacrt)
- unos: broj nabavke, naziv, opis, organizaciona jedinica/OPNZ, tražilac, procijenjena vrijednost
- OPNZ je definisan kao ovlašten za podnošenje zahtjeva za nabavku (član 2 – OPNZ; član 10 i 11 – ko je OPNZ)
- početni status u app-u: `nacrt`
- **referenca:** član 15 (pokretanje postupka nabavke) + član 16 stav 1 (sačinjavanje i dostavljanje zahtjeva za nabavku)

### 2.2 Tri ključna postupka u PoC-u
U Uputstvu su u članu 18 taksativno pobrojani postupci koje Društvo koristi. Za PoC pokrivamo tri koja su najčešća i dovoljno jasno opisana za brzu demonstraciju:

1. **Direktni postupak** – za nabavke ≤ 8.500,00 KM, provodi ga zaduženi radnik/EFP bez komisije; na kraju se obično izdaje narudžbenica sa svim elementima ugovora.
    - **referenca:** član 19, stav (1), (2), (14) :contentReference[oaicite:3]{index=3}
2. **Pozivni postupak** – poziv se šalje određenom broju ponuđača, „u pravilu ne manje od tri“, za nabavke iz odgovarajućeg vrijednosnog ranga; na kraju se sačinjava izvještaj sa preporukom i odluka o dodjeli/poništenju.
    - **referenca:** dio “Pozivni postupak – sačinjava se izvještaj sa preporukom...” :contentReference[oaicite:4]{index=4}
3. **Javno nadmetanje (otvoreni postupak)** – postupak za koji se **obavezno objavljuje obavještenje o nabavci na web stranici Društva**, otvaranje ponuda je pred komisijom, nakon ocjene iz koverte 1 donosi se odluka o ocjeni ponuda, zatim se otvara koverta 2 i obavlja javno nadmetanje/pregovori, pa se sačinjava izvještaj o radu.
    - **referenca:** član 31 “Javno nadmetanje je postupak nabavke za koji se obavezno objavljuje obavještenje...” i opis toka javnog nadmetanja :contentReference[oaicite:5]{index=5} :contentReference[oaicite:6]{index=6} :contentReference[oaicite:7]{index=7}

> Napomena: “Pregovarački postupak” i dalje postoji u članu 18, ali ga u PoC-u samo ponudimo kao vrijednost u polju `postupak_nabavke`, bez kompletnog toka.  
> **referenca:** član 18 – vrste postupaka :contentReference[oaicite:8]{index=8}

### 2.3 Odabir postupka u formi
- forma u aplikaciji ima polje:  
  `postupak_nabavke` = {`direktni`, `pozivni`, `javno_nadmetanje`, `pregovaracki`}
- korisnik (OPNZ/zaduženi radnik) može ručno izabrati postupak, jer u članu 18 jasno piše da OPNZ pri odabiru vrste postupka mora voditi računa o najvišim interesima Društva
- **referenca:** član 18, završni dio (“OPNZ prilikom odabira vrste postupka...”) :contentReference[oaicite:9]{index=9}

### 2.4 Registar dobavljača (osnovno)
- u Uputstvu je naglašena **politika upravljanja dobavljačima** i provjera podataka o dobavljačima, pa je opravdano imati internu listu dobavljača u app-u
- u PoC-u: `naziv`, `kontakt`, `aktivan` (da/ne)
- koristi se za povezivanje poziva i ponuda
- **referenca:** član 6 i 7 – provjera i primjena politike upravljanja dobavljačima :contentReference[oaicite:10]{index=10}

### 2.5 Slanje poziva dobavljačima
- na jednoj nabavci odabrati 1..N dobavljača i označiti “poziv poslan”
- čuva se datum slanja
- za **pozivni postupak** „broj u pravilu nije manji od tri“ – to možemo validirati u aplikaciji
- za **javno nadmetanje** objava ide na web stranicu Društva (to možemo u PoC-u samo označiti checkboxom `objavljeno_na_webu`)
- **referenca:** za pozivni – dio “Pozivni postupak” :contentReference[oaicite:11]{index=11}; za javno nadmetanje – “obavezno objavljuje obavještenje o nabavci na web stranici Društva” :contentReference[oaicite:12]{index=12}

### 2.6 Zaprimanje ponuda
- ručni unos ponude za konkretnog dobavljača i konkretnu nabavku
- polja: dobavljač, iznos, valuta, datum prijema, napomena
- u **direktnom postupku** rok za evaluaciju je kratak (do 3 dana) – u PoC-u samo polje “primljeno_u”
- u **javnom nadmetanju** otvaranje zaprimljenih ponuda vrši komisija, neposredno nakon preuzimanja ponuda, i svi ponuđači imaju pravo prisustvovati otvaranju – mi u PoC-u to modelujemo kao status + zapisnik (tekst) :contentReference[oaicite:13]{index=13}
- **referenca:** član 19, stav (10)–(13) za prijem i evaluaciju; član 76 za otvaranje ponuda i rad komisije u postupcima gdje se imenuje komisija :contentReference[oaicite:14]{index=14}

### 2.7 Promjena statusa nabavke
Minimalni workflow koji pokriva direktni, pozivni i javno nadmetanje:

1. `nacrt` – sačinjavanje zahtjeva (član 16)
2. `pozivi_poslani` – kad je zaduženi radnik poslao poziv / objavio nabavku (pozivni i javno nadmetanje) :contentReference[oaicite:15]{index=15} :contentReference[oaicite:16]{index=16}
3. `ponude_zaprimljene` – kad je bar jedna ponuda stigla ili je komisija otvorila koverte (čl. 76 otvaranje ponuda) :contentReference[oaicite:17]{index=17}
4. **(samo za javno nadmetanje)** `javno_nadmetanje_u_toku` – faza gdje komisija vodi nadmetanje u do tri iteracije i bilježi ponude po kriteriju (član 35) :contentReference[oaicite:18]{index=18}
5. `odobreno` – nakon što je urađen izvještaj sa preporukom i odobren (za direktni i pozivni zaduženi radnik/komisija sačinjava izvještaj; za javno nadmetanje komisija sačinjava izvještaj o radu) :contentReference[oaicite:19]{index=19}

- za direktni postupak: mogućnost generisanja narudžbenice sa elementima ugovora
- **referenca:** član 19, stav (14)–(16) :contentReference[oaicite:20]{index=20}

### 2.8 Komunikacija (pitanja i odgovori) na tender
- u Uputstvu postoji mehanizam da zaduženi radnik dostavi primjedbe/sugestije ili vrati zahtjev na korekciju – to u app-u modelujemo kao komentare na nabavku
- u PoC-u dodati entitet “Komentar/KomunikacijaNabavke”
- polja: autor, tekst, datum/vrijeme, (opciono) parent_id
- koristi se kad zaduženi radnik vraća zahtjev na korekciju ili kad služba želi zabilježiti pojašnjenje
- **referenca:** član 16, stav (6)–(9) – vraćanje zahtjeva i dostavljanje primjedbi/sugestija :contentReference[oaicite:21]{index=21}

### 2.9 Osnovni audit
- aplikacija treba da sačuva: ko je promijenio status, kada i sa kog statusa na koji
- razlog: u članu 12 su jasno pobrojane odgovornosti (OPNZ, ID EFP, komisija) i ko za šta odgovara
- **referenca:** član 12 – odgovornosti u procesu nabavke (dovoljno je imati trag) :contentReference[oaicite:22]{index=22}

## 3. Tehničke pretpostavke za PoC
- Laravel 10+ aplikacija
- korištenje JSON konfiguracije koju aplikacija već ima za generisanje entiteta: **Nabavka**, **Dobavljac**, **PozivNabavke**, **PonudaNabavke**, **KomunikacijaNabavke**
- jedna baza (MySQL/PostgreSQL)
- seed podataka: min. 25 redova
- jednostavan web UI (Blade ili Vue/Inertia)
- **referenca za postupanje i rokove:** član 19 (direktni), član 27 / član 76 (otvaranje ponuda i rad komisije), član 35 (tok javnog nadmetanja) – PoC ih ne mora automatizovati, samo omogućiti polja :contentReference[oaicite:23]{index=23} :contentReference[oaicite:24]{index=24}

## 4. Van obuhvata PoC-a
- višestepeno odobravanje po iznosu (član 13 – ovlašteni organi za odobrenje)
- online nabavke (član 22–25)
- reklamno-promotivne nabavke (član 21)
- posebni postupci i pregovarački u punom profilu (član 28–30)
- integracije sa OWIS/PS modulom (u PoC-u ručno unosimo ono što se inače nosi preko OWIS-a)

---

# Specifikacija razvoja aplikacije za BHT javne nabavke (PoC fokus)

## 1. Uloge (roles)

### 1.1 OPNZ (Ovlašteni podnosilac nabavke)
- pokreće i priprema nabavku
- smije: `procurement.create_draft`, `procurement.update_draft`, `procurement.submit_for_check`, `procurement.add_qa`
- ne smije: slati pozive, evidentirati ponude, odobravati

### 1.2 NABAVKE (SN/EFP – služba za nabavke)
- vodi postupak, provjerava usklađenost
- smije: `procurement.return_for_fix`, `procurement.mark_plan_checked`,  
  `procurement.select_procedure`, `procurement.send_invitations`,  
  `procurement.receive_offer`, `procurement.change_status`, `procurement.add_qa`
- ne smije: konačno odobriti

### 1.3 DIREKTOR
- završno odobravanje
- smije: `procurement.approve`, `procurement.reject`, `procurement.add_qa`

### 1.4 READ_ONLY
- samo čitanje

## 2. Akcije koje pozivamo iz kontrolera

### 2.1 CreateProcurementAction (`procurement.create_draft`)

* **Opis (BS):** kreira novu nabavku i postavlja je u početni status `nacrt`.
* **Ko poziva:** OPNZ.
* **Rezultat:** u bazi postoji novi predmet nabavke koji OPNZ može dalje popunjavati.

### 2.2 UpdateProcurementDraftAction (`procurement.update_draft`)

* **Opis (BS):** mijenja postojeću nabavku dok je još u nacrtu ili vraćena na doradu (izmjena opisa, vrijednosti, OJ...).
* **Ko poziva:** OPNZ ili NABAVKE (ako koriguje).
* **Rezultat:** podaci na nabavci su ažurirani i zabilježen je korisnik koji je mijenjao.

### 2.3 SubmitProcurementForCheckAction (`procurement.submit_for_check`)

* **Opis (BS):** OPNZ predaje gotov nacrt službi nabavke da ga pregleda.
* **Ko poziva:** OPNZ.
* **Rezultat:** nabavka dobija status tipa `na_provjeri` i vidljiva je službi nabavke za dalje korake.

### 2.4 ReturnProcurementForFixAction (`procurement.return_for_fix`)

* **Opis (BS):** služba nabavke vraća zahtjev nazad OPNZ-u jer nešto nedostaje ili nije ispravno.
* **Ko poziva:** NABAVKE.
* **Rezultat:** status ide u `vraceno_na_doradu` i uz to se upisuje komentar sa razlogom vraćanja.

### 2.5 MarkPlanCheckedAction (`procurement.mark_plan_checked`)

* **Opis (BS):** označava da je provjeren plan/sredstva i da je nabavka usklađena sa uputstvom.
* **Ko poziva:** NABAVKE.
* **Rezultat:** polje "plan_provjeren" je postavljeno i sistem dozvoljava izbor postupka.

### 2.6 SelectProcurementProcedureAction (`procurement.select_procedure`)

* **Opis (BS):** bira koji postupak se primjenjuje na ovu nabavku (direktni, pozivni, otvoreni, pregovaracki).
* **Ko poziva:** NABAVKE.
* **Rezultat:** na nabavci je upisan tip postupka i status prelazi u fazu "postupak izabran".

### 2.7 SendInvitationsAction (`procurement.send_invitations`)

* **Opis (BS):** dodaje odabrane dobavljače i označava da su pozivi poslani (posebno za pozivni postupak).
* **Ko poziva:** NABAVKE.
* **Rezultat:** kreirani su zapisi poziva i nabavka prelazi u status `pozivi_poslani`.

### 2.8 ReceiveOfferAction (`procurement.receive_offer`)

* **Opis (BS):** evidentira da je od određenog dobavljača stigla ponuda za ovu nabavku.
* **Ko poziva:** NABAVKE.
* **Rezultat:** postoji ponuda vezana za nabavku; kada postoji bar jedna ponuda, može se ići na naredni status.

### 2.9 ChangeProcurementStatusAction (`procurement.change_status`)

* **Opis (BS):** kontrolisano mijenja status nabavke kroz PoC tok (npr. iz poziva u ponude, iz ponuda u odobreno).
* **Ko poziva:** NABAVKE.
* **Rezultat:** nabavka dobija novi status, a promjena se bilježi u auditu.

### 2.10 AddProcurementCommentAction (`procurement.add_qa`)

* **Opis (BS):** dodaje pitanje ili odgovor (komentar) u toku postupka nabavke, da sve bude zabilježeno.
* **Ko poziva:** OPNZ, NABAVKE, DIREKTOR.
* **Rezultat:** nova stavka u komunikaciji/Q&A listi vezana za nabavku.

### 2.11 ApproveProcurementAction (`procurement.approve`)

* **Opis (BS):** označava da je nabavka odobrena od strane odgovorne osobe.
* **Ko poziva:** DIREKTOR.
* **Rezultat:** status nabavke je `odobreno` i dalje se može ići na narudžbu/isporuku.

### 2.12 RejectProcurementAction (`procurement.reject`)

* **Opis (BS):** odbija ili vraća nabavku na doradu sa navedenim razlogom.
* **Ko poziva:** DIREKTOR.
* **Rezultat:** nabavka se vraća u `vraceno_na_doradu` i komentar je vidljiv svima u postupku.
