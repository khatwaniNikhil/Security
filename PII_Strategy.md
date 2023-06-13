# What is PII
1. identifiers: fname, lname, address etc.
2. Work, education
3. Biometric data
4. Internet data: browsing history, search history, IP addresses
5. Financial information: credit card numbers
6. Healthcare data: medical conditions or illnesses

# Ways to secure PII data
1. Data obfuscation
2. Data Scrambling
3. Adding Noise | stochastic substitution
4. Data Encryption
   ## Vault - Sensitive info. management
   1. Manages generation, storage, usage of sensitive info. like api keys, db access credentials
   2. For saas domain, manage tenant level data encryption keys
   3. tenant level keys are itself encrypted via master key
   4. tenant level keys are cached in api server

  ## Domain PII candidates - entities & fields
   1.  
