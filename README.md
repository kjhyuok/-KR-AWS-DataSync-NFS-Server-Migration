# 이제 온프레미스에서도 AWS Storage Gateway, AWS datasync를 활용하여 AWS S3를 NFS 처럼 사용해 봅시다!

**AWS Builders Online 2023** 스토리지 학습에 오신 여러분들을 환영합니다.😃\
AWS의 스토리지 시스템과 기존 서비스 간의 데이터 이동을 단순화, 자동화 및 가속화하는 AWS 온라인 데이터 전송 서비스인 **AWS Datasync**를 사용해 온프레미스에 위치한 NFS 서버를 AWS S3로 마이그레이션 하고, 온프레미스에 있는 Application 서버가 사용하던 NFS를 **AWS File Gateway**를 통해 S3로 대체해 보는 워크샵(실습)을 진행해 볼 예정입니다.\
기존 AWS의 Workshop인 [**AWS DataSync: NFS Server Migration**](\[https:/catalog.us-east-1.prod.workshops.aws/workshops/976050cc-0606-4b23-b49f-ca7b8ac4b153/en-US/]\(https:/catalog.workshops.aws/datasync-nfs-server-migration-with-storage-gateway/en-US\)/)을 한글화 하고, 실습자에게 필요한 부분을 재구성 했습니다.\
본 Workshop은 약 1\~1.5시간 정도 소요되며, 활용되는 AWS Resource 목록은 다음과 같습니다.

* [AWS Datasync](https://aws.amazon.com/ko/datasync/), [Amazon S3](https://aws.amazon.com/ko/s3/), [AWS Storage gateway](https://aws.amazon.com/ko/storagegateway/) 그리고 코드기반으로 실습환경을 자동으로 프로비저닝해 줄 [AWS CloudFormation](https://aws.amazon.com/ko/cloudformation/)이 활용됩니다.

### 워크샵 시나리오: AWS DataSync 및 AWS Storage Gateway를 통해서 온프레미스 NFS를 정리하고, 저렴한 AWS 클라우드의 S3로 대체해 보기

온프레미스에 오래된 NFS 서버가 있고, NFS 서버에 있는 대부분의 데이터는 몇 년 된 것이며 가끔 읽기 위해서만 액세스됩니다.\
서버에 새 파일이 기록되고 있지만 자주 기록되지는 않습니다. 온프레미스 공간을 줄이고 리소스 확보를 위해 NFS 서버의 데이터를 클라우드로 이동하려고 합니다만 NFS 데이터에 액세스하는 Application 서버는 아직 이동할 수 없는 상황입니다.\
\
몇 가지의 조사를 해본 결과 [AWS DataSync](https://aws.amazon.com/ko/datasync/)를 사용하여 온프레미스 NFS 서버에서 Amazon S3로 데이터를 마이그레이션할 수 있다는 것을 알게되었습니다. 데이터가 S3에 있을 때 [AWS Storage gateway](https://aws.amazon.com/ko/storagegateway/)를 사용하여 온프레미스에서 NFS 액세스를 제공할 수 있습니다.\
\
이 워크샵에서는 **CloudFormation** 템플릿을 사용하여 리소스를 자동 배포하고 AWS 관리 콘솔을 사용하여 리소스를 적절하게 구성하는 방법을 설명합니다.\
아래 아키텍처 다이어그램에 표시된 것처럼 NFS 서버, 애플리케이션 서버, DataSync 에이전트 및 File Gateway 를 AWS 리전에 배포하고 On-Premise 환경을 시뮬레이션합니다. NFS 서버의 데이터가 마이그레이션될 AWS 클라우드 영역을 시뮬레이션하는 S3 버킷이 AWS 환경(이하 _IN-CLOUD 리전_)에 생성됩니다.\


![intro](<images/3-1 (4).png>)

### 다루게 될 주제들

* CloudFormation을 사용하여 리소스 배포하기
* DataSync를 사용하여 NFS 파일을 S3으로 마이그레이션
* NFS를 사용하여 사내에서 S3 호스팅된 파일을 처리하도록 Storage Gateway 구성
* DataSync를 통한 증분 복사
* NFS 서버 해체 및 Storage Gateway로의 컷오버

### 전제조건

#### AWS Account

이 워크샵을 완료하려면 선택한 AWS 영역에서 AWS IAM Roles, EC2 Instance, AWS DataSync, AWS Storage Gateway 및 CloudFormation 스택을 생성할 수 있는 권한을 가진 AWS Account가 필요합니다.

#### Software

인터넷 브라우저 - 이 워크샵에서는 최신 버전의 Chrome 또는 Firefox를 사용하는 것이 좋습니다.

### 비용

이 Workshop을 따라서 실습하는 데 약 3.00 USD의 비용이 듭니다. Workshop을 완료한 후 정리 지침에 따라 배포된 모든 리소스를 제거하고 AWS 계정에 대한 지속적인 비용을 제한하는 것이 좋습니다.

### Workshop 모듈 및 소개

이 워크샵은 다음 6개의 모듈로 구성되고 사전 Preparations 단계를 통해서 AWS Console 사용을 위한 기본적인 설정을 진행 합니다.

* [Preparations](detail/Preparations.md) - 실습 사전 준비
* [Module1](detail/module1.md) - 실습용 AWS Resource 배포(가상의 On-premises와 클라우드 인프라 구성)하기
* [Module2](detail/module2.md) - AWS DataSync를 사용해서 Amazon S3에 기존 NFS 파일을 복사하기
* [Module3](detail/module3.md) - On-premises내 서버가 AWS Storage Gateway를 통해 Amazon S3로의 접근하기
* [Module4](detail/module4.md) - On-premises내 서버가 NFS 컷오버(기존 NFS 연결해제) 수행 전 마지막 증분 데이터를 Amazon S3로 복제
* [Module5](detail/module5.md) - AWS Storage Gateway로 컷오버하여 On-premises내 서버와 Amazon S3 간 완전한 연결
* [Module6](detail/module6/s3-1.md) - 이제 Amazon S3를 더 똑똑한 스토리지로 활용하기 위한 작업
* [Module7](detail/module7.md) - CleanUp: 실습에 사용했던 모든 자원을 정리

* [Contact Me](detail/Contactme.md)


_**Cut-Over란**_: 기존에 운영되던 정보시스템을 완전히 중단시키고, 새로 구축된 정보시스템을 본격 오픈하는 작업입니다. 본 실습 예제에서는 기존 On-premise내 NFS 서버를 AWS Storage gw로 대체하는 작업을 지칭합니다.


자\~ 이제 실습 준비를 위해 [Preparations](detail/Preparations.md)으로 이동해 볼까요?
