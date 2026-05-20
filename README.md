Decoding the 2026 Tamil Nadu Assembly Election

An objective, non-partisan data storytelling project analyzing the 2026 Tamil Nadu Legislative Assembly election results. Designed as a strategic editorial pitch for AtliQ Media's prime-time election broadcast.

📺 Project Overview

This project transforms raw candidate-level election results across 234 assembly constituencies in Tamil Nadu into a structured, high-impact news narrative. Instead of traditional political commentary, this analysis takes a strictly neutral, factual approach, focusing on structural voter shifts, regional polarization, and the emerging multi-party landscape in 2026.

🎯 Key Business Requirements (AtliQ Media)

Neutrality: Strictly fact-based analysis using public ECI data. No political predictions or causal claims.

Clarity: Simple, visual-first storytelling built to guide prime-time show production.

The Moat: High-value editorial recommendations backed by statistical hygiene.

📊 Core Data Storylines & Insights

Our single-page master dashboard synthesizes the election results into three highly connected storylines:

1. The Mega Flip Mechanics

The Disruption: A massive wave of shift swept the state, causing 163 out of 234 assembly seats (69.66%) to flip from incumbent parties.

The Major Beneficiary: Thalapathy Vijay’s newly formed party, TVK, captured 108 of these 163 flipped seats, solidifying its position as the state's primary disruptor.

Legacy Impact: Traditional major formations faced severe squeeze in flipped zones, with AIADMK retaining only 25 seats and DMK managing 19.

2. The Geography of Disruption

TVK Strongholds: The wave was highly polarized, with TVK achieving dominant proportional seat shares in the Chennai Metro, South, and the crucial Kongu region.

The Delta Exception: The Delta region completely bucked the state-wide wave, remaining highly loyal to the DMK, which maintained its highest regional proportional seat share.

3. Popular Vote vs. Seat Conversion

The Popular Mandate: TVK's 108-seat victory is backed by a massive popular base, securing a consolidated 34.92% statewide vote share in its debut major run.

Three-Way Split: DMK retained 24.19% of the popular vote, while AIADMK followed closely at 21.21%, indicating that legacy parties still hold unbreakable core voter pockets despite seat losses.

🛠️ Data Model & Tech Stack

Tool: Power BI Desktop

Architecture: Star Schema utilizing a master dimension table linked to factual results.

Relationship Mapping: 1-to-Many join established between constituency_master and candidate-level result tables (tn_2021_results, tn_2026_results) via the primary key ac_number.

🔢 Core DAX Measures & Calculations

1. Candidate Seat Winners (Calculated Tables)

Used to extract the exact winning candidate and party for each of the 234 constituencies:

Winners_2021 = 
SUMMARIZE(
    FILTER(
        tn_2021_results,
        tn_2021_results[votes] = CALCULATE(
            MAX(tn_2021_results[votes]),
            ALLEXCEPT(tn_2021_results, tn_2021_results[ac_number])
        )
    ),
    tn_2021_results[ac_number],
    tn_2021_results[constituency],
    tn_2021_results[party],
    tn_2021_results[votes]
)


Winners_2026 = 
SUMMARIZE(
    FILTER(
        tn_2026_results,
        tn_2026_results[votes] = CALCULATE(
            MAX(tn_2026_results[votes]),
            ALLEXCEPT(tn_2026_results, tn_2026_results[ac_number])
        )
    ),
    tn_2026_results[ac_number],
    tn_2026_results[constituency],
    tn_2026_results[party],
    tn_2026_results[votes]
)


2. Flip Classification (Calculated Column)

Identifies whether a constituency flipped its political allegiance between 2021 and 2026:

Is_Flip = 
VAR Party_2021 = RELATED(Winners_2021[party])
VAR Party_2026 = Winners_2026[party]
RETURN
IF(Party_2021 = Party_2026, "No Flip", "Flip")


3. TVK Statewide Vote Share (Measure)

TVK_Vote_Share_2026 = 
DIVIDE(
    CALCULATE(SUM(tn_2026_results[votes]), tn_2026_results[party] = "TVK"),
    SUM(tn_2026_results[votes]),
    0
)


⚠️ Data Limitations & Hygiene

Scope: This analysis is based strictly on public candidate-level ECI data.

Demographics: The dataset lacks demographic metadata (e.g., age group, gender split, income brackets). All findings represent geographic correlations; no causal claims regarding voter intent or behavior are made.

📂 Repository Contents

tn_2021_results.csv & tn_2026_results.csv: Raw ECI election result datasets.

constituency_master.csv: Unified mapping table for 234 Assembly Constituencies with region and district tags.

TN_Election_Report_dashboard.pbix : Final Power BI Master Dashboard.

Atliq_Media_TN_Election_Pitch_2026 : The 8-slide executive presentation pitch.
