# Module 1 : 온프레미스 및 클라우드 내 지역에 리소스 배포

Module 1에서는 CloudFormation 스크립트를 사용하여 두 개의 AWS 리전에 리소스를 배포합니다.<br>
하나는 온프레미스 환경을 나타내고 다른 하나는 클라우드 내 환경을 나타내는데, 우선 모든 리소스가 배포되면 애플리케이션 서버의 NFS 서버에서 내보내기를 마운트하고 기존 파일을 확인합니다.<br>

![1-1](../images/1-1.png)

### Module Steps (👉🏻*Storage 모든 실습을 us-east-1: US East(N. Virginia)에서 진행합니다.*)
***
1. 온프레미스 지역에 대한 AWS 리소스 배포<br>
    a. 환경을 자동으로 배포하기 위해서 CloudFormation을 사용합니다. 온프레미스 리소스를 배포하려면 아래 표의 us-east-1: US East(N. Virginia)를 선택해주세요.<br>
   
|Region Code|Region Name|Launch|
|------|---|---|
|us-east-1|US East(N. Virginia)|![Launch in us-east-1](https://us-east-1.console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/create?stackName=DataMigrationWorkshop-onPremResources&templateURL=https://aws-datasync-samples.s3-us-west-2.amazonaws.com/workshops/nfs-migration/data-migration-workshop-on-prem.yaml)|

    1. 스택 생성 페이지에서 다음을 클릭합니다.
    2. 로컬 SSH 클라이언트를 사용하여 이 워크샵에서 생성된 EC2 인스턴스에 액세스하려면 선택한 리전에서 EC2 키 쌍의 이름을 입력하십시오. 그렇지 않으면 키 쌍을 비워 둘 수 있습니다.
    3. Next 클릭
    4. Next 클릭
    5. Next을 다시 클릭(옵션 및 고급 옵션 섹션 건너뛰기)
    6. review 페이지에서 아래로 스크롤하여 CloudFormation이 IAM 리소스를 생성함을 확인하는 확인란을 선택한 다음 스택 생성을 클릭합니다.
   
참고: 이 CloudFormation 템플릿의 일부로 시작된 인스턴스는 몇 분 동안 초기화 중 상태일 수 있습니다.<br>
자~ 이제 온프레미스 지역에서 이 CloudFormation 배포가 진행되는 동안 클라우드 내 지역에 대한 리소스를 또 다른 CloudFormation으로 동시에 배포를 진행해 보시죠.<br>

2. IN-CLOUD 리전에 대한 AWS 리소스 배포<br>
      a. 환경을 자동으로 배포하기 위해서 CloudFormation을 사용합니다. IN-CLOUD 리소스를 배포하려면 아래 표의 us-east-1: US East(N. Virginia)를 선택해주세요.<br>
   
|Region Code|Region Name|Launch|
|------|---|---|
|us-east-1|US East(N. Virginia)|![Launch in us-east-1](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/new?stackName=DataMigrationWorkshop-inCloudResources&templateURL=https://aws-datasync-samples.s3-us-west-2.amazonaws.com/workshops/nfs-migration/data-migration-workshop-in-cloud.yaml)|

    1. 스택 생성 페이지에서 다음을 클릭합니다.
    2. Next 클릭(스택 parameters 없음).
    3. Next을 다시 클릭(옵션 및 고급 옵션 섹션 건너뛰기)
    4. review 페이지에서 아래로 스크롤하여 CloudFormation이 IAM 리소스를 생성함을 확인하는 확인란을 선택한 다음 스택 생성을 클릭합니다.

참고: 다음 단계를 진행하기 전에 각 리전의 CloudFormation 스택이 CREATE_COMPLETE 상태에 도달할 때까지 기다리십시오. 우리가 지금 실행 한 위의 2개 CloudFormation 스택이 완료되는 데 약 10분이 소요됩니다.<br>


