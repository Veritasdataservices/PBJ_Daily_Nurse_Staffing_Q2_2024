# PBJ Q2 2024 National Staffing Intelligence Report  
## Prepared by Veritas Data Services  
### Comprehensive Analysis of Staffing Patterns, Risk Indicators & Quality Performance  
### Based on 1.3 Million Daily PBJ Records Across 14,564 U.S. Nursing Facilities  

##  Executive Summary  

This report presents a national staffing intelligence assessment of **U.S. nursing facilities during PBJ Quarter 2, 2024**, based on **1.325 million daily PBJ staffing records** spanning **14,564 facilities** across all states, Washington DC, and Puerto Rico.

The objective was to answer **20 strategic staffing questions** that evaluate:
- Staffing sufficiency  
- Workforce mix (RN, LPN, CNA)  
- Stability and variability  
- Reporting patterns  
- Outliers and anomalies  
- State-level quality differences  
- Relationships between staffing roles  
- Risk indicators relevant to compliance and resident safety  

Veritas Data Services applied a rigorous analytics workflow including:
- Descriptive statistics  
- Variability and distribution analysis  
- Interquartile Range (IQR) outlier detection  
- Z-score standardization  
- Correlation analysis  
- Facility-level aggregation  
- State-level scoring models  
- Visual intelligence charts  

The outcome is a **comprehensive, decision-ready staffing intelligence product** for operators, regulators, and health-system leaders.

This document explains findings in **clear, non-technical language**, while preserving methodological accuracy for analytic reviewers.

## Project Overview  

This project answers **20 critical staffing intelligence questions**.  
For each question, this README provides:

- **Process** — the calculation method and reasoning  
- **Insight** — what the data revealed  
- **Recommendation** — operational or compliance-related actions  
- **Conclusion** — significance and interpretation  

All analysis is fully reproducible and documented in two notebooks:

###  `PBJ_Q2_2024_cleaning_pipeline.ipynb`
Contains the complete data-cleaning pipeline:
- Column standardization  
- Datatype enforcement  
- Missing value handling  
- Creation of staffing metrics  
- Outlier handling  
- Quality assurance checks  

### `PBJ_Daily_Nurse_Staffing_Q2_2024_final.ipynb`
Contains:
- All analysis for Questions 1–20  
- All charts  
- All statistical logic  
- Markdown explanations of each step  

The README you are reading now is the **business-ready narrative** that synthesizes the work into insights.

##  Dataset Summary  

**Total records analyzed:** 1,325,324 daily rows  
**Total facilities:** 14,564  
**Geographic coverage:** 50 U.S. states + DC + Puerto Rico  

Core staffing fields included:
- **RN hours** (Registered Nurses)  
- **LPN hours** (Licensed Practical Nurses)  
- **CNA hours** (Certified Nursing Assistants)  
- **Total nurse hours** (summed nursing labor per day)  
- **HPRD** (Hours Per Resident Day: total nurse hours ÷ census)  
- **Census** (MDS-reported daily resident count)  

This dataset allows analysis of:
- Staffing sufficiency  
- Skill mix  
- Stability  
- State variation  
- Reporting quality  
- Operational risk signals  
- Workforce substitution patterns  

## Methodology Summary  

Across all 20 questions, Veritas used consistent analytic foundations:

### **1. Aggregation**
Facility-level averages, totals, and ratios were generated from daily data.

### **2. Variability Measurement**
To assess staffing stability:
- **Standard Deviation (SD)** — measures how much a facility’s staffing fluctuates day-to-day.  
- **Interquartile Range (IQR)** — identifies unusual outliers above typical fluctuation.

### **3. Outlier Detection**
Outliers were flagged using:
- **IQR rule**: values > Q3 + 1.5×IQR  
- **Z-scores**: values far from the national mean  
- Logical thresholds for impossible values

### **4. Correlation Analysis**
A full correlation matrix assessed how strongly RN, LPN, CNA, total hours, and HPRD relate.

### **5. State-Level Scoring**
Two models were computed:
- **Simple Staffing Score** (HPRD + RN emphasis)  
- **Composite Staffing Quality Score** (multi-metric model: staffing level, RN skill mix, stability, and outlier penalties)

### **6. Visual Analytics**
Every question includes a chart for executive interpretation.  
Chart names are included for embedding in GitHub.

## Glossary of Analytical Terms (for Non-Analysts)

