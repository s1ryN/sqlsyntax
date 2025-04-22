# Databáze

## Charakteristika
Databáze je uspořádaná sbírka dat, která umožňuje:
- **Ukládání**: bezpečné uložení velkého množství informací  
- **Vyhledávání**: rychlé dotazy nad daty  
- **Správu**: přidávání, aktualizaci a mazání dat  
- **Integritu**: zachování konzistence a správnosti dat  

## Druhy databází
- **Hierarchické**: data ve stromové struktuře  
- **Síťové**: data s mnohonásobnými vazbami  
- **Relační**: tabulky (relace) s předem definovanými vazbami  
- **Objektově orientované**: data jako objekty s metodami  
- **NoSQL**  
  - *Dokumentové* (např. MongoDB)  
  - *Sloupcové* (např. Cassandra)  
  - *Key–Value* (např. Redis)  
  - *Grafové* (např. Neo4j)  

## Princip a použití
1. **Definice schématu**: návrh struktury (tabulky, sloupce, typy)  
2. **Ukládání dat**: transakce pro bezpečné zápisy  
3. **Dotazování**: SQL / API pro získání výsledků  
4. **Zálohování a obnova**: prevence ztráty dat  

## Systémy pro řízení báze dat (SŘBD)
Software, který zajišťuje:
- Správu schématu
- Zpracování dotazů
- Transakční zpracování
- Řízení přístupu  
příklady: MySQL, PostgreSQL, Oracle DB, MS SQL Server

## Databázové systémy (DBS)
Komplexní ekosystém kolem SŘBD zahrnující:
- Klientské nástroje (administrace, vizualizace)
- Middleware (aplikace, ORM knihovny)
- Zálohovací a replikční modul
- Monitorovací a ladicí nástroje

---

# Relační databáze

## Relace
- **Tabulka** s řádky (záznamy) a sloupci (atributy)  
- Každý řádek je jedinečný, každý sloupec má konzistentní datový typ  

## ER diagramy
- **Entita**: objekt (tabulka)  
- **Vazba**: spojení mezi entitami  
- **Atribut**: vlastnost entity  
- **Cardinalita**: 1:1, 1:N, N:M  

## Druhy klíčů
- **Primární klíč (PK)**: jednoznačná identifikace záznamu  
- **Cizí klíč (FK)**: odkaz na PK jiné tabulky  
- **Kandidátní klíč**: potenciální PK  
- **Superklíč**: libovolná kombinace, která zaručuje unikátnost  
- **Alternativní klíč**: kandidát, který není vybrán jako PK  

## ACID transakce
1. **Atomicita**: všechny úkony proběhnou nebo žádný  
2. **Konzistence**: transakce mění data pouze z jednoho validního stavu do druhého  
3. **Izolace**: transakce se navzájem neovlivňují  
4. **Trvalost (Durability)**: po potvrzení (COMMIT) jsou změny trvalé  

---

# Modely databáze

## Konceptuální model
- Abstraktní schéma nezávislé na technologiích  
- ER diagramy, definiční UML  

## Logický model
- Převod konceptuálního do tabulek a vazeb  
- Bez ohledu na fyzickou implementaci  
- Definuje tabulky, sloupce, omezení  

## Fyzický model
- Implementace na konkrétním SŘBD  
- Indexy, fyzické soubory, partioning  
- Optimalizace výkonu  

---

# SQL (Structured Query Language)

## Charakteristika
- Standardizovaný jazyk pro relační SŘBD  
- Dělení příkazů:
  - DDL (Data Definition Language) – CREATE, ALTER, DROP  
  - DML (Data Manipulation Language) – SELECT, INSERT, UPDATE, DELETE  
  - DCL (Data Control Language) – GRANT, REVOKE  
  - TCL (Transaction Control Language) – COMMIT, ROLLBACK  

## Základní syntaxe
```sql
-- Výběr sloupců
SELECT sloupec1, sloupec2
FROM tabulka
WHERE podmínka
ORDER BY sloupec DESC
LIMIT 10;
```

## CRUD příklady
- **CREATE (INSERT)**
  ```sql
  INSERT INTO uzivatele (jmeno, email, vek)
  VALUES ('Jan Novak', 'jan@example.com', 30);
  ```
- **READ (SELECT)**
  ```sql
  SELECT * FROM uzivatele
  WHERE vek > 18;
  ```
- **UPDATE**
  ```sql
  UPDATE uzivatele
  SET email = 'novy@example.com'
  WHERE id = 1;
  ```
- **DELETE**
  ```sql
  DELETE FROM uzivatele
  WHERE id = 2;
  ```

---

## JOIN a VIEW

### JOIN
- **INNER JOIN**: pouze shodné řádky  
  ```sql
  SELECT o.id, u.jmeno
  FROM objednavky o
  INNER JOIN uzivatele u
    ON o.uzivatel_id = u.id;
  ```
- **LEFT (OUTER) JOIN**: všechny z levé + shodné z pravé  
- **RIGHT (OUTER) JOIN**: všechny z pravé + shodné z levé  
- **FULL (OUTER) JOIN**: kombinace obou  
- **CROSS JOIN**: kartézský součin  

### VIEW
- **Virtualní tabulka** definovaná SELECTem  
  ```sql
  CREATE VIEW aktivni_uzivatele AS
  SELECT id, jmeno, email
  FROM uzivatele
  WHERE aktivni = TRUE;
  ```
- Použití:
  ```sql
  SELECT * FROM aktivni_uzivatele;
  ```

---

## Agregační klauzule, podmínky a řazení

### Agregační funkce
- `COUNT()`, `SUM()`, `AVG()`, `MIN()`, `MAX()`

### GROUP BY a HAVING
```sql
SELECT uzivatel_id, COUNT(*) AS pocet_obj
FROM objednavky
GROUP BY uzivatel_id
HAVING COUNT(*) > 5;
```

### WHERE vs. HAVING
- `WHERE` filtruje před agregací  
- `HAVING` filtruje po agregaci  

### ORDER BY
```sql
SELECT produkt, SUM(mnozstvi) AS celkem
FROM objednavky
GROUP BY produkt
ORDER BY celkem DESC;
```

