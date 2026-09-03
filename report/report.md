# **CSE437 Data Science - Project Report** 

## **Cover** 

- **Project title:** Predicting Building Energy Use Intensity in Seattle Using Machine Learning • 

- **Course:** CSE437 Data Science 

- **Section:** 05 

- **Semester:** Summer 2026 

- **Group:** 09 

- **Group members:** 

   - Tashrique Tanvir - ID [23301389] 

- **GitHub repository:** https://github.com/tashriquetanvir-commits/cse437-building-energy-G9.git 

- • **Date:** September 3, 2026 

## **Summary** 

This project predicts **Site Energy Use Intensity (SiteEUI)** , measured in kBtu per square foot, from non-leaking physical and operational characteristics in the City of Seattle’s _Building Energy Benchmarking Data, 2015-Present_ . The downloaded data contained 38,309 building-year records and 46 columns; after retaining compliant records, removing missing targets, and excluding 34 extreme SiteEUI values above 800, 35,114 records remained. Ridge Regression and Random Forest Regressor were compared with an 80/20 building-grouped holdout and five-fold grouped cross-validation. The target was modelled as log1p(SiteEUI) to reduce severe right-skew, and log-scale R² was declared the primary metric. Random Forest achieved the best test log-R² (0.420) and the lowest raw-scale MAE (20.49 kBtu/sf), while Ridge achieved the best raw-scale R² (0.273). The central finding is that building use type is the clearest available signal: supermarkets, laboratories, hospitals, recreation facilities, and data-centre-related uses have the largest positive effects. However, high-intensity specialist buildings remain the main failure cases, showing that physical metadata cannot fully represent occupancy, equipment loads, or operating schedules. 

## **1. Problem and Dataset** 

### **1.1 Problem statement** 

Energy use intensity makes buildings of different sizes comparable by expressing annual site energy per square foot. Predicting SiteEUI from building characteristics can support early screening of inefficient buildings, prioritisation of audits, and more targeted energy-management decisions when complete consumption data are not yet available. The task is supervised regression: estimate a continuous SiteEUI value from attributes known about the building and its uses, while excluding any column that directly contains energy consumption or is mathematically derived from the target. 

### **1.2 Dataset** 

**Source:** City of Seattle Open Data, _Building Energy Benchmarking Data, 2015-Present_ : https://data.seattle.gov /d/teqw-tu6e. **Provider:** Seattle Office of Sustainability and Environment. **Collection method:** annual owner reported benchmarking through ENERGY STAR Portfolio Manager for covered multifamily and non-residential properties. **Downloaded size:** 38,309 rows × 46 columns. **Period in the downloaded file:** 2015-2025. **Terms:** public-domain dataset as stated in the portal metadata. 

### **1.3 Target variable** 

The target is SiteEUI(kBtu/sf), a continuous variable. In the final cleaned sample (n = 35,114), mean = 51.11, median = 36.70, standard deviation = 49.09, minimum = 0.10, and maximum = 787.20 kBtu/sf. The raw distribution is strongly right-skewed (Figure 1). 



<!-- Start of picture text -->
SiteEWl Before Transtonmation<br>12000<br>Lodo<br>OO<br>=<br>wal<br>FT<br>Print)<br>Priit<br>o<br>a 1a 200 wd era] 50d 60d 700 ao<br>SiteElll<br><!-- End of picture text -->



<!-- Start of picture text -->
SIteELA After log]p Transformation<br>S00 7<br>00 +<br>rude+<br>-<br>4 Boe 4<br>F 1500 +<br>1000 +<br>ol a) +<br>a<br>o 1 Z 1 4 | &<br>logLp Simba<br><!-- End of picture text -->



<!-- Start of picture text -->
Median Seetulby Major EPA Property Tipe<br>Supe erie ery one<br>Llaboniony<br>Hoan (Geral Medical&@ Surgical<br>Dhar: Reireatats<br>Motto! Often<br>E “es<br>fhe<br>i Hote<br>i fend Ling Comin<br>& TH aerurys<br>Ae Lue Prager<br>Paneer Pili Dom boy<br>Cater - GrieraoreeniPabie Asembty<br>Bethel Sore<br>fe<br>4 Et] Lil pea] ath<br>Beedian Geil[ib Bnati<br><!-- End of picture text -->



<!-- Start of picture text -->
Truncated S¥D Cimerndionalaty Reduction<br>: Be ee el .<br>i oe<br>; a7<br>4 oe<br>Pos<br>a=<br>a<br>Fi |<br>=<br>ta<br>a3<br>a i 10 i a0 Fe) Qn<br>SVD Componest.<br><!-- End of picture text -->