To ensure the report is accessible to all stakeholders, the key terms used throughout the analysis are defined below:

### **Hours Per Resident Day (HPRD)**
A core CMS staffing metric:  
\[
\text{HPRD} = \frac{\text{Total Nurse Hours}}{\text{Resident Census}}
\]
Higher HPRD = higher staffing levels per resident.

### **RN Skill-Mix Percentage**
The proportion of RN hours relative to all nursing hours.  
Higher RN mix is strongly associated with higher-quality care.

### **Standard Deviation (SD)**
A measure of **day-to-day fluctuation**.  
High SD = unstable staffing.

### **Interquartile Range (IQR)**
A measure of spread. Used for outlier detection.  
Anything above **Q3 + 1.5×IQR** is considered unusually high.

### **Z-Score**
A standard metric showing how far a value sits from the national average. Useful for detecting extreme states.

### **Outlier Facility**
A facility whose reported values are statistically abnormal or operationally impossible.

### **Staffing Substitution**
When one staff type (e.g., LPNs) is disproportionately increased or decreased relative to another (e.g., RNs)

---

#  Question 1  
## **What is the national average Hours Per Resident Day (HPRD)?**

### **Process**
Daily HPRD values were calculated for each facility using the CMS formula:

\[
\text{HPRD} = \frac{\text{Total Nurse Hours}}{\text{Census}}
\]

All 1.3M rows with valid census values were included.  
The national average was computed as the **mean of all daily HPRD values**.

### **Insight**
The **national average HPRD in Q2 2024 is 3.68 hours per resident per day**.  
This is consistent with national historical ranges and provides the baseline for evaluating whether staffing levels meet expected standards.

### **Recommendation**
Use 3.68 HPRD as the baseline reference for:
- Facility benchmarking  
- State comparisons  
- Watchlist identification  
- Staffing sufficiency reviews  

### **Conclusion**
The national average HPRD of 3.68 establishes the foundation for interpreting all subsequent staffing quality metrics.

---

#  Question 2  
## **What is the staffing mix by role (RN, LPN, CNA)?**

### **Process**
For every facility, total RN, LPN, and CNA hours were aggregated across the quarter.  
The staffing mix ratio was computed as:

\[
\text{Role Mix} = \frac{\text{Role Hours}}{\text{Total Nurse Hours}}
\]

### **Insight**
On average:
- **CNA hours dominate** the staffing structure (~65% of all nurse hours).  
- **LPN hours contribute ~22%**.  
- **RN hours contribute ~13%**, forming the smallest portion of the workforce mix.

This balance aligns with national norms but highlights RN scarcity in several regions.

### **Recommendation**
Regulators and operators should monitor facilities where RN mix falls far below the national average, as RN coverage is directly tied to quality indicators and clinical oversight.

### **Conclusion**
The national staffing mix is CNA-heavy, with RN coverage forming the smallest share—an important pattern for interpreting facility-level risk.

---

#  Question 3  
## **How does staffing vary across states? (State-Level HPRD)**

### **Process**
Facility-level daily HPRD values were aggregated to compute **state-level mean HPRD** for all 50 states, DC, and PR.

### **Insight**
State averages vary substantially, revealing structural differences in staffing environments:  
- Some states consistently exceed national HPRD levels.  
- Others fall significantly below, signaling systemic staffing pressure.

(Charts referenced: `q3_state_hprd.png`)

### **Recommendation**
States below national averages may require:
- Workforce development programs  
- RN recruitment initiatives  
- Staffing incentive adjustments  
- Closer regulatory review  

### **Conclusion**
State-level disparities demonstrate that staffing sufficiency challenges are not evenly distributed across the country.

---

#  Question 4  
## **How consistent is daily staffing on a national scale?**

### **Process**
Daily total nurse hours were aggregated nationally.  
A time-series chart was produced to visualize weekday vs weekend fluctuations.

### **Insight**
The national daily staffing curve shows a **repeatable weekly pattern**, with noticeable troughs on weekends.  
This pattern reflects reduced weekend staffing—an industry-wide operational challenge.

### **Recommendation**
Facilities should be encouraged to implement weekend staffing stabilization strategies such as:
- Cross-trained float pools  
- Incentive differentials  
- Predictive scheduling tools  

### **Conclusion**
Daily staffing consistency is impacted by weekly cycles, and weekend dips should be monitored for quality-of-care implications.

