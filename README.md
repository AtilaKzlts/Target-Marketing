# Target Marketing for Insurance Company 

### Table of Contents

- Project Introduction
    - Executive summary
    - About the Data Set
    - Objective
- Analysis Outputs
- Detailed Summary and Recommendations


## Project Introduction

The aim of this project is to analyse the insurance company's customer data to design more accurate targeting and personalised marketing campaigns. Customer segmentation will be performed to better understand the existing customer base and segment-specific strategies will be developed. Particular emphasis will be placed on age groups, estimated salary and tenure.

## Executive summary

The company's customer base caters to the upper-middle income group with an average age of 39 years and a policy duration of 5 years. The average salary is 100K and Health, Car, Home and Life policies have almost equal number of customers (~2500 customers). Men have more policies than women in all policy types. The high income group is the largest segment with 7,039 customers, with the 30s (4,346 customers) constituting the largest customer group, while the 20s are the second largest group with 1,592 customers. Health insurance is the policy type with the highest complaint rate with 547 complaints.

**For more detailed demographic information :**
**[Demographic Visualisation Report](https://github.com/AtilaKzlts/Insurance_customer_targeting/blob/main/assets/demographic_report.pdf)**


## About the Data Set

The table below contains the column names and descriptions in the dataset on insurance policies.

| **Column Name** | **Description** |
|----------------------------|--------------------------------------|
| `policy_id` | Policy ID |
| `policy_holder` | Name of policyholder |
| `risk_assessment_score` | Risk assessment score |
| `insured_region` | Insured region |
| `coverage_amount` | Amount of insurance coverage (USD) |
| `policies_owned` | Number of policies owned |
| `autopay_enabled` | Is automatic payment enabled? |
| `is_active_policy` | Is the policy active? |
| `annual_premium` | Annual premium amount |
| `policy_type` | Policy type (Health, Vehicle, etc.) |
| `reward_points` | Loyalty programme points |
| `policy_canceled` | Policy cancellation status |
| `claim_complaint` | Is there a claim complaint? |
| `customer_feedback_score` | Customer feedback score |


## Objective

The purpose of this study is to develop target marketing strategies by analyzing the demographic, behavioral and financial characteristics of customers in the insurance sector.
Within the scope of the analysis:

Age, income and policy ownership characteristics of customers were analyzed according to general customer profile and policy types.
Customers were segmented by income groups and age cohorts, and the insurance needs and marketing opportunities of each group were analyzed.
Customer satisfaction and complaints were analyzed by policy type, problematic areas were identified and areas for improvement were identified.
Differences between gender and policy types were analyzed, and personalized marketing recommendations were presented for different customer groups.


## Analysis Outputs

``` sql 
/* General customer profiles */
SELECT  
    COUNT(policy_id) AS TotalPolicies,
    ROUND(AVG(age)::NUMERIC, 2) AS AverageAge,  
    ROUND(AVG(tenure)::NUMERIC, 2) AS AverageTenure,  
    ROUND(AVG(estimatedsalary)::NUMERIC, 1) AS AverageSalary,
    SUM(is_active_policy) AS ActivePolicies
FROM incurance  
```
Outputs:

																  
| Total Policies | Average Age | Average Tenure | Average Salary | ActivePolicies |
| :------------: | :---------: | :------------: | :------------: | :------------: |
| 10,000         | 38.92       | 5.01           | 100,090.2      | 5,151      |



```sql 
/* Customer information by policy type */ 
SELECT  
    policy_type AS PolicyType,  
    COUNT(policy_id) AS TotalPolicies, 
    ROUND(AVG(age)::NUMERIC, 2) AS AverageAge,  
    ROUND(AVG(tenure)::NUMERIC, 2) AS AverageTenure,
    ROUND(AVG(estimatedsalary)::NUMERIC, 1) AS AverageSalary,  
    SUM(policies_owned) AS TotalPoliciesOwned,
    SUM(is_active_policy) AS ActivePolicies
FROM incurance  
GROUP BY policy_type
ORDER BY TotalPolicies DESC;
```

Outputs:
| Policy Type | Total Policies | Average Age | Average Tenure | Average Salary | Total Policies Owned | Active Policies |
| :---------: | :------------: | :---------: | :------------: | :------------: | :------------------: | :-------------: |
| Health      | 2,507          | 40.99       | 5.01           | 96,525.8       | 3,789               | 1,242           |
| Car         | 2,502          | 38.94       | 5.07           | 105,551.0      | 3,807               | 1,313           |
| Home        | 2,496          | 38.75       | 5.05           | 111,092.5      | 3,836               | 1,302           |
| Life        | 2,495          | 39.01       | 4.92           | 100,197.5      | 3,870               | 1,294           |

```sql 
/* Customer profile by gender and police type */
SELECT 
    policy_type AS PolicyType, 
    gender AS Gender, 
    ROUND(AVG(age), 1) AS AverageAge, 
    COUNT(policy_id) AS TotalPolicies,
    ROUND(AVG(tenure), 2) AS AverageTenure,
    ROUND(AVG(estimatedsalary)::NUMERIC, 1) AS AverageSalary
FROM incurance
GROUP BY policy_type, gender
ORDER BY TotalPolicies DESC;
```

Outputs;
| Policy Type | Gender | Average Age | Total Policies | Average Tenure | Average Salary |
| :---------: | :----: | :---------: | :------------: | :------------: | :------------: |
| Car         | Male   | 38.4        | 1,440          | 5.12           | 98,714.7       |
| Health      | Male   | 38.9        | 1,344          | 5.01           | 97,972.5       |
| Home        | Male   | 38.8        | 1,340          | 5.01           | 101,920.6      |
| Life        | Male   | 38.6        | 1,333          | 5.06           | 100,129.0      |
| Health      | Female | 39.2        | 1,163          | 5.00           | 99,165.2       |
| Life        | Female | 39.5        | 1,162          | 4.76           | 100,276.2      |
| Home        | Female | 38.7        | 1,156          | 5.10           | 100,132.7      |
| Car         | Female | 39.7        | 1,062          | 5.01           | 103,040.8      |

```sql
/* Customer information based on estimated income groups */
SELECT 
    CASE 
        WHEN estimatedsalary < 30000 THEN 'Low Income'
        WHEN estimatedsalary BETWEEN 30000 AND 60000 THEN 'Middle Income'
        ELSE 'High Income'
    END AS IncomeGroup,
    ROUND(AVG(policies_owned), 2) AS AveragePoliciesOwned,
    COUNT(policy_id) AS TotalCustomers,
    round(avg(coverage_amount::int),1) as CoverageAmount
FROM incurance
GROUP BY IncomeGroup
ORDER BY AveragePoliciesOwned DESC;
```
Outputs;
| Income Group  | Average Policies Owned | Total Customers | Coverage Amount |
| :-----------: | :--------------------: | :-------------: | :-------------: |
| Middle Income | 1.53                   | 1,483           | 76,895.8        |
| High Income   | 1.53                   | 7,039           | 76,883.4        |
| Low Income    | 1.51                   | 1,478           | 74,181.3        |

```sql
/* 10`s Customer characteristics according to age groups */
SELECT 
    FLOOR(age / 10) * 10 AS AgeCohort, -- Yaşı 10'luk gruplara ayırır
    COUNT(policy_id) AS CustomerCount, 
    ROUND(AVG(tenure), 2) AS AverageTenure,
    SUM(policies_owned) AS TotalPoliciesOwned,
    ROUND(AVG(estimatedsalary::INT), 2) AS AverageSalary,
    ROUND(AVG(reward_points::INT), 2) AS AverageRewardPoints
FROM incurance
WHERE age IS NOT NULL
GROUP BY AgeCohort
ORDER BY AgeCohort;
```

Outputs;
| Age Cohort | Customer Count | Average Tenure | Total Policies Owned | Average Salary | Average Reward Points |
| :--------: | :------------: | :------------: | :------------------: | :------------: | :-------------------: |
| 10.0       | 49             | 4.96           | 70                   | 92,062.69      | 625.65               |
| 20.0       | 1,592          | 5.11           | 2,485                | 101,125.86     | 607.31               |
| 30.0       | 4,346          | 4.99           | 6,690                | 98,616.32      | 607.17               |
| 40.0       | 2,618          | 5.01           | 3,976                | 103,543.08     | 603.88               |
| 50.0       | 869            | 4.97           | 1,287                | 97,144.92      | 603.84               |
| 60.0       | 375            | 4.92           | 562                  | 97,859.57      | 611.49               |
| 70.0       | 136            | 4.99           | 208                  | 95,993.62      | 625.93               |
| 80.0       | 13             | 5.92           | 21                   | 102,709.46     | 591.08               |
| 90.0       | 2              | 2.00           | 3                    | 115,000.50     | 539.50               |

```sql
/* Number of customer satisfaction and complaints by policy type */
SELECT 
    policy_type AS PolicyType,
    COUNT(claim_complaint) AS TotalComplaints,
    ROUND(AVG(satisfaction_score), 2) AS AverageSatisfactionScore
FROM incurance
WHERE claim_complaint > 0
GROUP BY policy_type
ORDER BY TotalComplaints DESC;
```

Outputs;
| Policy Type | Total Complaints | Average Satisfaction Score |
| :---------: | :--------------: | :------------------------: |
| Health      | 547              | 3.01                       |
| Life        | 511              | 2.97                       |
| Home        | 502              | 2.94                       |
| Car         | 484              | 3.08                       |

--- 


### [**Return to Portfolio**](https://github.com/AtilaKzlts/Atilla-Portfolio)