### **5.4 Metrics** 

Primary metric: R² on log1p(SiteEUI), declared before test evaluation because this is the scale actually optimised and it limits domination by a few extreme buildings. Secondary metrics are raw-scale R², MAE, and RMSE. MAE describes typical absolute error; RMSE gives extra weight to large misses. 

5 

## **6. Hyperparameter Tuning** 

### **6.1 Search space** 

Model Hyperparameter Values searched 

Ridge alpha 0.1, 1, 10, 30, 100 Random Forest n_estimators 100 Random Forest max_depth 10, 20, None Random Forest min_samples_leaf 2, 4 Random Forest max_features sqrt, 0.5 

### **6.2 Method** 

Ridge used an exhaustive five-candidate grid. Random Forest used a compact six-configuration grid selected from the combinations above to keep computation reasonable. Each candidate was assessed with five building-grouped folds and scored by log-scale R². All preprocessing was re-fitted inside each fold. 

### **6.3 Results** 

Model/configuration Mean grouped CV log-R² 

Ridge alpha = 0.1 0.308 Ridge alpha = 1 0.315 Ridge alpha = 10 0.327 Ridge alpha = 30 0.334 **Ridge alpha = 100 0.335** RF depth = 10, leaf = 2, sqrt 0.319 RF depth = 20, leaf = 2, sqrt 0.376 **RF depth = None, leaf = 2, sqrt 0.381** RF depth = 10, leaf = 4, features = 0.5 0.358 RF depth = 20, leaf = 4, features = 0.5 0.376 RF depth = None, leaf = 4, features = 0.5 0.373 

Ridge improved gradually as regularisation increased. Random Forest underfit at depth 10; deeper trees improved validation performance, with unlimited depth, two observations per leaf, and square-root feature sampling perform ing best. 

## **7. Results, Visualization and Error Analysis** 

### **7.1 Test set performance** 

Model Log-R² Raw R² MAE (kBtu/sf) RMSE (kBtu/sf) 

Mean baseline -0.001 -0.052 27.07 59.62 Ridge (alpha = 100) 0.400 **0.273** 21.05 **49.57 Random Forest (tuned) 0.420** 0.253 **20.49** 50.26 

Random Forest is selected on the declared primary metric and MAE. Ridge remains competitive and performs slightly better on raw-scale R² and RMSE, so the advantage is modest rather than decisive. 



<!-- Start of picture text -->
Actual v5 Predicted SibeEUl<br>ang a<br>“ fr<br>a<br>Fi 400 ora<br>5 00 a<br>t af ol ie = i,<br>20 og RB me<br>| oe ©<br>: ai<br>fru] ' com ."<br><.. “stn” ma | si.<br>» @ a .<br>a<br>0 1 00 ao 0 son Boo<br>Actual SEU!<br><!-- End of picture text -->



<!-- Start of picture text -->
Actual vi Pred i netctedil — Loy deale<br>a<br>4 ral<br>Pe?<br>a<br>5 =<br>Sou ‘a.<br>4) 2s * sees te<br>Silt. 2 AF :<br>i ae i.<br>; + mm“figi +4bo 4 * :<br>2 # a<br>a<br>L e<br>Z *<br>L i ET 4 4 a<br>Artal bog] pL<br><!-- End of picture text -->



<!-- Start of picture text -->
on Residual Mot<br>_—<br>_ soe ‘<br>; Pa]= a aawr |& z<br>*| ow a'ri ey 4 . .<br>i i oS, ee . =<br># rj ‘ 7. , 7. . =<br>= La . « Ps 5 aa =“* 7-<br>a = im Pas an Phi<br>Predeind Saab<br><!-- End of picture text -->