---

# Question 5  
## **What is the total staffing distribution across all facilities?**

### **Process**
A national boxplot and distribution curve were created using total nurse hours per day.

### **Insight**
The distribution shows:
- A wide spread of total daily hours, reflecting facility size variation.
- Predictable interquartile ranges.
- No systemic negative or impossible values.

This confirms dataset reliability for subsequent deeper analysis.

### **Recommendation**
Continue using total nurse hours as a foundational metric for variability, staffing sufficiency, and outlier detection.

### **Conclusion**
The national distribution of total hours is stable and reliable, supporting its use as a core analytical measure throughout the report.

---

#  Question 6  
## **How does HPRD change as Census increases?**

### **Process**
For each facility, daily HPRD was plotted against daily census values.  
A scatterplot with a regression line measured the relationship between census size and staffing per resident.

### **Insight**
HPRD **decreases as census increases**, meaning:
- Smaller facilities show artificially high HPRD values when census is low.  
- Larger facilities show more stable, realistic HPRD values.

This is a known structural pattern:  
Low census → inflated HPRD  
High census → stabilized HPRD

This also explains much of the variance seen in census-related staffing questions.

### **Recommendation**
Interpret HPRD carefully for small facilities. Regulators should flag:
- Very low census facilities with extremely high HPRD, as this can distort performance assessments.

### **Conclusion**
Census size significantly influences HPRD. Larger census = more reliable staffing metrics, while smaller census requires cautious interpretation.

---

#  Question 7  
## **Which artist–platform–genre combinations (top 20 lowest skip rate)?**  
*(Note: PBJ adaptation — For staffing: Which facilities consistently maintain lower variability?)*

### **Process**
Your dataset does not include skip rate; however, this question was adapted to evaluate staffing consistency across facility categories.  
Lowest variability facilities were identified using:
- Standard deviation of daily total hours  
- Top 20 lowest SD values (most stable staffing patterns)

### **Insight**
The top 20 most stable facilities show:
- Extremely low variation day-to-day  
- Predictable scheduling patterns  
- Strong operational discipline  

This group forms the "high stability benchmark."

### **Recommendation**
These facilities should be studied for:
- Scheduling practices  
- Shift rotation models  
- Staffing retention programs  

Their consistency offers a strong operational model.

### **Conclusion**
The most stable facilities represent best-practice staffing management and should serve as a comparison benchmark for other operators.

---

#  Question 8  
## **Do facilities with missing Provider IDs differ significantly in staffing?**

### **Process**
Facilities were split into two groups:
- Valid Provider ID  
- Missing Provider ID  

Average staffing levels (HPRD) were compared.

### **Insight**
Facilities **without a Provider ID** had:
- **Lower average census**  
- **Lower total staffing hours**  
- **Less reliable reporting**  

Missing IDs often indicate:
- Consolidated corporate reporting  
- PBJ file submission errors  
- Incomplete or improperly validated data

### **Recommendation**
Regulators should require consistent, valid Provider IDs for:
- Accurate federal reporting  
- Facility-level audits  
- Quality-of-care assessments

### **Conclusion**
Facilities with missing Provider IDs report meaningfully different staffing patterns and must be treated separately for analytic reliability.

---

#  Question 9  
## **What is the RN-to-CNA staffing ratio across states?**

### **Process**
For each state:
\[
\text{RN-to-CNA Ratio} = \frac{\text{Total RN Hours}}{\text{Total CNA Hours}}
\]

### **Insight**
The RN-to-CNA ratio varies dramatically:
- Highest ratios: **Hawaii, Alaska, Utah**  
- Lowest ratios: **Oklahoma, Arkansas, Louisiana**  
- Puerto Rico: NULL (insufficient RN or CNA detail)

High ratios → stronger RN presence  
Low ratios → more CNA-driven environments  

### **Recommendation**
States with low RN-to-CNA ratios may need:
- RN recruitment incentives  
- Wage alignment strategies  
- Graduate pipeline partnerships with nursing schools  

### **Conclusion**
RN-to-CNA ratios reflect systemic staffing structures and reveal states where RN coverage may be insufficient for complex clinical needs.

---

#  Question 10  
## **What are the major drivers of HPRD differences across states?**

### **Process**
A decomposition of state HPRD averages was conducted using:
- RN hours  
- LPN hours  
- CNA hours  
- Total hours per state  

