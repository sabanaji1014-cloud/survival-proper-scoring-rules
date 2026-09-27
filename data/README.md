# Data

The three datasets are the ones used in Yanagisawa (2023). All of them are public and ship with R packages; they were exported to CSV without changes.

| File | R package | Description | Time / event columns |
|---|---|---|---|
| `flchain.csv` | [`survival`](https://cran.r-project.org/package=survival) | Serum free light chain study: 7,874 residents of Olmsted County, Minnesota, followed for mortality. | `futime` (days), `death` (1 = died) |
| `prostateSurvival.csv` | [`asaur`](https://cran.r-project.org/package=asaur) | 14,294 men with localized prostate cancer (SEER-Medicare). | `survTime` (months), `status` (0 = censored, 1 = prostate-cancer death, 2 = other death) |
| `support.csv` | [`casebase`](https://cran.r-project.org/package=casebase) | SUPPORT study: 9,104 seriously ill hospitalized adults. | `d.time` (days), `death` (1 = died) |

## Preprocessing used in the notebook

- `flchain`: rows with missing `creatinine` are removed (6,524 rows remain); `chapter` (cause-of-death chapter) is dropped because it is only known after death.
- `prostateSurvival`: any death (`status > 0`) is treated as the event.
- `support`: `slos` (days from study entry to discharge) is dropped.
- Missing numeric values → median; categorical variables → dummy variables; survival times of 0 → 0.001.

## How the CSV files were made

```r
library(survival); library(asaur); library(casebase)

write.csv(survival::flchain, "flchain.csv", row.names = FALSE)
write.csv(asaur::prostateSurvival, "prostateSurvival.csv", row.names = FALSE)
write.csv(casebase::support, "support.csv", row.names = FALSE)
```

## Original sources

- Dispenzieri, A. et al. (2012). Use of nonclonal serum immunoglobulin free light chains to predict overall survival in the general population. *Mayo Clinic Proceedings*, 87(6), 517–523.
- Lu-Yao, G. L. et al. (2009). Outcomes of localized prostate cancer following conservative management. *JAMA*, 302(11), 1202–1209.
- Knaus, W. A. et al. (1995). The SUPPORT prognostic model: objective estimates of survival for seriously ill hospitalized adults. *Annals of Internal Medicine*, 122(3), 191–203.