<!-- Start of picture text -->
Tap LS Aldge Coeftic bent: by Sbacdine Blagrinide<br>calegemal PRLPnpes bye Sperrue Seen Were Po<br>valanceple ge Died en ;<br>ornate pe eee |<br>a ga a_i Ln Dag pe TL ab ;<br>eee eee ey ee ee ;<br>Ca eg a_i LE gh ek Diy _COR Da ~ : bad a |<br>Se eetee Cee .Bere |<br>SWRPA Sp eC hl<br>CabCae Tere|reee eeLa yerpe_Coed a peer ;hz Ss<br>hatedi anpestPger ty bor epee fide: Boedaeuradia hi<br>GH _ ora Leeper _ Fd ee 7<br>taiepea Lage Papesldhepe Celebs Teds fT<br>Rod_Caceres Pacey hs<br>“Lf  oLe<br>Fale Contthiatat:<br><!-- End of picture text -->



<!-- Start of picture text -->
Hay terst Prediction: Error Sy EPAPropeerts Type<br>Seeleyre<br>Wiha = Reena<br>tethet- bee reel ic Ay<br>Pe<br>simp Mall<br>Laboreicey<br>Bet eerste eke<br>Bebe te<br>ther<br>Pia]Le Pet<br>ob LE: Pit= Ba: a: Ei: on: W=<br>Meer Sickle Eee<br><!-- End of picture text -->

**Example 2 - recreation/public assembly building.** A record above 400 kBtu/sf is predicted near 100, an error of roughly 300. Event frequency, pools/ice systems, and intermittent occupancy can vary sharply between buildings with similar metadata. 

8 

### **7.4 Answers to the three questions** 

**Q1 - Which characteristics matter most?** Property-use categories are the clearest predictors. Median comparisons and Ridge coefficients consistently identify supermarkets, laboratories, hospitals, data centres, restaurants, and recreation uses as high-intensity groups. Building age, floor area composition, location, and reporting year provide additional but weaker signals. 

**Q2 - How accurately can the models predict unseen buildings?** Accuracy is useful but limited. The tuned Random Forest explains about 42.0% of test variation on the log scale and reduces raw MAE from 27.07 to 20.49 kBtu/sf. Large specialist-building errors remain, so predictions are screening estimates rather than substitutes for measured energy data. 

**Q3 - Which approach performs best?** Tuned Random Forest on the full, leakage-safe encoded feature set performs best on the declared log-R² metric and MAE. Ridge is close and gives the best raw R²/RMSE plus clearer coefficient interpretation. Truncated SVD was not kept because the full feature representation preserved interpretability without a meaningful validation penalty. 

## **8. Limitations and Next Steps** 

The data are annual self-reports and may contain reporting corrections, inconsistent use classifications, and changes in building operation. The model does not observe occupancy density, operating hours, weather at the property level, equipment condition, retrofit history, tenant behaviour, or detailed end uses. The 800-kBtu/sf ceiling makes the model more stable but means conclusions do not cover the most extreme records. Associations should not be interpreted causally. 

With more time, the next steps are to add weather variables fitted by reporting year, use repeated-building history without leaking the test period, tune gradient-boosted trees, calculate confidence intervals through grouped resampling, and investigate separate models for major property types. External validation on another city’s benchmarking data would test generalisability. 

## **9. Contributions** 

Member Student ID Contribution 

Tashrique Tanvir [23301389] Data audit, preprocessing, feature engineering, modelling, tuning, 

evaluation, visualisation, error analysis, and report preparation. 

## **References** 

- City of Seattle. _Building Energy Benchmarking Data, 2015-Present_ . https://data.seattle.gov/d/teqw-tu6e. Public domain. 

- City of Seattle Office of Sustainability and Environment. _Energy Benchmarking Program_ . https://www.seattle. gov/environment/climate-change/buildings-and-energy/energy-benchmarking. 

- Libraries: pandas, NumPy, scikit-learn, matplotlib, and seaborn. 

- **AI assistance disclosure:** ChatGPT (OpenAI) was used to help structure the analysis in line with the faculty template, check leakage safeguards, reproduce model-evaluation calculations, and draft this report from the dataset and supplied notebook figures. The student remains responsible for reviewing the code, values, interpretations, and final submission. 

9 

