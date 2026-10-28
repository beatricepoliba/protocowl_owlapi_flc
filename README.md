# ProtocOWL per OWLAPI

Progetto per l'esame di Formal Languages and Compilers (Politecnico di Bari, A.A. 2025/2026,
Prof. Floriano Scioscia).

Autori: Beatrice Apollo, Mario Cassano

Il progetto parte da un parser e un renderer ProtocOWL già integrati in OWLAPI, scritti per una
versione precedente del formato. Li abbiamo aggiornati alla specifica v1.0 fino a leggere e
scrivere correttamente home.owl e pizza.owl, poi li abbiamo estesi ai costrutti delle 100
ontologie ORE 2014 del dataset, compresa la lettura della variante MIS_128 con i frame Reset.
Sul dataset abbiamo eseguito l'analisi di copertura, il round-trip completo e il benchmark
rispetto alla sintassi Functional.

La relazione è in `report/Report_FLC.pdf` (nella stessa cartella c'è anche il file Word).

## Contenuto

- `lib/src/main/java/it/poliba/sisinflab/protocowl/`: parser (`Parser.java`), renderer
  (`Renderer.java`), costanti del formato e classi di integrazione con OWLAPI
- `lib/src/test/java/`: test di parser e renderer, analisi di copertura, round-trip sul dataset
  e classi usate dal benchmark
- `lib/src/test/resources/`: home e pizza in sintassi Functional e in ProtocOWL
- `run_benchmark.sh`: script del benchmark
- `benchmark/metadata.csv`: elenco delle 100 ontologie del dataset
- risultati: `benchmark_results.csv`, `benchmark_environment.txt`, `roundtrip_sizes.csv` e
  `lib/coverage_report.csv`

## Esecuzione

Serve un JDK tra 21 e 25; Gradle non va installato perché c'è il wrapper. Il dataset non è nel repository: va messo in `../dataset_onto`, accanto alla
cartella del progetto, oppure indicato con la variabile `DATASET_DIR`. Se nella cartella del
dataset c'è un `metadata.csv` con tutte le ontologie ORE, bisogna impostare anche
`METADATA_FILE=benchmark/metadata.csv`.

```bash
./gradlew --no-daemon :lib:test            # test di parser e renderer
./gradlew --no-daemon :lib:coverageTest    # copertura sul dataset
./gradlew --no-daemon :lib:datasetTest     # round-trip sul dataset
python3 benchmark/test_run_benchmark.py    # test dello script di benchmark
bash ./run_benchmark.sh                    # benchmark
```

Il benchmark riesegue prima i tre gruppi di test, poi misura ogni combinazione in una JVM
separata e sostituisce il CSV solo se la validazione finale va a buon fine. Richiede GNU time:
su macOS si installa con `brew install gnu-time` e si indica con `export TIME_CMD=gtime`.

## Risultati

I risultati inclusi sono stati misurati su un MacBook Pro con Apple M3 Pro e 18 GiB di RAM;
sistema operativo, JDK e opzioni JVM sono riportati in `benchmark_environment.txt`.
