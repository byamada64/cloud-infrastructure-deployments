# 🔐 AWS Secrets Manager Deployment

Provisioned and validated AWS Secrets Manager to demonstrate secure credential storage, KMS-backed encryption, centralized secret lifecycle management, and cloud-native access control for production-style workloads.


## 🏗️ Environment Build Choices

### Project Details

- **AWS Account:** Temporary sandbox / lab environment
- **Region:** Current active lab region
- **Service:** AWS Secrets Manager

### Secret Configuration

- **Secret Type:** Other type of secret
- **Encryption Key:** aws/secretsmanager (AWS managed KMS key)
- **Secret Name:** `prod/appbeta/mysql`

### Stored Key / Value Pairs

- `username = admin`
- `password = SecurePass123!`
- `api_token = PROD-XYZ-789`

### Rotation

- Automatic rotation reviewed
- Rotation disabled for initial build simplicity

### Notes

- Some database discovery banners showed SCP restrictions in sandbox
- Generic secret creation remained fully functional
- AWS sample SDK retrieval code was generated automatically after secret creation


## 💻 Commands Used

- AWS Console used for full Secrets Manager deployment workflow
- No CLI required during initial build
- Reviewed AWS-generated retrieval examples for Java and other SDK-supported languages


## ✅ Validation

- Secret created successfully
- Secret stored with KMS encryption
- Secret values accessible through controlled reveal workflow
- Secret details page loaded successfully
- Edit, retrieve, replicate, and delete options visible
- Rotation settings available
- Resource permissions and policy options available
- SDK sample retrieval code generated automatically


## 🧠 Lessons Learned

- AWS Secrets Manager securely stores passwords, tokens, and application credentials
- KMS encryption protects secrets at rest
- Applications can retrieve secrets dynamically using SDK/API integrations
- Avoids hardcoding passwords in source code or configuration files
- IAM controls who can access or manage secrets
- Rotation supports automated credential lifecycle management
- SCP restrictions can block related AWS services while core Secrets Manager functions still operate
- Secrets Manager aligns closely with enterprise PAM and credential governance models


## 🔮 Future Improvements

- Enable automatic rotation using AWS Lambda
- Apply IAM least-privilege access to secret retrieval
- Integrate EC2 or Lambda workloads with runtime secret retrieval
- Store database connection strings and certificates
- Integrate CI/CD pipelines with secure secret access
- Enable CloudTrail alerting for secret access events
- Replicate secrets across regions for resiliency


## 🧹 Final Hygiene Cleanup

- Delete test secret after validation if no longer needed
- Remove stale or unused secret versions
- Review access permissions before production use
- Confirm no unnecessary resources remain in sandbox environment
- Maintain clean credential governance practices
