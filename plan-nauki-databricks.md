# Plan nauki Databricks — rozmowa na Junior Data Engineer (Azure)

**Cel:** zrozumieć teorię stojącą za kodem w Spark/Databricks i umieć ją wytłumaczyć własnymi słowami na rozmowie.
**Czas:** 14 dni × ok. 1,5–2 h dziennie. Jeśli rozmowa jest wcześniej, skup się na dniach oznaczonych ⭐.
**Środowisko do ćwiczeń:** Databricks Free Edition (darmowe konto) albo trial Azure Databricks.

> 📚 Linki prowadzą głównie do dokumentacji **Azure Databricks** (learn.microsoft.com), bo rozmowa dotyczy Azure. Tematy czysto sparkowe mają linki do dokumentacji **Apache Spark**. Wszystkie strony są po angielsku (dobra praktyka słownictwa przed rozmową 😉). Jeśli któryś link przekieruje albo nie zadziała, wpisz tytuł strony w wyszukiwarkę na learn.microsoft.com.

---

## Tydzień 1 — Fundamenty

### ⭐ Dzień 1 — Czym jest Databricks i Lakehouse
- Co to jest Databricks i po co firmie (jedna platforma do danych, analityki i ML).
- Architektura **Lakehouse** = data lake (tanie przechowywanie plików) + funkcje hurtowni (transakcje, schemat, SQL).
- Data Lake vs Data Warehouse vs Lakehouse — umieć porównać w 3 zdaniach.
- **Control plane** (UI, notebooki, zarządzanie) vs **compute plane** (klastry, które przetwarzają dane).
- ✅ Ćwiczenie: załóż konto, utwórz notebook, uruchom `spark.range(10).show()`.

