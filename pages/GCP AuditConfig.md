- ```
  auditConfigs:
  - auditLogConfigs:
    - logType: ADMIN_READ
    - logType: DATA_WRITE
    - logType: DATA_READ
    service: allServices
  - auditLogConfigs:
    - exemptedMembers:
      - 499862534253-compute@developer.gserviceaccount.com
      logType: ADMIN_READ
    service: cloudsql.googleapis.com
  ```