## Wyniki zapytań SQL

### Zapytanie 4ii: Testy, w których uczestniczyło więcej niż 30 pacjentów

**Zapytanie:**

```sql
SELECT
    t.test_type,
    COUNT(DISTINCT pt.patient_id) AS num_patients
FROM tests t
JOIN patient_test pt ON t.test_id = pt.test_id
GROUP BY t.test_type
HAVING COUNT(DISTINCT pt.patient_id) > 30;
```

**Wynik:**

| test_type | num_patients |
|---|---:|
| NGS-panel | 38 |
| SNP Array | 39 |

---

### Zapytanie 4iii: Średnia liczba wariantów dla każdego testu

**Zapytanie:**

```sql
SELECT
    t.test_type,
    ROUND(AVG(r_count.variant_count), 2) AS avg_variants
FROM (
    SELECT
        test_id,
        COUNT(*) AS variant_count
    FROM results
    GROUP BY test_id
) r_count
JOIN tests t ON r_count.test_id = t.test_id
GROUP BY t.test_type;
```

**Wynik:**

| test_type | avg_variants |
|---|---:|
| SNP Array | 3.00 |
| NGS-panel | 3.69 |
| WES | 3.53 |
| WGS | 3.61 |