🔗 **Dokumentacja:**
- [What is Azure Databricks?](https://learn.microsoft.com/azure/databricks/introduction/)
- [What is a data lakehouse?](https://learn.microsoft.com/azure/databricks/lakehouse/)
- [Azure Databricks architecture overview](https://learn.microsoft.com/azure/databricks/getting-started/overview)
- [Databricks Free Edition](https://learn.microsoft.com/azure/databricks/getting-started/free-edition)

### ⭐ Dzień 2 — Architektura Sparka
- Driver, executory, cluster manager — kto co robi.
- Job → Stage → Task — jak Spark dzieli pracę.
- Partycje — dlaczego dane są dzielone na kawałki i jak to wpływa na równoległość.
- ✅ Ćwiczenie: `df.rdd.getNumPartitions()`, `repartition()` vs `coalesce()` — sprawdź różnicę.

🔗 **Dokumentacja:**
- [Apache Spark — Cluster Mode Overview](https://spark.apache.org/docs/latest/cluster-overview.html) (driver, executory, joby, stage’y, taski)
- [Apache Spark on Azure Databricks](https://learn.microsoft.com/azure/databricks/spark/)

### ⭐ Dzień 3 — Lazy evaluation, transformacje i akcje
- **Transformacje** (`select`, `filter`, `join`, `groupBy`) są leniwe — nic się nie dzieje.
- **Akcje** (`show`, `count`, `collect`, `write`) uruchamiają obliczenia.
- **Narrow** vs **wide** transformations → wide powoduje **shuffle**.
- DAG i **Catalyst Optimizer**.
- ✅ Ćwiczenie: `df.explain(True)` — znajdź w planie `Exchange` (to shuffle).

🔗 **Dokumentacja:**
- [Spark — RDD Operations (transformations & actions, lazy evaluation)](https://spark.apache.org/docs/latest/rdd-programming-guide.html#rdd-operations)
- [Spark — Shuffle operations](https://spark.apache.org/docs/latest/rdd-programming-guide.html#shuffle-operations)
- [Spark SQL, DataFrames and Datasets Guide](https://spark.apache.org/docs/latest/sql-programming-guide.html)

### Dzień 4 — DataFrame API i Spark SQL
- Czytanie danych: CSV, JSON, Parquet (`spark.read.format(...)`).
- Operacje: `select`, `withColumn`, `filter`, `groupBy().agg()`, `join`.
- To samo w Spark SQL (`createOrReplaceTempView` + `spark.sql(...)`).
- Typy joinów: inner, left, anti, semi.
- ✅ Ćwiczenie: wczytaj dataset z `/databricks-datasets/`, zrób agregację w PySpark i w SQL.

🔗 **Dokumentacja:**
- [Tutorial: Load and transform data using Apache Spark DataFrames](https://learn.microsoft.com/azure/databricks/getting-started/dataframes)
- [PySpark DataFrame API reference](https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql/dataframe.html)
- [SQL — JOIN (typy joinów)](https://learn.microsoft.com/azure/databricks/sql/language-manual/sql-ref-syntax-qry-select-join)

### ⭐ Dzień 5 — Delta Lake (najważniejszy temat!)
- Delta = pliki Parquet + **transaction log** (`_delta_log`).
- **ACID** — co oznacza każda litera.
- **Time travel** (`VERSION AS OF`), `DESCRIBE HISTORY`.
- **Schema enforcement** vs **schema evolution** (`mergeSchema`).
- **MERGE INTO** (upsert) — umieć napisać z pamięci.
- ✅ Ćwiczenie: tabela Delta → UPDATE → MERGE → powrót do starszej wersji.

🔗 **Dokumentacja:**
- [What is Delta Lake?](https://learn.microsoft.com/azure/databricks/delta/)
- [What are ACID guarantees on Azure Databricks?](https://learn.microsoft.com/azure/databricks/lakehouse/acid)
- [Work with table history (time travel)](https://learn.microsoft.com/azure/databricks/delta/history)
- [Schema enforcement](https://learn.microsoft.com/azure/databricks/tables/schema-enforcement)
- [Update table schema (schema evolution)](https://learn.microsoft.com/azure/databricks/delta/update-schema)
- [Upsert into a Delta table using MERGE](https://learn.microsoft.com/azure/databricks/delta/merge)

### Dzień 6 — Optymalizacja Delta
- Problem małych plików → `OPTIMIZE`.
- `ZORDER BY` vs **Liquid Clustering**.
- `VACUUM` — co usuwa i jaki ma związek z time travel.
- Partycjonowanie tabel — kiedy ma sens, a kiedy szkodzi.
- ✅ Ćwiczenie: `OPTIMIZE` + `DESCRIBE DETAIL` — porównaj liczbę plików.

🔗 **Dokumentacja:**
- [Optimize data file layout (OPTIMIZE)](https://learn.microsoft.com/azure/databricks/delta/optimize)
- [Data skipping and Z-order](https://learn.microsoft.com/azure/databricks/delta/data-skipping)
- [Use liquid clustering for tables](https://learn.microsoft.com/azure/databricks/delta/clustering)
- [Remove unused data files with VACUUM](https://learn.microsoft.com/azure/databricks/delta/vacuum)
- [When to partition tables](https://learn.microsoft.com/azure/databricks/tables/partitions)

### Dzień 7 — Powtórka + przerwa
- Przejrzyj notatki z dni 1–6.
- Odpowiedz na głos na pytania z części A.

---

## Tydzień 2 — Data Engineering i Azure

### ⭐ Dzień 8 — Architektura medalionowa
- **Bronze** (surowe) → **Silver** (oczyszczone) → **Gold** (agregaty biznesowe).
- Dlaczego tak się to robi.
- ✅ Ćwiczenie: mini pipeline bronze → silver → gold (np. sprzedaż klimatyzatorów 😉).

🔗 **Dokumentacja:**
- [What is the medallion lakehouse architecture?](https://learn.microsoft.com/azure/databricks/lakehouse/medallion)

### ⭐ Dzień 9 — Azure: przechowywanie i integracja
- **ADLS Gen2** — gdzie fizycznie leżą dane.
- Dostęp do danych: **storage credentials** + **external locations** (Unity Catalog), managed identity / access connector. Mount points to stare podejście.
- **Azure Data Factory** jako orkiestrator uruchamiający notebooki Databricks.
- **Azure Key Vault** + **secret scopes** — nigdy haseł w kodzie.
- ✅ Ćwiczenie: narysuj przepływ: źródło → ADF → ADLS (bronze) → Databricks → Gold → Power BI.

🔗 **Dokumentacja:**
- [Connect to Azure Data Lake Storage and Blob Storage](https://learn.microsoft.com/azure/databricks/connect/storage/azure-storage)
- [Create a storage credential for connecting to ADLS](https://learn.microsoft.com/azure/databricks/connect/unity-catalog/cloud-storage/storage-credentials)
- [Create an external location](https://learn.microsoft.com/azure/databricks/connect/unity-catalog/cloud-storage/external-locations)
- [Secret management (secret scopes, Key Vault)](https://learn.microsoft.com/azure/databricks/security/secrets/)
- [ADF: Transform data by running a Databricks notebook](https://learn.microsoft.com/azure/data-factory/transform-data-databricks-notebook)

### ⭐ Dzień 10 — Unity Catalog
- Hierarchia: **metastore → catalog → schema → table/view/volume**.
- Managed vs external tables — co się dzieje z danymi po `DROP TABLE`.
- Uprawnienia: `GRANT SELECT ON TABLE ... TO ...`.
- Data lineage.
- ✅ Ćwiczenie: utwórz catalog/schema i tabelę managed, obejrzyj ją w Catalog Explorer.

🔗 **Dokumentacja:**
- [What is Unity Catalog?](https://learn.microsoft.com/azure/databricks/data-governance/unity-catalog/)
- [Unity Catalog managed tables](https://learn.microsoft.com/azure/databricks/tables/managed)
- [Unity Catalog external tables](https://learn.microsoft.com/azure/databricks/tables/external)
- [Manage privileges in Unity Catalog](https://learn.microsoft.com/azure/databricks/data-governance/unity-catalog/manage-privileges/)
- [Data lineage in Unity Catalog](https://learn.microsoft.com/azure/databricks/data-governance/unity-catalog/data-lineage)

### Dzień 11 — Ładowanie przyrostowe i streaming
- Batch vs streaming; **Structured Streaming** — podstawy.
- **Auto Loader** (`cloudFiles`) — wczytywanie tylko nowych plików.
- Checkpointy — po co są.
- **Lakeflow Spark Declarative Pipelines** (wcześniej Delta Live Tables) — co to jest, czym są *expectations* (reguły jakości danych).
- ✅ Ćwiczenie: Auto Loader na folderze z JSON-ami, dorzuć nowy plik i uruchom ponownie.

🔗 **Dokumentacja:**
- [Structured Streaming concepts](https://learn.microsoft.com/azure/databricks/structured-streaming/)
- [Structured Streaming checkpoints](https://learn.microsoft.com/azure/databricks/structured-streaming/checkpoints)
- [What is Auto Loader?](https://learn.microsoft.com/azure/databricks/ingestion/cloud-object-storage/auto-loader/)
- [Lakeflow Spark Declarative Pipelines](https://learn.microsoft.com/azure/databricks/ldp)

### Dzień 12 — Klastry, joby i wydajność
- Typy compute: all-purpose vs jobs compute vs SQL warehouse vs serverless. Autoscaling, auto-termination (koszty!).
- **Photon** w jednym zdaniu.
- **Lakeflow Jobs** — taski, zależności, harmonogram.
- Wydajność: **broadcast join**, **cache/persist**, **data skew**, **AQE**.
- Spark UI — gdzie szukać wolnych stage’y.
- ✅ Ćwiczenie: job z 2 taskami (bronze → silver) uruchamiany z harmonogramu.

🔗 **Dokumentacja:**
- [Compute (typy klastrów)](https://learn.microsoft.com/azure/databricks/compute/)
- [What is Photon?](https://learn.microsoft.com/azure/databricks/compute/photon)
- [Lakeflow Jobs](https://learn.microsoft.com/azure/databricks/jobs/)
- [Adaptive query execution (AQE)](https://learn.microsoft.com/azure/databricks/optimizations/aqe)
- [Spark — Performance Tuning (broadcast join, cache, AQE, skew)](https://spark.apache.org/docs/latest/sql-performance-tuning.html)
- [Debugging with the Spark UI](https://learn.microsoft.com/azure/databricks/compute/troubleshooting/debugging-spark-ui)
- [Best practices for cost optimization](https://learn.microsoft.com/azure/databricks/lakehouse-architecture/cost-optimization/best-practices)

### Dzień 13 — Projekt na rozmowę
- Złóż wszystko w jeden mały projekt: pliki → Auto Loader → Delta bronze/silver/gold → job.
- Przygotuj **2-minutową opowieść** o projekcie (problem → rozwiązanie → czego się nauczyłeś).
- Opcjonalnie wrzuć kod na GitHub.

🔗 **Dokumentacja:**
- [Tutorial: Build an ETL pipeline with Lakeflow Jobs / Declarative Pipelines](https://learn.microsoft.com/azure/databricks/getting-started/data-pipeline-get-started)

### ⭐ Dzień 14 — Symulacja rozmowy
- Odpowiedz na głos na wszystkie pytania poniżej (najlepiej się nagraj).
- „Opowiedz o sobie” — przejście ze sprzedaży klimatyzacji do danych to atut (znasz biznes i rozmowy z klientem).
- Przygotuj 2–3 pytania do rekrutera.

---

## Pytania rekrutacyjne

### Część A — Spark i Delta
1. Czym różni się transformacja od akcji? Co to jest lazy evaluation?
2. Czym jest shuffle i dlaczego jest kosztowny?
3. Narrow vs wide transformation — podaj przykłady.
4. `repartition()` vs `coalesce()`?
5. Kiedy użyjesz broadcast join?
6. Co to jest data skew i jak sobie z nim radzić?
7. Czym jest Delta Lake i co daje ponad zwykły Parquet?
8. Jak działa time travel? Co ma z nim wspólnego `VACUUM`?
9. Schema enforcement vs schema evolution?
10. Napisz `MERGE INTO` dla upsertu klientów.

### Część B — Platforma i Azure
11. Czym jest Lakehouse?
12. Opisz architekturę medalionową.
13. Czym jest Unity Catalog? Managed vs external table?
14. Jak Databricks bezpiecznie łączy się z ADLS Gen2?
15. Jak zorganizujesz ładowanie przyrostowe nowych plików?
16. All-purpose compute vs jobs compute — kiedy który?
17. Jak obniżyć koszty w Databricks?
18. Jak ADF współpracuje z Databricks?

### Część C — SQL (często na live codingu)
19. `WHERE` vs `HAVING`.
20. `ROW_NUMBER()` vs `RANK()` vs `DENSE_RANK()`.
21. Znajdź duplikaty / usuń duplikaty, zostawiając najnowszy rekord.
22. Drugi najwyższy wynik w każdej grupie (window function lub CTE).

🔗 **Dokumentacja:** [Window functions w Databricks SQL](https://learn.microsoft.com/azure/databricks/sql/language-manual/sql-ref-window-functions)

---

## Ściąga na ostatni wieczór
| Pojęcie | Jedno zdanie |
|---|---|
| Lazy evaluation | Spark buduje plan i liczy dopiero przy akcji. |
| Shuffle | Przemieszczanie danych między executorami przez sieć — drogie. |
| Delta Lake | Parquet + log transakcji = ACID, time travel, MERGE. |
| Medallion | Bronze (surowe) → Silver (czyste) → Gold (biznes). |
| Unity Catalog | Centralne zarządzanie danymi i uprawnieniami: catalog.schema.table. |
| Auto Loader | Przyrostowe wczytywanie nowych plików z chmury. |
| OPTIMIZE | Łączy małe pliki w większe. |
| VACUUM | Usuwa stare pliki — ogranicza time travel. |
| Broadcast join | Mała tabela kopiowana do każdego executora — bez shuffle. |
| AQE | Spark poprawia plan w trakcie wykonania. |

---

## Materiały dodatkowe
- [Dokumentacja Azure Databricks — strona główna](https://learn.microsoft.com/azure/databricks/)
- [Databricks Academy — darmowe kursy](https://www.databricks.com/learn/training/home) (np. ścieżka Data Engineer Associate)
- Microsoft Learn — ścieżka szkoleniowa „Azure Databricks” dla data engineerów
- Książka: *Learning Spark, 2nd Edition* (rozdziały 1–3 i 7)