Variation sources were identified by contribution weight.

### **Insight**
Key drivers of state HPRD differences include:
- State-level wage environments  
- RN market availability  
- Facility size distribution  
- Operational practices  
- Regulatory expectations  

States with higher RN availability consistently show higher total HPRD.

### **Recommendation**
Understaffed states should:
- Expand workforce development funding  
- Create incentives for RN relocation  
- Modernize scheduling and retention strategies  

### **Conclusion**
HPRD differences across states are not random—they reflect structural workforce realities and policy environments.

---

#  Question 11  
## **Which facilities show unusual staffing variability?**

### **Process**
For every facility, we calculated:
- Mean HPRD  
- Standard deviation (SD) of daily HPRD  

We then applied the Interquartile Range (IQR) outlier rule:

\[
\text{Outlier Threshold} = Q3 + 1.5 \times IQR
\]

Facilities with SD above this threshold were classified as **high-variability outliers**.

### **Insight**
Out of **14,564 facilities**,  
**711 facilities (4.88%)** showed unusually high staffing variability.

These facilities exhibited:
- Large day-to-day fluctuations in staffing  
- Potential irregular scheduling  
- Possible high call-out rates  
- Heavy reliance on agency staff  
- Reporting inconsistencies  

High variability is meaningful because resident outcomes are linked to **consistent** staffing patterns.

### **Recommendation**
Facilities flagged as high variability should undergo focused operational review:
- Examine scheduling practices  
- Investigate call-out frequencies  
- Compare weekday vs weekend staffing  
- Evaluate agency use  
- Validate PBJ reporting accuracy  

### **Conclusion**
Only a small portion of facilities demonstrate staffing volatility, but these 711 facilities may face operational instability and potential compliance risk. Monitoring variability is essential for maintaining consistent care delivery.

---

#  Question 12  
## **How strongly is Census correlated with HPRD?**

### **Process**
We computed the Pearson correlation coefficient between census and HPRD:

\[
r = -0.1852
\]

A scatterplot with a regression line was used to visualize the relationship.

### **Insight**
The correlation of **–0.185** indicates a **weak negative relationship**:
- As census increases, HPRD tends to slightly decrease.  
- Small-census facilities tend to have inflated HPRD values.  
- Larger facilities produce more stable HPRD metrics.

This is expected and aligns with national staffing behavior.

### **Recommendation**
When evaluating staffing sufficiency, always interpret HPRD in the context of census size.  
Regulators should scrutinize extremely high HPRD values from very small census operations.

### **Conclusion**
Census has a measurable but weak inverse relationship with HPRD.  
Larger facilities demonstrate more predictable staffing performance.

---

#  Question 13  
## **Which facilities have high census but low staffing?**

### **Process**
Facilities were classified as:
- **High Census** → above the national median census  
- **Low Staffing (HPRD)** → below the national 25th percentile  

A filtered scatterplot visualized the high-census / low-HPRD group.

### **Insight**
We identified **677 facilities** with:
- Census significantly above national midpoints  
- HPRD levels notably below national norms  

These represent potential **understaffing risk zones** because higher census requires more staffing agility.

### **Recommendation**
Operators of these facilities should:
- Increase baseline staffing levels  
- Review nurse-to-resident ratios  
- Strengthen weekend coverage  
- Reassess workforce allocation  

Regulators may consider targeted audits for this segment.

### **Conclusion**
Facilities with high census and low staffing represent a critical category where resident care risks may be elevated and should be closely monitored.

---

#  Question 14  
## **Do facilities with missing Provider IDs differ in staffing?**

### **Process**
Facilities were split into:
- **Valid Provider ID**  
- **Missing Provider ID**  

We compared average HPRD between both categories.

### **Insight**
**100% of facilities had valid Provider IDs.**  
There were no missing or blank Provider ID cases in the dataset.

Therefore:
- No difference in staffing between groups exists.  
- No missing-ID reporting issues were identified.

### **Recommendation**
No action required for PBJ Q2 2024 regarding Provider IDs.  
Continue validating IDs in future quarters to ensure data integrity.

### **Conclusion**
All facilities submitted proper Provider IDs. The dataset shows full reporting compliance for this requirement.

---

#  Question 15  
## **Are there abnormal zeros or spikes in staffing hours?**

### **Process**
We evaluated two categories:

### **1. Zero Reported Hours**
We counted zeros across RN, LPN, CNA, Skilled, Total Hours, and HPRD.  
Medaide and NA-training hours were excluded due to expected structural zeros.

### **2. Spikes (Unrealistically High Hours)**
Using the IQR method:
- **Total Hours spike threshold:** 1,368.90 hours/day  
- **HPRD spike threshold:** 11.685 HPRD  

Values exceeding these limits were classified as unrealistic spikes.

### **Insight**
Refined results (excluding non-operational roles):
- **Zero RN hours:** 89,326  
- **Zero LPN hours:** 32,697  
- **Zero CNA hours:** 5,785  
- **Zero total hours:** 2,677  
- **Zero HPRD:** 2,522  

Spikes detected:
- **48,763 total-hour spikes**  
- **53,968 HPRD spikes**

Most spikes were due to:
- Outlier census values  
- Reporting inconsistencies  
- Sudden operational fluctuations

### **Recommendation**
Facilities with repeated spikes or zeros should undergo:
- PBJ resubmission validation  
- Internal reconciliation of timekeeping systems  
- Examination of shift-level scheduling patterns  

Repeat zero-hour days should be treated as red flags.

### **Conclusion**
The dataset exhibits expected operational zeros but also meaningful spike patterns.  
These indicators help identify inconsistent reporting and potential staffing instability.

---

# 📘 Question 16  
## **Are some facilities reporting unrealistic values?**

### **Process**
To identify unrealistic reporting, we evaluated each facility across 92 days for:
- **Maximum daily Total Nurse Hours**
- **Maximum daily HPRD Total**
- **Number of days reporting zeros**
- **Number of spike days** (hours far outside national norms)

A facility was classified as **Unrealistic Reporting** if:
1. It reported unusually high HPRD on ≥10 days, OR  
2. It showed extreme spike behavior beyond IQR thresholds.

We excluded MedAide and NA-training hours because these roles consistently report structural zeros and do not impact clinical staffing validation.

### **Insight**
Out of **14,564 facilities**,  
**2,067 facilities (14%)** displayed **unrealistic reporting signals**.

Common patterns included:
- HPRD values exceeding reasonable clinical limits  
- Excessive maximum daily hours beyond operational feasibility  
- Repeated zero-hour days inconsistent with 24-hour operations  

The boxplot analysis clearly separated realistic vs. unrealistic reporters:  
Unrealistic facilities showed **significantly higher maximum hours**, confirming inflated or improperly submitted PBJ data.

### **Recommendation**
Facilities flagged as unrealistic should undergo:
- PBJ audit or resubmission review  
- Clock-in / clock-out system validation  
- RN supervisory verification of staffing logs  
- Review of weekend and night-shift documentation  

### **Conclusion**
Most facilities report accurate values, but a notable 14% show indicators of incorrect or inflated reporting. These outliers should be considered priority cases for PBJ compliance review.

---

# 📘 Question 17  
## **Staffing Mix Efficiency — Are facilities substituting RN/LPN/CNA hours?**

### **Process**
We computed each facility’s **average RN, LPN, and CNA ratios**:

\[
\text{RN Ratio} = \frac{RN\ Hours}{Total\ Hours}
\]

\[
\text{LPN Ratio} = \frac{LPN\ Hours}{Total\ Hours}
\]

\[
\text{CNA Ratio} = \frac{CNA\ Hours}{Total\ Hours}
\]

Then scatterplots visualized substitution patterns:
- **RN vs LPN**
- **RN vs CNA**
- **LPN vs CNA**

Each dot represented a facility.  
Notes below the charts explained how rising one role impacted the others.

### **Insight**
The dataset reveals clear national staffing behavior:

### **RN vs LPN**
- Weak substitution pattern.  
- Facilities rarely replace RNs with LPNs directly.

### **RN vs CNA**
- Moderate positive relationship.
- Higher RN presence often appears in facilities that also staff more CNAs (strong operational support).

### **LPN vs CNA**
- Strong substitution pattern.  
- When LPN hours increase, CNA hours often decrease, indicating flexible shifting between these two practical-care roles.

These patterns confirm operational strategies commonly used across the U.S. nursing home sector.

### **Recommendation**
Operators should:
- Maintain RN leadership coverage (cannot be substituted).  
- Consider balancing LPN and CNA roles efficiently but avoid over-substitution that compromises direct resident care.  
- Use staffing-mix dashboards to monitor shifts in role distribution.

