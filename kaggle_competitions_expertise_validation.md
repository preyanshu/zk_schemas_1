
# **Schema Proposal: Kaggle Competitions Expertise Validation**


### **Data Source :**  
**Kaggle**, a popular platform for data science and machine learning competitions. It allows users to participate in real-world problems and showcase their skills through rankings and competition performance.

---

### **API Endpoint URL**  
**`GET https://www.kaggle.com/api/v1/user/{username}/competitions`**  
- **Purpose**: This endpoint retrieves details about the competitions a user has participated in, including their rankings, medals, and points.  
- **Sample JSON Response**:
```json
{
    "username": "data_master",
    "competitions": [
        {
            "title": "Titanic - Machine Learning from Disaster",
            "rank": 12,
            "medal": "gold",
            "points": 450,
            "teamSize": 1,
            "submissionDate": "2024-01-10T10:00:00Z"
        },
        {
            "title": "House Prices - Advanced Regression Techniques",
            "rank": 50,
            "medal": "silver",
            "points": 200,
            "teamSize": 3,
            "submissionDate": "2023-12-05T15:00:00Z"
        }
    ],
    "totalPoints": 650,
    "medals": {
        "gold": 1,
        "silver": 1,
        "bronze": 0
    }
}

```


### **Technical Breakdown**  
This schema validates a user's expertise in data science and machine learning competitions hosted on Kaggle.


#### **Key Validation Conditions**  
- The user must have participated in at least one competition with a rank in the top 10% (rank ≤ 10% of total participants).  
- The user must have earned at least one gold or silver medal in a competition.  
- The totalPoints field must exceed 500 to confirm the user is an active and skilled participant.



#### **Key Use Cases**  
1. Verification of data science expertise for job applications or freelance project bids. 
2. Qualification for advanced workshops, mentorship programs, or hackathons.
3. Validation for earning badges or recognitions within data science learning platforms.

### **Schema Code**

```json
{
  "issuer": "Kaggle",
  "desc": "Validates a user's expertise in data science and machine learning competitions.",
  "website": "https://www.kaggle.com",
  "APIs": [
    {
      "host": "kaggle.com",
      "intercept": {
        "url": "/api/v1/user/{username}/competitions",
        "method": "GET"
      },
      "assert": [
        {
          "key": "competitions[*].medal",
          "value": "gold|silver",
          "operation": "in"
        },
        {
          "key": "totalPoints",
          "value": "500",
          "operation": ">="
        },
        {
          "key": "competitions[*].rank",
          "value": "10%",
          "operation": "<="
        }
      ],
      "nullifier": "username"
    }
  ],
  "HRCondition": [
    "User must have at least one gold or silver medal and a minimum of 500 total points in Kaggle competitions."
  ],
  "tips": {
    "message": "Ensure your Kaggle profile is public, and you have participated in competitions to meet the validation criteria."
  },
  "category": "Data Science",
  "id": "0xkaggleSchemaExpertise01"
}


```

#### **Demo/Test Case**

**Scenario**: A data scientist applies for a job requiring proven expertise in real-world machine learning competitions.

1. The Kaggle API is queried for the user's profile using the provided username.
2. The response is validated to confirm the following:
    1. At least one gold or silver medal is present.
    2. The user has at least 500 total points.
    3. The user ranks in the top 10% in at least one competition.
3.  If the conditions are met, the schema validator issues an attestation confirming the user's eligibility.

**Real-world Use Case**:

- This schema can be used by companies or platforms to verify data scientists’ experience and skills through their performance in Kaggle competitions. It is particularly useful for recruiting, certifications, or awarding exclusive project opportunities.




