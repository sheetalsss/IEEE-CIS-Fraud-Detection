**Goal**
- Build fraud detection with real temporal validation

**Dataset** : IEEE-CIS, merged transaction + identity
* **TransactionDT**: timedelta from a given reference datetime (not an actual timestamp)
* **TransactionAMT**: transaction payment amount in USD
* **ProductCD**: product code, the product for each transaction
* **card1 - card6**: payment card information, such as card type, card category, issue bank, country, etc.
* **addr**: address
* **dist**: distance
* **P_ and (R__) emaildomain**: purchaser and recipient email domain
* **C1-C14**: counting, such as how many addresses are found to be associated with the payment card, etc. The actual meaning is masked.
* **D1-D15**: timedelta, such as days between previous transaction, etc.
* **M1-M9**: match, such as names on card and address, etc.
* **Vxxx**: Vesta engineered rich features, including ranking, counting, and other entity relations.

**Pipeline**
- Data loading & memory reduction
- Feature engineering
- Temporal train/val/test split
- Model training (LGBM/XGB/CatBoost)
- Threshold tuning
- Evaluation (PR-AUC, Recall@FPR)
- Deployment simulation (API + streaming replay)


**Considering cardinality check and mutual info overview**

Typical identity cluster:
```
(card1, card2, addr1, addr2, DeviceInfo, P_emaildomain)
```
1.     PRIMARY_ENTITY = 'card1'
2.     SECONDARY_ENTITY = ['card1','addr1']
3.     DEVICE_ENTITY = 'DeviceInfo'
4.     EMAIL_ENTITY = 'P_emaildomain'

- Missing values stats is present in ``data/processed/missing_summary.csv``
- Cardinality summary is present in ``data/processed/cardinality_summary.csv``
