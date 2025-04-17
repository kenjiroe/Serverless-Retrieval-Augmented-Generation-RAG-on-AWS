# ขั้นตอนการ Deploy Serverless RAG บน AWS

## Prerequisites (สิ่งที่ต้องมี)
- Node.js >= v18.18.2
- NVM (Node Version Manager)
- Docker
- AWS Account และ AWS CLI
- AWS CDK CLI >= 2.142.1

## 1. เตรียมความพร้อมระบบ
```bash
# ตรวจสอบ Node.js version
node -v

# ใช้ Node.js version ที่ถูกต้อง
nvm use

# ติดตั้ง AWS CDK CLI (ถ้ายังไม่มี)
npm install -g aws-cdk
```

## 2. ตั้งค่า AWS Credentials
```bash
# ตั้งค่า AWS credentials และ region
aws configure

# กรอกข้อมูลต่อไปนี้:
# AWS Access Key ID: [กรอก Access Key]
# AWS Secret Access Key: [กรอก Secret Key]
# Default region name: us-west-2
# Default output format: [กด Enter]
```

## 3. ตั้งค่า IAM Permissions
ต้องมั่นใจว่า IAM User มีสิทธิ์ดังต่อไปนี้:
- AmazonEC2ContainerRegistryFullAccess
- AdministratorAccess

วิธีเพิ่มสิทธิ์:
1. ไปที่ AWS Console > IAM
2. เลือก Users
3. เลือก user ที่ต้องการ
4. คลิก "Add permissions"
5. เลือก "Attach existing policies directly"
6. เลือก policies ที่ต้องการ

## 4. ติดตั้ง Dependencies
```bash
# ติดตั้ง dependencies ของโปรเจกต์
npm install
```

## 5. Bootstrap CDK
```bash
# ทำครั้งแรกครั้งเดียวต่อ region
cdk bootstrap --region us-west-2

# ถ้าเจอ error เรื่อง region ลองใช้:
export AWS_DEFAULT_REGION=us-west-2
cdk bootstrap
```

## 6. Deploy Stack
```bash
# Deploy stack
cdk deploy
```

## 7. ตรวจสอบ Outputs
หลังจาก deploy สำเร็จ จะได้ outputs ประมาณนี้:
```bash
Outputs:
ServerlessRagOnAwsStack.WebDistributionName = https://dxxxxxxxxxx.cloudfront.net
ServerlessRagOnAwsStack.userPoolId = us-west-2_xxxxxxxxxx
ServerlessRagOnAwsStack.webClientId = xxxxxxxxxxxxx
# ... และ outputs อื่นๆ
```

## 8. ทดสอบระบบ
1. เปิด URL จาก `WebDistributionName` ในเบราว์เซอร์
2. สร้างบัญชีผู้ใช้ใหม่
3. ทดสอบการใช้งานระบบ

## หมายเหตุสำคัญ
1. **Region**: แนะนำให้ใช้ `us-west-2` เพราะมี LLM models ให้ใช้งานมากที่สุด
2. **เวลา**: การ deploy ครั้งแรกจะใช้เวลาประมาณ 20 นาที
3. **ค่าใช้จ่าย**: จะมีค่าใช้จ่ายเกิดขึ้นจากการใช้บริการต่างๆ ของ AWS

## การแก้ไขปัญหาที่พบบ่อย

### 1. Region NONE Error
```bash
export AWS_DEFAULT_REGION=us-west-2
```

### 2. Permission Error
ตรวจสอบว่ามีสิทธิ์ที่จำเป็นครบถ้วน (ดูขั้นตอนที่ 3)

### 3. Node Version Error
```bash
nvm install 18.18.2
nvm use 18.18.2
```

### 4. Docker Error
- ตรวจสอบว่า Docker daemon กำลังทำงานอยู่
- รัน `docker ps` เพื่อทดสอบ

## การลบ Stack
หากต้องการลบ stack ทั้งหมด:
```bash
cdk destroy
```

## ข้อมูลเพิ่มเติม
สำหรับรายละเอียดเพิ่มเติม สามารถดูได้ที่:
- [README.md](./README.md)
- [AWS CDK Documentation](https://docs.aws.amazon.com/cdk/)

 ✅  ServerlessRagOnAwsStack

✨  Deployment time: 447.24s

Outputs:
ServerlessRagOnAwsStack.FrontendConfigS3Path = s3://serverlessragonawsstack-frontendbucketefe2e19c-r0mzckedqm8v/appconfig.json
ServerlessRagOnAwsStack.WebDistributionName = https://d159hs7vugk70u.cloudfront.net
ServerlessRagOnAwsStack.allowUnauthenticatedIdentities = true
ServerlessRagOnAwsStack.authRegion = us-west-2
ServerlessRagOnAwsStack.identityPoolId = us-west-2:264349ab-de24-4f1a-be74-b34354e9b23b
ServerlessRagOnAwsStack.mfaConfiguration = OFF
ServerlessRagOnAwsStack.mfaTypes = []
ServerlessRagOnAwsStack.oauthClientId = 7a4a3nmrkvbsutdo6fh7u7mfse
ServerlessRagOnAwsStack.oauthCognitoDomain = 
ServerlessRagOnAwsStack.oauthRedirectSignIn = https://example.com
ServerlessRagOnAwsStack.oauthRedirectSignOut = 
ServerlessRagOnAwsStack.oauthResponseType = code
ServerlessRagOnAwsStack.oauthScope = ["profile","phone","email","openid","aws.cognito.signin.user.admin"]
ServerlessRagOnAwsStack.passwordPolicyMinLength = 8
ServerlessRagOnAwsStack.passwordPolicyRequirements = ["REQUIRES_NUMBERS","REQUIRES_LOWERCASE","REQUIRES_UPPERCASE","REQUIRES_SYMBOLS"]
ServerlessRagOnAwsStack.signupAttributes = ["email"]
ServerlessRagOnAwsStack.socialProviders = 
ServerlessRagOnAwsStack.userPoolId = us-west-2_ePhj9Xr3K
ServerlessRagOnAwsStack.usernameAttributes = ["email"]
ServerlessRagOnAwsStack.verificationMechanisms = ["email"]
ServerlessRagOnAwsStack.webClientId = 7a4a3nmrkvbsutdo6fh7u7mfse
Stack ARN:
arn:aws:cloudformation:us-west-2:703671924712:stack/ServerlessRagOnAwsStack/396afcf0-1b4d-11f0-9e80-064e73e9acd7

✨  Total time: 479.62s