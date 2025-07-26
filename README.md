# 🌱 Lifestyle Sustainability Data Analysis Dashboard

This project presents an interactive data visualization dashboard analyzing lifestyle sustainability factors across various dimensions like environment, household, and personal choices. The analysis is based on survey data from 499 participants and aims to understand behaviors impacting sustainability.

<br/>

## 📊 Dashboard Overview

The dashboard is divided into three main sections, each designed to explore specific aspects of sustainability and provide actionable insights.

### 1. **Overview**
- Total number of participants
- Environmental awareness score by gender
- Age distribution of participants
- Physical activity levels
- Rating distribution on sustainability understanding
- Environmental awareness levels 

**📖 Storyline:**  
This section analyzes the basic demographics and environmental awareness of participants. The goal is to understand **who the participants are**, how aware they are of sustainability issues, and whether factors like gender, age, or physical activity influence this awareness. It helps to identify **key target groups** for sustainability outreach and **education programs**.



### 2. **Household**
- Water usage per person by home type
- Community involvement in sustainability
- Home location (Urban/Suburban/Rural)
- Electricity efficiency index by energy source
- Preferred modes of transportation

**📖 Storyline:**  
Sustainability starts at home. This section explores **household behaviors** that influence environmental impact—such as water and energy consumption, location, and travel choices. It sheds light on how **home type and infrastructure affect sustainable living**, and helps identify areas where lifestyle adjustments can reduce carbon footprint.



### 3. **Lifestyle**
- Local food impact score by food frequency
- Waste disposal methods
- Food & clothing consumption habits
- Sustainability score based on diet type
- Use of sustainable brands
- Overall lifestyle sustainability score

**📖 Storyline:**  
Lifestyle choices play a major role in sustainability. This section dives into **individual habits** like food sourcing, brand preferences, diet, and waste disposal. It highlights how these behaviors impact the environment and calculates a **sustainability score** to measure the eco-friendliness of one’s lifestyle. It’s aimed at encouraging **more conscious consumer behavior**.



<br/>

## 📌 Key Insights

- Majority of participants engage in **moderate physical activity**.
- **Females and non-binary individuals** show higher environmental awareness scores.
- Most people live in **suburban or urban** areas and use **public transport or cars**.
- Only **47% of participants use sustainable brands**.
- **Recycling** is the most common disposal method, followed by landfill usage.

<br/>

## 🧮 DAX Measures Used


1. ElectricityEfficiencyIndex = 
DIVIDE(
    SUM(lifestyle_sustainability_data[EnvironmentalAwareness]),
    SUM(lifestyle_sustainability_data[MonthlyElectricityConsumption])
)

2. EnvironmentalAwarenessScore = 
AVERAGEX(
    FILTER(
        lifestyle_sustainability_data,
        NOT(ISBLANK(lifestyle_sustainability_data[EnvironmentalAwareness]))
    ),
    lifestyle_sustainability_data[EnvironmentalAwareness]
)

3. LocalFoodImpactScore = 
AVERAGEX(
    lifestyle_sustainability_data,
    SWITCH(
        lifestyle_sustainability_data[LocalFoodFrequency],
        "Always", 5,
        "Often", 4,
        "Sometimes", 3,
        "Rarely", 2,
        "Never", 1,
        0
    ) * 
    DIVIDE(lifestyle_sustainability_data[EnvironmentalAwareness], 5)
)

4. SustainabilityScore = 
VAR AwarenessScore = DIVIDE(AVERAGE(lifestyle_sustainability_data[EnvironmentalAwareness]), 5) * 25
VAR WaterScore = 
    VAR AvgWater = AVERAGE(lifestyle_sustainability_data[MonthlyWaterConsumption])
    RETURN IF(AvgWater < 1000, 25, IF(AvgWater < 2000, 15, 5))
VAR ElectricityScore = 
    VAR AvgElec = AVERAGE(lifestyle_sustainability_data[MonthlyElectricityConsumption])
    RETURN IF(AvgElec < 300, 25, IF(AvgElec < 500, 15, 5))
VAR PlasticPenalty = 
    IF(MAX(lifestyle_sustainability_data[UsingPlasticProducts]) = "Yes", -10, 0)
VAR FinalScore = AwarenessScore + WaterScore + ElectricityScore + PlasticPenalty
RETURN ROUND(FinalScore, 1)

5. SustainabilityScore1 = 
AVERAGEX(
    lifestyle_sustainability_data,
    DIVIDE(
        lifestyle_sustainability_data[EnvironmentalAwareness] +
        lifestyle_sustainability_data[Rating] +
        SWITCH(TRUE(),
            lifestyle_sustainability_data[DisposalMethods] = "Compost", 5,
            lifestyle_sustainability_data[DisposalMethods] = "Recycle", 4,
            lifestyle_sustainability_data[DisposalMethods] = "Trash", 2,
            1
        ),
        3
    )
)

6. Total = COUNTROWS(lifestyle_sustainability_data)

7. WaterPerPerson = 
DIVIDE(
    SUM(lifestyle_sustainability_data[MonthlyWaterConsumption]),
    SUM(lifestyle_sustainability_data[HomeSize])
)
<br/>

## 🖼️ Dashboard Snapshots

<br/>

<div align="center">
  <img width="879" height="487" alt="image" src="https://github.com/user-attachments/assets/fd15b418-9703-487c-8014-a670fc7a6928" />
</div>
<br/>

<div align="center">
  <img width="879" height="485" alt="image" src="https://github.com/user-attachments/assets/25bcc9e6-c7cf-4142-a20b-7591f130ef9d" />
</div>
<br/>

<div align="center">
  <img width="879" height="488" alt="image" src="https://github.com/user-attachments/assets/cc9f71c6-2c90-4f1c-a27f-734f4066dd83" />
</div>


