# AWS datasync로 NFS 서버를 AWS S3로 마이그레이션 해보자!

스토리지 시스템과 서비스 간의 데이터 이동을 단순화, 자동화 및 가속화하는 온라인 데이터 전송 서비스인 AWS datasync를 사용해 온프레미스에 위치한 NFS 서버를 AWS S3로 마이그레이션 하고, 온프레미스에 있는 Application 서버가 사용하던 NFS를 AWS File Gateway를 통해 S3로 대체해 보는 실습입니다.<br>
기존 AWS의 Workshop인 [**AWS DataSync: NFS Server Migration**]([https://catalog.us-east-1.prod.workshops.aws/workshops/976050cc-0606-4b23-b49f-ca7b8ac4b153/en-US/](https://catalog.workshops.aws/datasync-nfs-server-migration-with-storage-gateway/en-US))을 한글화 하고, 실습자에게 필요한 부분만 구성합니다.<br>
본 Workshop은 약 1~1.5시간 정도 소요되며, 활용되는 AWS Resource 목록은 다음과 같습니다.

* [AWS Datasync](https://aws.amazon.com/ko/datasync/), [Amazon S3](https://aws.amazon.com/ko/s3/), [AWS Storage gateway](https://aws.amazon.com/ko/storagegateway/) 그리고 코드기반으로 실습환경을 자동으로 프로비저닝해 줄 [AWS Formation](https://aws.amazon.com/ko/cloudformation/)이 활용됩니다.
*****  
### 워크샵 시나리오: AWS DataSync 및 AWS Storage Gateway를 사용한 NFS 서버 마이그레이션<br>
데이터 센터에 오래된 NFS 서버가 있고, NFS 서버에 있는 대부분의 데이터는 몇 년 된 것이며 가끔 읽기 위해서만 액세스됩니다.<br>서버에 새 파일이 기록되고 있지만 자주 기록되지는 않습니다. 데이터 센터 공간을 줄이고 리소스를 확보하기 위해 NFS 서버의 데이터를 클라우드로 이동하려고 합니다. 그러나 NFS 데이터에 액세스하는 애플리케이션 서버는 아직 이동할 수 없습니다. 사용자의 대기 시간을 최소화하려면 온프레미스에 있어야 합니다.<br><br>
몇 가지의 조사를 해본 결과 [AWS DataSync](https://aws.amazon.com/ko/datasync/)를 사용하여 온프레미스 NFS 서버에서 Amazon S3로 데이터를 마이그레이션할 수 있다는 것을 알게되었습니다. 데이터가 S3에 있을 때 [AWS Storage gateway](https://aws.amazon.com/ko/storagegateway/)를 사용하여 온프레미스에서 NFS 액세스를 제공할 수 있습니다.<br><br>
이 워크샵에서는 Cloud Formation 템플릿을 사용하여 리소스를 배포하고 AWS 관리 콘솔을 사용하여 리소스를 적절하게 구성하는 방법을 설명합니다.<br>아래 아키텍처 다이어그램에 표시된 것처럼 NFS 서버, 애플리케이션 서버, DataSync 에이전트 및 File Gateway 어플라이언스가 AWS 영역에 구축되어 사내 환경을 시뮬레이션합니다. NFS 서버의 데이터가 마이그레이션될 AWS 클라우드 영역을 시뮬레이션하는 S3 버킷이 AWS 영역에 생성됩니다.<br><br>
![intro](./images/intro.png)

### 다루게 될 주제들 <br>
* Cloud Formation을 사용하여 리소스 배포
* DataSync를 사용하여 NFS 파일을 S3으로 마이그레이션
* NFS를 사용하여 사내에서 S3 호스팅된 파일을 처리하도록 Storage Gateway 구성
* DataSync를 통한 증분 복사
* NFS 서버 해체 및 Storage Gateway로의 컷오버

*****
### 전제조건<br>
#### AWS Account<br>
이 워크샵을 완료하려면 선택한 AWS 영역에서 AWS IAM 역할, EC2 인스턴스, AWS DataSync, AWS Storage Gateway 및 Cloud Formation 스택을 생성할 수 있는 권한을 가진 AWS 계정이 필요합니다.
#### Software<br>
인터넷 브라우저 - 이 워크샵에서는 최신 버전의 Chrome 또는 Firefox를 사용하는 것이 좋습니다.<br>
*****
### 비용<br>
이 Workshop을 따라서 실습하는 데 약 3.00 USD의 비용이 듭니다. Workshop을을 완료한 후 정리 지침에 따라 배포된 모든 리소스를 제거하고 AWS 계정에 대한 지속적인 비용을 제한하는 것이 좋습니다.<br>
*****
### Workshop 모듈 및 소개<br>
이 워크샵은 다음 5개의 모듈로 구성됩니다.<br>
* [Module1](./detail/module1.md) - 온프레미스 및 클라우드 내 region에 리소스 배포
* [Module2](./detail/module2.md) - DataSync를 사용하여 S3에 초기 파일 복사
* [Module3](./detail/module3.md) - Storage Gateway를 사용하여 온프레미스에서 S3 버킷에 액세스
* [Module4](./detail/module4.md) - 컷오버 전 마지막 증분 복제본
* [Momdule5](./detail/module5.md) - Storage Gateway로 컷오버, NFS 서버 종료 및 워크샵 정리<br><br>
자~ 이제 워크샵 시작을 위해 [Module1](./detail/module1.md)로 이동해 볼까요?
