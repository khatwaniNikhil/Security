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
      #### Background
      1. Manages generation, storage, usage of sensitive info. like api keys, db access credentials
      2. For saas domain, manage tenant level data encryption keys
      3. tenant level keys are itself encrypted via master key
      4. tenant level keys are cached in api server
      
      #### Storage backend
      Consul is recommend.
      S3 can be used as an alternative
      1. AWS side availability and durability
      2. Vault side HA setup not possible, we used Active standby based clusted behind HAProxy.

      #### Secret engine
      1. Using vault as Encryption as a service: Transit engine was used as only keys were stored in vault and actual encrypted data was stored in mysql, mongo 
      2. custom encryption logic (like AES-256 ) to encrypt credentials, use Vault for tenant specific Key Management
   
      #### Access Control
      Tenant specific users with access to only its own keys
      
      #### Authentication method
      "userpass" auth
      
# Application side Implementation 
1. spring module with vault related application context

# Sample Vault commands
1.  Login 
curl --request POST --data '{"password": ""}' http://domain/v1/auth/userpass/login/sampleTenant

2.  Encrypt (send base64 encoded string in request body)
curl -v --header "X-Vault-Token: sampleToken" --request POST --data '{"plaintext": ""}' http://domain/v1/transit/encrypt/$tenant

3. Decrypt (returns base64 encoded decrypted string)
curl -v --header "X-Vault-Token: $token" --request POST --data '{"ciphertext":"vault:v1:$data"}' http://domain/v1/transit/decrypt/$tenant

3. Fetching Encryption KEYS from Vault
   1. Specific Version
     curl --header "X-Vault-Token: $token" http://127.0.0.1:8200/v1/transit/encryption-key/$tenant/1
   2. Latest Version
    curl --header "X-Vault-Token: $token" http://127.0.0.1:8200/v1/transit/export/encryption-key/$tenant/latest
   3. All version
    curl --header "X-Vault-Token: $token" http://127.0.0.1:8200/v1/transit/export/encryption-key/$tenant
