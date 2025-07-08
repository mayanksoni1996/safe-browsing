# 🛡️ Safe Browsing System

A phishing and typosquatting detection system built to validate whether a domain may be impersonating a trusted website. The system performs real-time domain validation by comparing against a known list of popular domains (Tranco).

---

## 🚀 How It Works

### 1. **Trusted Domains Initialization**

- On system startup, the [Tranco Full List](https://tranco-list.eu/) (~4.2 million domains) is loaded into memory.
- A preprocessing step is applied to:
    - Normalize domains.
    - Store the length and first character of each domain for efficient filtering.
    - Buffer the dataset into memory (this step may take a few minutes).

### 2. **Domain Validation Workflow**

When a domain is submitted for validation:

1. **Pre-filter** the Tranco domains based on:
    - Same first character.
    - Similar length (within ±2 characters).

2. **Compute Edit Distance** between the input domain and the filtered trusted domains.

3. **Decision Logic**:
    - If **edit distance < 2** for any trusted domain:
        - The system considers it a **potential typosquatting attempt**.
        - The response includes a warning and a list of closest matching trusted domains.
    - If no such domain is found, the domain is considered safe.

---

## ⚙️ Key Features

- 🧠 **Smart Preprocessing**: Optimized domain representation for fast lookup and memory efficiency.
- ⚡ **Efficient Matching**: Minimizes comparisons using structural domain filters.
- 🧮 **Edit Distance Algorithm**: Levenshtein Distance is used to detect visual similarity.
- 🛑 **Phishing Detection**: Warn users of possible impersonation attempts in real-time.

---

## 🧪 Example Use Case

Suppose a user visits `g00gle.com`.  
The system will:

- Extract `g00gle.com`.
- Filter Tranco domains starting with `g` and with a similar length.
- Compute edit distances.
- Detect that `google.com` is 2 character off.
  - Return a response:
    ```json
    {
    "stateModel": {
      "state": "UUID-1234-5678-90AB-CDEF12345678",
      "domainName": "example.com",
      "ipAddress": "127.0.0.1",
      "accessAllowed": false,
      "accessOverrideControlAvailable": true,
      "stateExpiresAt": "2022-01-01T00:00:00Z"
    },
    "typosquattingValidationResults": {
      "isTyposquatted": true,
      "domainName": "example.com",
      "closestMatchingDomain": "example.com",
      "editDistance": 1,
      "isPhoneticMatch": true,
      "phoneticMatchingDomain": "example.com",
      "phoneticMatchType": "SOUNDEX"
    }
    }
      ```
