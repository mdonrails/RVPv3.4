# RVPv3.4
POCT RVP &amp; CXR Analysis, with ED tracking data and BusinessObjects Order Data
1. ED Tracking Import & Cleaning
	* converted (sex, dispo) to factors → track.clean.Rda
	* removed duplicates→ track.dedup.Rda
2. Import & Cleaning BO Order Data→ bo.raw.Rda
	* converted (Dispo_BO, Order_Status, MnemonicName) to factors→ bo.clean.Rda
	* subset Mnemonic Name = ‘CXR’ only→ CXR.rda
	* subset Mnemonic Name != ‘CXR’ only→ rvp.Rda
3. Match ED Track w/ RVP
	* merged track.dedup.Rda w/ bo.clean.Rda, natural join (keep intersecting only)
	* rearranged and renamed columns→ rvp.track.clean.Rda
4. Match CXR Data w/. Tracking & RVP
	* dropped CXR unnecessary columns, factored (Order_Status, XR_Type), dropped duplicates→ cxr.dedup.Rda
	* left joined rvp.track.clean.Rda w/ cxr.dedup by FIN; created binary of CXR_completed→ rvp.track.cxr.Rda
5. Split Data by Mnemonic Name
* removed encounters w/ duplicate Encounter IDs and Mnemonic Name (same order 2x)→ rvp.track.cxr.dedup.Rda
* extracted datasets of each test only
	* subset Mnemonic Name = “Influenza” = only.poct.flu.Rda
	* subset Mnemonic Name = “RSV” = only.poct.rsv.Rda
	* 	subset Mnemonic Name = “Strep A (POCT)” = only.poct.strep.Rda
	* 	subset Mnemonic Name = “Respiratory” = only.lab.pcr.Rda
	* 	subset Mnemonic Name = “Rapid Strep A” = only.lab.strep.Rda
6. Merge POCT vs. Lab Data Sets
	* created df of POCT flu vs. Resp PCR, removing all encounters in which both tests were ordered→ df.flu.Rda
		* 1054 flu POCT, 981 Resp PCR
		* POCT flu w/o encounters w concurrent Resp PCR ordered→ flu.poct.dedup.Rda
		* Resp PCR w/o encounters w concurrent POCT flu ordered→ flu.lab.dedup.Rda
	* created df of POCT Flu vs. Resp PCR, before & after→ df.flu.beforeafter.Rda
		* via rbind of flu.poct.dedup.Rda & lab.pcr.before.Rda
7. Flu Analysis (OLD)
7.1 Flu Analysis Before & After (OLD)
8. Strep Analysis
9. RSV Analysis
10. Flu Order Patterns Comparison (OLD)
11. Alere Data Import
	* subset to only pt data (removed test encounters)
	* removed irrelevant vars, unusable Patient IDs
	* created $FIN col from Patient IDs = 12 characters and only digits→ alere.all.Rda
12. Alere & Flu Merge & Analysis (source: df.flu.Rda)
13. Alere & Flu Before After Merge & Analysis
14. Alere & Tracking Merge
15. Redo 13 (Flu and LOSCXR) with New Full Set
16. Alere & Flu Ordering Patterns.nb
17. PCR Orders
18.  Full Datasets Merge (BO, ED Tracking Shell, Alere, PCR)
	* source: df.flu.beforeafter.Rda, alere.dedup.Rda, pcr.flu.clean.Rda
	* merged by FIN and created unified FluOverall column; computed durations→ alere.flu.pcr.Rda
19. Final analyses
	* 19.1 - Flu LOS Analysis
	* 19.2 - Flu LOS ILI Subgroup Analysis
	* 19.3 - Flu LOS CXR Analysis
	* 19.4 - Flu Ordering Patterns Analysis
