# Proiect Baze de Date – Platforma de Microfinanțare

## Cuprins
1. [Descriere proiect](#descriere-proiect)  
2. [Descriere componente](#descriere-componente)  
3. [Descriere relații entități](#descriere-relații-entități)  
4. [Restricții și reguli](#restricții-și-reguli)

---

## I. Descriere proiect

Proiectul reprezintă o **platformă de microfinanțare**, unde utilizatorii pot avea două roluri principale:
- **Beneficiar** – solicită finanțare pentru diverse proiecte.  
- **Investitor** – finanțează proiectele disponibile.

### Modul de funcționare:
1. Un utilizator se înregistrează și primește un cont.
2. Dacă este **beneficiar**, creează un **împrumut**.
3. Împrumutul devine **public** și poate fi finanțat de mai mulți investitori.
4. **Investitorii** contribuie cu sume de bani.
5. După finanțare, beneficiarul începe să returneze banii în **rate**.
6. Sistemul urmărește **statusul împrumutului** și **istoricul tranzacțiilor**.

---

## II. Descriere componente

### 1. UTILIZATOR
- **Rol**: Păstrează datele utilizatorilor (pot fi investitori sau beneficiari).  
- **Cheie primară**: `id_utilizator`  
- **Chei străine**: `id_localitate` → `LOCALITATE(id_localitate)`

---

### 2. LOCALITATE
- **Rol**: Păstrează localitatea utilizatorilor.  
- **Cheie primară**: `id_localitate`  
- **Chei străine**: —  

---

### 3. CONT
- **Rol**: Contul financiar al utilizatorului.  
- **Cheie primară**: `id_cont`  
- **Chei străine**: `id_localitate` → `LOCALITATE(id_localitate)`  

---

### 4. TRANZACTIE
- **Rol**: Stochează operațiile financiare (depuneri, retrageri, investiții).  
- **Cheie primară**: `id_tranzactie`  
- **Chei străine**: `id_cont` → `CONT(id_cont)`

---

### 5. IMPRUMUT
- **Rol**: Cererile de finanțare ale utilizatorilor.  
- **Cheie primară**: `id_imprumut`  
- **Chei străine**: `id_beneficiar` → `UTILIZATOR(id_utilizator)`

---

### 6. CATEGORIE
- **Rol**: Încadrează împrumutul într-o categorie anume.  
- **Cheie primară**: `id_categorie`  
- **Chei străine**: —  

---

### 7. IMPRUMUT_CATEGORIE
- **Rol**: Tabel asociativ care implementează relația many-to-many între împrumut și categorie.  
- **Cheie primară**: (`id_imprumut`, `id_categorie`)  
- **Chei străine**:  
  - `id_imprumut` → `IMPRUMUT(id_imprumut)`  
  - `id_categorie` → `CATEGORIE(id_categorie)`

---

### 8. CONTRIBUTIE
- **Rol**: Investițiile utilizatorilor în împrumuturi.  
- **Cheie primară**: `id_contributie`  
- **Chei străine**:  
  - `id_imprumut` → `IMPRUMUT(id_imprumut)`  
  - `id_investitor` → `UTILIZATOR(id_utilizator)`

---

### 9. RAMBURSARE
- **Rol**: Stochează plățile efectuate după returnarea împrumuturilor.  
- **Cheie primară**: `id_rambursare`  
- **Chei străine**: `id_imprumut` → `IMPRUMUT(id_imprumut)`

---

### 10. PLAN_RAMBURSARE
- **Rol**: Ratele programate pentru un împrumut.  
- **Cheie primară**: `id_plan`  
- **Chei străine**: `id_imprumut` → `IMPRUMUT(id_imprumut)`  

---

### 11. STATUS_IMPRUMUT
- **Rol**: Istoric statusuri împrumut.  
- **Cheie primară**: `id_status`  
- **Chei străine**: `id_imprumut` → `IMPRUMUT(id_imprumut)`  

---

### 12. RECENZIE
- **Rol**: Părerea utilizatorilor despre împrumuturi.  
- **Cheie primară**: `id_recenzie`  
- **Chei străine**:  
  - `id_utilizator` → `UTILIZATOR(id_utilizator)`  
  - `id_imprumut` → `IMPRUMUT(id_imprumut)`

---

## III. Descriere relații entități

1. **UTILIZATOR – CONT (1:1)**  
   - Fiecare utilizator are un singur cont.  
   - Fiecare cont aparține unui singur utilizator.

2. **UTILIZATOR – IMPRUMUT (1:N)**  
   - Un utilizator (beneficiar) poate crea mai multe împrumuturi.  
   - Un împrumut aparține unui singur utilizator.

3. **UTILIZATOR – CONTRIBUTIE (1:N)**  
   - Un utilizator (investitor) poate realiza mai multe contribuții.  
   - Fiecare contribuție aparține unui singur utilizator.

4. **IMPRUMUT – CONTRIBUTIE (1:N)**  
   - Un împrumut poate avea mai mulți investitori.  
   - Fiecare contribuție este pentru un singur împrumut.

5. **IMPRUMUT – CATEGORIE (M:N)**  
   - Un împrumut poate aparține mai multor categorii.  
   - O categorie poate include mai multe împrumuturi.

6. **IMPRUMUT – RAMBURSARE (1:N)**  
   - Un împrumut este plătit în mai multe tranșe.  
   - Fiecare rambursare aparține unui împrumut.

7. **IMPRUMUT – PLAN_RAMBURSARE (1:N)**  
   - Definește ratele planificate ale împrumutului.  
   - Fiecare rată are o dată scadentă.

8. **IMPRUMUT – STATUS_IMPRUMUT (1:N)**  
   - Stochează istoricul statusurilor împrumutului.

9. **UTILIZATOR – RECENZIE (1:N)**  
   - Un utilizator poate adăuga mai multe recenzii.  
   - O recenzie aparține unui singur utilizator.

10. **LOCALITATE – UTILIZATOR (1:N)**  
    - O localitate poate avea mai mulți utilizatori.  
    - Un utilizator aparține unei singure localități.

---

## IV. Restricții și reguli

- Fiecare tabel are **cheie primară**.  
- Relațiile sunt definite prin **chei externe**.  
- **Emailul fiecărui utilizator** este unic.  
- `suma_ceruta` > 0  
- `suma_contributie` > 0  
- **Scorul recenziei** este un număr în intervalul [1,5].  
- Un **cont** aparține unui singur utilizator.  
- Un **împrumut** trebuie să aibă un beneficiar valid.  
- **Rambursările** se fac doar după finanțare.  
- **Contribuțiile** sunt permise doar dacă împrumutul este valid.
- <img width="636" height="520" alt="image" src="https://github.com/user-attachments/assets/6d7c566e-acd0-49da-aaa4-2cd50ff7b56b" />


---

