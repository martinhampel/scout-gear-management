# Návrh aplikácie pre správu materiálu skautského zboru

---

## 1. Prehľad

Aplikácia bude slúžiť na správu materiálu skautského zboru, umožní sledovať stav inventára, spravovať požičanie a rezervácie položiek, evidovať históriu a vykonávať inventúry. Aplikácia bude škálovateľná, aby ju mohli v budúcnosti používať aj ďalšie zbory.

---

## 2. Technológie

### Frontend:
- **Framework**: Vue.js (alebo Nuxt.js pre server-side rendering)
- **UI Knižnica**: Material UI (prípadne TailwindCSS alebo Vuetify pre rýchly dizajn UI)
- **Autentifikácia**: Firebase Authentication pre bezpečné prihlasovanie a správu užívateľov.

### Backend:
- **Databáza**: PostgreSQL
- **API**: Node.js s Express alebo NestJS pre rýchle vytváranie REST API endpointov.
- **Kontajnerizácia**: Docker pre ľahšiu správu a nasadenie aplikácie.

### Nasadenie a infraštruktúra:
- **Hostovanie**: DigitalOcean alebo AWS pre server a databázu.
- **CI/CD**: GitHub Actions alebo GitLab CI pre automatické nasadzovanie.

---

## 3. Funkčné požiadavky

### 3.1. Správa položiek
- Pridávanie, aktualizovanie a mazanie položiek inventára.
- Kategorizácia položiek podľa typu, použitia a príslušenstva.

### 3.2. Prehľad vybavenia
- Hlavná stránka zobrazí kategórie položiek a počet voľných/zapožičaných položiek v každej kategórii.
- Možnosť filtrovania podľa dostupnosti, stavu a miesta uloženia.

### 3.3. Správa požičania
- Možnosť požičať a vrátiť položky.
- Evidencia kto si položku požičal a na aké obdobie.

### 3.4. Notifikácie
- **Požiadavky na požičanie**: Administrátor dostane notifikáciu (email alebo push notifikáciu) o novej požiadavke na požičanie materiálu. Používateľ dostane potvrdenie o úspešnom požičaní.
- **Pripomienky na vrátenie**: Automatické notifikácie pre používateľov 24 hodín pred termínom vrátenia materiálu.
- **Upozornenia na prekročenie doby**: Používateľ dostane upozornenie, ak prekročí stanovenú dobu požičania. Administrátor dostane informáciu o oneskorenom vrátení.
- **Notifikácie pre administrátorov**: Administrátori dostávajú upozornenia na nové rezervácie a položky, ktoré potrebujú opravu alebo údržbu.

### 3.5. Inventúra a údržba
- Pravidelná kontrola stavu materiálu.
- Záznamy o opravách alebo likvidácii vybavenia.

### 3.6. Užívateľské role a prístupy
- **Administrátor**: Správa celého systému, pridávanie/vymazávanie položiek, úprava údajov.
- **Používateľ**: Možnosť rezervovať, požičať a vidieť dostupné vybavenie.

---

## 4. Nefunkčné požiadavky

- **Škálovateľnosť**: Aplikácia musí byť schopná spravovať viacero skautských zborov s oddelenými inventármi.
- **Výkonnosť**: Real-time aktualizácia inventára, aj pri väčšom objeme dát.
- **Bezpečnosť**: Firebase Authentication na autentifikáciu, šifrovanie citlivých údajov.
- **Zálohovanie**: Automatizované zálohovanie databázy na dennej báze.

---

## 5. Návrh databázy

### Tabuľky:

1. **users**:
    - `id` (PK)
    - `email`
    - `role` (admin/user)
    - `created_at`
    - `updated_at`

2. **items**:
    - `id` (PK)
    - `name`
    - `category`
    - `quantity`
    - `barcode` (možné budúce rozšírenie na skenovanie čiarových kódov)
    - `status` (na sklade, vypožičané)
    - `created_at`
    - `updated_at`

3. **borrowed_items**:
    - `id` (PK)
    - `user_id` (FK na users)
    - `item_id` (FK na items)
    - `borrowed_date`
    - `return_date`
    - `status` (vypožičané, vrátené)

4. **reservations** (možné rozšírenie):
    - `id` (PK)
    - `user_id` (FK na users)
    - `item_id` (FK na items)
    - `reservation_date`
    - `status` (aktívna, zrušená)

---

## 6. Návrh UI

### 6.1. Hlavná stránka (Dashboard)
- Zobrazenie kategórií s počtom dostupných položiek.
- Vyhľadávacie pole na rýchle nájdenie položiek.
- Rýchly prístup k sekciám "Požičané položky" a "Inventúra".

### 6.2. Stránka kategórie
- Zobrazenie položiek v danej kategórii.
- Filtrovanie podľa stavu (dostupné, zapožičané, v oprave) a miesta uloženia.

### 6.3. Detail položky
- Zobrazenie detailných informácií o položke (názov, stav, história používania).
- Tlačidlá pre "Požičať", "Vrátiť", "Označiť ako poškodené".

### 6.4. Správa požičania
- Formulár pre požičanie položiek.
- Možnosť nastavenia termínu návratu a upozornenia na prekročenie doby požičania.

### 6.5. Inventúra a údržba
- Možnosť označovať položky, ktoré boli skontrolované alebo opravené.
- Vytváranie reportov o stave materiálu.

---

## 7. Zálohovanie a nasadenie

- Automatizované zálohovanie databázy (napríklad cez cron job).
- Použitie CI/CD na plynulé nasadenie aplikácie na server (GitHub Actions alebo GitLab CI).
- Nasadenie na cloudovú platformu (DigitalOcean, AWS).
