# Day 2: Paper Notes and Gap Section

Project: ids-cross-dataset-generalisation

Research question: Do machine-learning intrusion detection models trained on one network dataset still detect attacks when tested on a different dataset?

---

## Paper 1: Dataset problems in CICIDS2017

**Citation:** Engelen, G., Rimmer, V., & Joosen, W. (2021). Troubleshooting an Intrusion Detection Dataset: the CICIDS2017 Case Study. *2021 IEEE Security and Privacy Workshops (SPW)*, pp. 7-12.

1. **Flow construction errors**
   - Finding: The CICFlowMeter tool ended a flow at the first FIN packet, which broke TCP connections into extra "TCP appendix" rows. These made up 25.9% of the dataset.
   - Why it matters: A model can learn from these meaningless rows instead of real attack behaviour.
   - My project: I must check data quality before I blame a performance drop on distribution shift.

2. **Weak labelling**
   - Finding: Labels were given using only IP addresses and a time window. Flows with no attack data (empty or failed connections) were still labelled as attacks.
   - Why it matters: The ground truth is partly wrong, so accuracy numbers are not fully reliable.
   - My project: I need to check label quality in both datasets before I compare them.

3. **Shortcut learning**
   - Finding: The model used features unrelated to the attack (for example, the presence of RST packets for Bot traffic). The authors say columns such as Flow ID, IP addresses, ports and timestamp should be removed before training.
   - Why it matters: A shortcut that works inside one dataset is unlikely to work in another.
   - My project: I will remove identifier columns and check feature importance.

4. **Overall scores hide weak classes**
   - Finding: The weighted average was 0.99 in all three dataset versions, but some classes changed a lot (Bot F1 0.60 to 0.99, Web Attack XSS F1 0.18 to 0.79).
   - Why it matters: One total number is not enough for an unbalanced dataset.
   - My project: I will report precision, recall and F1 for each attack class.

5. **Attack diversity is not documented, and there is no cross-dataset test (the gap)**
   - Finding: The authors say it is not known whether attack tools were run with fixed or varying settings, and a model trained on one configuration may not work on a slightly different attack (Section III-C-2). Their own experiments use only a 75/25 split of one dataset.
   - Why it matters: The paper raises the generalisation concern but does not measure it.
   - My project: Training on one dataset and testing on another measures exactly this.

---

## Paper 2: Survey of network intrusion datasets

**Citation:** Goldschmidt, P., & Chudá, D. (2025). Network intrusion datasets: A survey, limitations, and recommendations. *Computers & Security*, 156, 104510. https://doi.org/10.1016/j.cose.2025.104510

1. **Datasets exist, but good practice is missing**
   - Finding: The survey reviews 89 public datasets across 13 properties. The authors argue that datasets exist but the community is unaware of them and of best practices, so many studies report biased and over-optimistic results.
   - Why it matters: High scores in the literature should be questioned.
   - My project: My work aims to give a more honest evaluation.

2. **Every network is different**
   - Finding: The survey describes inter-network variability: traffic patterns differ between networks, so the same attack can look different in another network. Models also assume training and future data come from the same distribution, which often fails.
   - Why it matters: This is the theory behind why a model may fail on another dataset.
   - My project: It supports my research question directly.

3. **Different feature sets block transfer between datasets**
   - Finding: There is no agreed feature set. Datasets use their own features, from about 10 to over 100. The authors say this hampers transferability and generalisability between datasets.
   - Why it matters: Cross-dataset testing is technically difficult.
   - My project: I need to find shared features, or convert both datasets to a common format.

4. **Poor train-test splits can inflate results**
   - Finding: Many datasets have no official train-test split, so users create their own. Splits that ignore time can inflate performance and make studies hard to compare.
   - Why it matters: Results from different papers may not be comparable.
   - My project: I will describe my split clearly and think about temporal bias.

5. **Popular datasets have known errors and limits**
   - Finding: The survey lists errors in CIC-IDS2017 and CSE-CIC-IDS2018 (duplicates, flow errors, labelling issues, deprecated attack tools). It also says lab-made traffic may not match real networks, and that no dataset is perfect or covers all attacks.
   - Why it matters: This confirms Engelen et al. from a second source.
   - My project: I should state the limits of CICIDS2017 and UNSW-NB15 openly in my proposal.

---

## Gap section (draft)

Machine learning models for intrusion detection often report very high accuracy, but most are tested on the same dataset they were trained on. Engelen et al. (2021) showed that CICIDS2017 contains flow-construction and labelling errors, and that models can learn shortcuts that high overall scores hide. Goldschmidt and Chudá (2025) add that networks differ from each other, that datasets use incompatible feature sets, and that this limits how well results transfer between datasets. However, neither work measures how a model trained on one dataset performs on a different one. This project addresses that gap by training on one dataset, testing on another, and reporting results for each attack class.

---

## Decisions for my experiments (from the papers)

- Remove identifier columns (Flow ID, IP addresses, timestamp) before training.
- Report per-class precision, recall and F1, not only overall accuracy.
- Check feature importance as a sanity check for shortcut learning.
- Find a shared feature set between CICIDS2017 and UNSW-NB15 before cross-dataset testing.
- State dataset limitations and the exact data version used (original or corrected CICIDS2017).
