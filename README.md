import pandas as pd 
import numpy as np 
df = pd.read_csv('/data/2021-New-Coder-Survey-csv/2021_New_Coder_Survey.csv')
/tmp/ipykernel_3425/1397107983.py:1: DtypeWarning: Columns (8,9) have mixed types. Specify dtype option on import or set low_memory=False.
  df = pd.read_csv('/data/2021-New-Coder-Survey-csv/2021_New_Coder_Survey.csv')
df.drop('Timestamp',axis=1,inplace=True)
 
columns = [
    'reason_learning',
    'learning_methods',
    'online_resources',
    'inperson_events',
    'coding_podcasts',
    'youtube_channels',
    'weekly_hours',
    'coding_months',
    'money_spent',
    'employed_dev',
    'first_dev_job',
    'want_dev_career',
    'job_preference',
    'career_interests',
    'job_apply_time',
    'expected_salary',
    'dev_career_reasons',
    'remote_days',
    'willing_relocate',
    'employment_status',
    'current_field',
    'last_year_income',
    'age',
    'gender_identity',
    'group_identity',
    'region',
    'us_state',
    'city_population',
    'diff_citizenship',
    'ethnic_minority',
    'english_second_lang',
    'education_level',
    'field_of_study',
    'current_student',
    'marital_status',
    'num_children',
    'support_dependents',
    'savings_3_months',
    'debt_car_loans',
    'debt_credit_cards',
    'debt_medical_loans',
    'debt_mortgages',
    'debt_payday_loans',
    'debt_student_loans',
    'debt_other',
    'current_job_duration',
    'prev_job_search_months',
    'job_loss_risk',
    'find_similar_job',
    'underemployed',
    'satisfaction_earnings',
    'satisfaction_benefits',
    'satisfaction_security',
    'satisfaction_balance',
    'satisfaction_growth',
    'satisfaction_culture',
    'satisfaction_diversity',
    'satisfaction_workload',
    'commute_minutes',
    'military_service',
    'disability_benefits',
    'home_internet'
]
df.columns = columns
col = [
    'reason_learning',
    'learning_methods',
    'online_resources',
    'inperson_events',
    'coding_podcasts',
    'youtube_channels',
    'first_dev_job',
    'want_dev_career',
    'job_preference',
    'job_apply_time',
    'dev_career_reasons',
    'remote_days',
    'employment_status',
    'current_field',
    'gender_identity',
    'group_identity',
    'us_state',
    'city_population',
    'diff_citizenship',
    'ethnic_minority',
    'english_second_lang',
    'current_student',
    'num_children',
    'support_dependents',
    'savings_3_months',
    'debt_car_loans',
    'debt_credit_cards',
    'debt_medical_loans',
    'debt_mortgages',
    'debt_payday_loans',
    'debt_student_loans',
    'debt_other',
    'current_job_duration',
    'prev_job_search_months',
    'job_loss_risk',
    'find_similar_job',
    'underemployed',
    'satisfaction_earnings',
    'satisfaction_benefits',
    'satisfaction_security',
    'satisfaction_balance',
    'satisfaction_growth',
    'satisfaction_culture',
    'satisfaction_diversity',
    'satisfaction_workload',
    'commute_minutes',
     'military_service'
]
df.drop(col,axis=1,inplace=True)
df.dropna(inplace=True)
df['employed_dev'] = df['employed_dev'].map({'Yes': 1, 'No': 0})
df['disability_benefits'] = df['disability_benefits'].map({'Yes': 1, 'No': 0})
df['home_internet'] = df['home_internet'].map({'Yes': 1, 'No': 0})
df['money_spent'] = df['money_spent'].astype(float)
df['coding_months'] = df['coding_months'].astype(float)
import re

def parse_salary(val):
    if pd.isnull(val):
        return np.nan

    val = str(val).strip()

    if "Under" in val:
        return 1000
    elif "over" in val or "Over" in val:
        return 260000  # or any upper limit
    elif "don't" in val.lower() or "don’t" in val.lower():
        return np.nan
    else:
        # Match range like "$10,000 to $20,999"
        match = re.findall(r'\$?([\d,]+)', val)
        if len(match) == 2:
            low = int(match[0].replace(',', ''))
            high = int(match[1].replace(',', ''))
            return (low + high) // 2
        elif len(match) == 1:
            return int(match[0].replace(',', ''))
        else:
            return np.nan
df['expected_salary'] = df['expected_salary'].apply(parse_salary)
df['last_year_income'] = df['last_year_income'].apply(parse_salary)
 
Questions
1. What is the marital status of the maximum coders ?
df['marital_status'].mode()
0    Single, never married
Name: marital_status, dtype: object
Ans : Single, never married
2. How many coders are university graduates ?
cn = df[df['education_level' ] == 'Bachelor’s degree']['education_level']
cn.shape
(3160,)
Ans : 3160
3. Which part of the world consist of highest coders ?
df['region'].mode()
0    North America
Name: region, dtype: object
Ans : North America
4. Which is the common age of maximum developers ?
df['age'].mode()
0    23.0
Name: age, dtype: float64
Ans : 23
5. How many developers are willing to relocate ?
ree = df[df['willing_relocate'] == 'Yes']['willing_relocate']
ree.shape
(3974,)
Ans : 3974
df.to_csv('2021-new-coder-analysis.csv',index= False)
