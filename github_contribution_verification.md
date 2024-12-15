
# **Schema Proposal: GitHub Verified Contribution Validation**


### **Data Source :**  
**GitHub**, the world's leading platform for software development and collaboration. GitHub allows developers to host, review, and manage their code repositories.

---

### **API Endpoint URL**  
**`GET https://api.github.com/users/{username}/repos`**  
- **Purpose**: This endpoint fetches a list of repositories owned by a GitHub user.  
- **Sample JSON Response**:
```json
[
    {
        "id": 123456789,
        "name": "example-repo",
        "full_name": "username/example-repo",
        "owner": {
            "login": "username",
            "id": 987654321
        },
        "private": false,
        "html_url": "https://github.com/username/example-repo",
        "description": "A sample repository",
        "fork": false,
        "created_at": "2023-01-01T00:00:00Z",
        "updated_at": "2024-01-01T00:00:00Z",
        "pushed_at": "2024-01-05T12:00:00Z",
        "stargazers_count": 100,
        "watchers_count": 100,
        "forks_count": 10,
        "open_issues_count": 2,
        "topics": ["blockchain", "zkpass", "security"],
        "has_issues": true,
        "has_wiki": true,
        "language": "Python",
        "license": {
            "key": "mit",
            "name": "MIT License"
        }
    }
] 
```


### **Technical Breakdown**  
This schema validates whether a GitHub user has made meaningful contributions to open-source projects based on the repositories they own.


#### **Key Validation Conditions**  
- **Public Repositories**: Must have at least one public repository (`private: false`).  
- **Stars and Forks**: A public repository must have a minimum of 50 stars (`stargazers_count`) and 5 forks (`forks_count`).  
- **Active Repository**: The repository must have been updated within the last 6 months (`updated_at`).  
- **Languages**: Repository should use a specified programming language, such as `Python`, `JavaScript`, etc.



#### **Key Use Cases**  
1. **Job Applications**: Verify that a developer has contributed to impactful projects for job applications or grant eligibility.  
2. **Hackathons and Competitions**: Validate open-source contributions for hackathons or competitive events.  
3. **Community Recognition**: Provide attestations for GitHub users who meet specific criteria for community awards or recognition programs.

### **Schema Code**

```json
{
  "issuer": "GitHub",
  "desc": "Verifies that a GitHub user has made significant contributions to public repositories",
  "website": "https://github.com",
  "APIs": [
    {
      "host": "api.github.com",
      "intercept": {
        "url": "/users/{username}/repos",
        "method": "GET"
      },
      "assert": [
        {
          "key": "private",
          "value": "false",
          "operation": "="
        },
        {
          "key": "stargazers_count",
          "value": "50",
          "operation": ">="
        },
        {
          "key": "forks_count",
          "value": "5",
          "operation": ">="
        },
        {
          "key": "updated_at",
          "value": "6 months ago",
          "operation": "recent"
        },
        {
          "key": "language",
          "value": ["Python", "JavaScript", "Go", "Rust"],
          "operation": "in"
        }
      ],
      "nullifier": "id"
    }
  ],
  "HRCondition": [
    "User must own at least one public repository with significant activity and community recognition."
  ],
  "tips": {
    "message": "Ensure your repository is public, active, and meets the contribution requirements before proceeding with validation."
  },
  "category": "Open Source",
  "id": "0xgithubSchemaVerified02"
}

```

#### **Demo/Test Case**

**Scenario**: A user attempts to authenticate their Discord account for participation in an exclusive gaming tournament.

1.  The API endpoint is queried upon user login using the `GET` method.
2.  A successful response is returned, containing the `verified: true` field.
3.  If the `verified` field is `true`, the schema validator issues an attestation that confirms the user meets the requirements for tournament entry.

**Real-world Use Case**:

-   This schema can be integrated with a gaming platform or community to allow only verified Discord users to join specific roles, access servers, or participate in competitive events.




