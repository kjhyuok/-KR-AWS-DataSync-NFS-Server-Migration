# Module 1 : 온프레미스 및 클라우드 내 지역에 리소스 배포

Module 1에서는 CloudFormation 스크립트를 사용하여 두 개의 AWS 리전에 리소스를 배포합니다.<br>
하나는 온프레미스 환경을 나타내고 다른 하나는 클라우드 내 환경을 나타내는데, 우선 모든 리소스가 배포되면 애플리케이션 서버의 NFS 서버에서 내보내기를 마운트하고 기존 파일을 확인합니다.<br>

![1-1](../images/1-1.png)

### Module Steps
***
1. 온프레미스 지역에 대한 AWS 리소스 배포<br>
   a. CloudFormation을 사용하여 온프레미스 리소스를 배포하려면 아래 표의 실행 링크 중 하나를 클릭하십시오. 배포 중 오류를 방지하려면 이전에 AWS 리소스를 생성한 지역을 선택하십시오.<br>
   
|Region Code|Region Name|Launch|
|------|---|---|
|us-east-1|US East(N. Virginia)|![Launch in us-east-1](https://us-east-1.console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/create?stackName=DataMigrationWorkshop-onPremResources&templateURL=https://aws-datasync-samples.s3-us-west-2.amazonaws.com/workshops/nfs-migration/data-migration-workshop-on-prem.yaml)|
