# AWS Infrastructure Common

AWS インフラ共通設定を管理するリポジトリ

## デプロイ

### プロファイル指定

AWS CLIでプロファイルを指定する場合は、各コマンドに `--profile` オプションを追加してください。

```bash
# 例: profileを指定してスタックを作成
aws cloudformation create-stack \
  --profile <profile-name> \
  --stack-name stack-infra-cloudtrail-main \
  ...
```

以下のコマンド例では `--profile` オプションを省略していますが、必要に応じて追加してください。

### CloudTrail設定

#### 作成

```bash
aws cloudformation create-stack \
  --stack-name stack-infra-cloudtrail-main \
  --template-body file://cloudtrail.yaml \
  --parameters \
    ParameterKey=S3BucketName,ParameterValue=s3-<任意の文字列>-infra-cloudtrail-main \
    ParameterKey=CloudTrailName,ParameterValue=cloudtrail-infra-main \
    ParameterKey=TableName,ParameterValue=table_infra_cloudtrail_main \
    ParameterKey=WorkgroupName,ParameterValue=InfraCloudTrail \
    ParameterKey=DatabaseName,ParameterValue=db_infra_cloudtrail_main \
  --capabilities CAPABILITY_NAMED_IAM
```

**注意**: `<任意の文字列>` の部分は任意の識別子（例: プロジェクト名、環境名など）に置き換えてください。

#### 更新

```bash
aws cloudformation update-stack \
  --stack-name stack-infra-cloudtrail-main \
  --template-body file://cloudtrail.yaml \
  --parameters \
    ParameterKey=S3BucketName,ParameterValue=s3-<任意の文字列>-infra-cloudtrail-main \
    ParameterKey=CloudTrailName,ParameterValue=cloudtrail-infra-main \
    ParameterKey=TableName,ParameterValue=table_infra_cloudtrail_main \
    ParameterKey=WorkgroupName,ParameterValue=InfraCloudTrail \
    ParameterKey=DatabaseName,ParameterValue=db_infra_cloudtrail_main \
  --capabilities CAPABILITY_NAMED_IAM
```

#### 削除

```bash
aws cloudformation delete-stack \
  --stack-name stack-infra-cloudtrail-main
```

**注意**: S3バケットにデータが残っている場合、スタック削除前に手動でバケットを空にする必要があります。

### アクセスキー無効化Lambda

#### 作成

```bash
aws cloudformation create-stack \
  --stack-name stack-infra-disable-access-key-01 \
  --template-body file://disable_access_key.yaml \
  --parameters \
    ParameterKey=AccessKeyId,ParameterValue=<your-access-key-id> \
    ParameterKey=AccessKeyUserName,ParameterValue=<iam-user-name> \
    ParameterKey=LambdaFunctionName,ParameterValue=lambda-infra-disable-access-key-001 \
    ParameterKey=CloudWatchRuleName,ParameterValue=schecule-infra-lambda-infra-disable-access-key-001 \
    ParameterKey=IAMRoleName,ParameterValue=role-infra-lambda-disable-access-key-001 \
  --capabilities CAPABILITY_NAMED_IAM
```

**注意**: `<your-access-key-id>` と `<iam-user-name>` は実際の値に置き換えてください。

#### 更新

```bash
aws cloudformation update-stack \
  --stack-name stack-infra-disable-access-key-01 \
  --template-body file://disable_access_key.yaml \
  --parameters \
    ParameterKey=AccessKeyId,ParameterValue=<your-access-key-id> \
    ParameterKey=AccessKeyUserName,ParameterValue=<iam-user-name> \
    ParameterKey=LambdaFunctionName,ParameterValue=lambda-infra-disable-access-key-001 \
    ParameterKey=CloudWatchRuleName,ParameterValue=schecule-infra-lambda-infra-disable-access-key-001 \
    ParameterKey=IAMRoleName,ParameterValue=role-infra-lambda-disable-access-key-001 \
  --capabilities CAPABILITY_NAMED_IAM
```

#### 削除

```bash
aws cloudformation delete-stack \
  --stack-name stack-infra-disable-access-key-01
```