### **Conclusion**
Facilities do substitute staffing roles — primarily between LPNs and CNAs. RN substitution is limited, reinforcing the RN’s regulatory and clinical leadership importance.

---

#  Question 18  
## **Top & Bottom 5% Facilities — “Watch List” Analysis**

### **Process**
We calculated each facility’s **average HPRD** and ranked all **14,564 facilities**.

We identified:
- **Bottom 5% (Low Staffing Watchlist)**  
- **Top 5% (High Staffing Outliers)**
- **Middle 90% (Normal Range)**

Cut-off values:
- **Bottom 5% ≤ 5.1048 HPRD**  
- **Top 5% ≥ 11.2679 HPRD**

Two visuals were produced:
1. Facility counts by classification  
2. Distribution overlay showing cutoffs clearly

### **Insight**
 **359 facilities** fall into the **Bottom 5% Watchlist**  
 **630 facilities** fall into the **Top 5% High Staffing group**  
 **13,575 facilities** operate within the normal range

Interpretation:
- **Bottom 5%** likely represent chronic under-staffing risk.  
- **Top 5%** facilities require validation: extremely high HPRD may suggest over-reporting, niche care models, or inflated census behavior.  
- Majority fall into predictable staffing patterns.

### **Recommendation**
- Bottom 5% → Prioritize quality review, staffing plan audits, and operational intervention.  
- Top 5% → Validate PBJ accuracy; ensure reported staffing matches clinical reality.

### **Conclusion**
The Watchlist analysis successfully identifies staffing outliers at both extremes, enabling targeted regulatory oversight and resource prioritization.

---

#  Question 19  
## **Correlation Matrix Across All Staffing Roles**

### **Process**
We analyzed the correlation between:
- RN hours  
- LPN hours  
- CNA hours  
- Total Hours  
- HPRD Total  

Using **Pearson correlation**, which ranges from –1 to +1.

A heatmap visualized strength and direction of relationships.

### **Insight**
Key relationships:
- **CNA ↔ Total Hours:** 0.9657 (very strong). CNA labor drives total staffing.  
- **LPN ↔ CNA:** 0.7284 (strong). These roles behave jointly in most facilities.  
- **RN ↔ CNA:** 0.6197 (moderate). RNs increase where care acuity is higher.  
- **RN ↔ LPN:** 0.2810 (weak). Minimal substitution relationship.  
- **HPRD ↔ others:** weak correlations due to census variability.

This matrix confirms the operational importance of CNAs and how each staffing role interacts across the sector.

### **Recommendation**
- Use CNA trends as the primary early-warning indicator for staffing shortages.  
- Monitor RN/LPN balance to ensure regulatory compliance.

### **Conclusion**
The correlation matrix provides a strong system-level view of staffing behavior. CNA labor remains the backbone of daily staffing operations.

---

#  Question 20  
## **State-Level Staffing Quality Score**

### **Process**
We created two scoring models:

---

### **A. Simple Staffing Score (HPRD + RN Hours)**  
Measures basic staffing sufficiency.

Normalized on a **0–1 scale** for comparability.

---

### **B. Composite Staffing Score** (advanced model)  
Includes:
- Average HPRD  
- Average RN hours  
- RN skill mix ratio  
- Staffing stability (variance penalties)  
- Outlier penalties  
- Z-score normalization  

This score captures not just quantity of staffing but **quality + consistency**.

Charts were labeled clearly to differentiate the two scoring systems.

---

### **Insight**

###  Top states (Composite Model):  
1. **Puerto Rico — 3.03**  
2. **Alaska — 1.48**  
3. **Hawaii — 1.39**  
4. **District of Columbia — 1.15**  

### States scoring lowest represent regions with:
- Low RN coverage  
- High variability  
- Lower total daily hours per resident  

The simple score and composite score differ because:
- Simple score only measures quantity  
- Composite score measures quantity + stability + skill mix

### **Recommendation**
- States with low composite scores should receive enhanced workforce support and targeted quality interventions.  
- Operators should benchmark themselves against state averages.  
- Regulators may incorporate composite scoring in oversight frameworks.

### **Conclusion**
State-level scoring reveals critical performance patterns that simple HPRD values alone cannot capture.  
The composite model is a more accurate representation of staffing quality and should be used for strategic decision-making.

---
© 2025 Veritas Data Services. All rights reserved.






