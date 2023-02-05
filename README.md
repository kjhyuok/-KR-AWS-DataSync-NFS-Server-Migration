# AWS datasync로 NFS 서버를 AWS S3로 마이그레이션

스토리지 시스템과 서비스 간의 데이터 이동을 단순화, 자동화 및 가속화하는 온라인 데이터 전송 서비스인 AWS datasync를 사용하여 온프레미스에 위치한 NFS 서버를 AWS S3로 마이그레이션 하고, 온프레미스에 있는 Application 서버가 사용하던 NFS를 AWS File Gateway를 통해 S3로 대체해 보는 실습입니다.
기존 AWS의 Workshop인 [**AWS DataSync: NFS Server Migration**]([https://catalog.us-east-1.prod.workshops.aws/workshops/976050cc-0606-4b23-b49f-ca7b8ac4b153/en-US/](https://catalog.workshops.aws/datasync-nfs-server-migration-with-storage-gateway/en-US))을 한글화 하고, 실습자에게 필요한 부분만 구성합니다.<br>
본 Workshop은 약 1~1.5시간 정도 소요되며, 활용되는 AWS Resource 목록은 다음과 같습니다.

* [AWS Datasync](https://aws.amazon.com/ko/datasync/), [Amazon S3](https://aws.amazon.com/ko/s3/), [AWS Filegateway](https://aws.amazon.com/ko/storagegateway/file/) 그리고 코드기반으로 실습환경을 자동으로 프로비저닝해 줄 [AWS Formation](https://aws.amazon.com/ko/cloudformation/)이 활용됩니다.

워크샵 시나리오: AWS DataSync 및 AWS Storage Gateway를 사용한 NFS 서버 마이그레이션
데이터 센터에 오래된 NFS 서버가 있습니다. 서버에 있는 대부분의 데이터는 몇 년 된 것이며 가끔 읽기 위해서만 액세스됩니다. 서버에 새 파일이 기록되고 있지만 자주 기록되지는 않습니다. 데이터 센터 설치 공간을 줄이고 리소스를 확보하려면 NFS 서버의 데이터를 클라우드로 이동해야 합니다. 그러나 NFS 데이터에 액세스하는 애플리케이션 서버는 사용자의 대기 시간을 최소화하기 위해 사내에 있어야 하는 애플리케이션 서버를 아직 이동할 수 없습니다.
조사 결과 AWS DataSync를 사용하여 사내 NFS 서버에서 Amazon S3으로 데이터를 마이그레이션하고 AWS 스토리지 Gateway를 사용하여 S3에 있는 데이터에 대한 내부 NFS 액세스를 제공할 수 있다는 사실을 알게 되었습니다.
이 워크샵에서는 Cloud Formation 템플릿을 사용하여 리소스를 배포하고 AWS 관리 콘솔을 사용하여 리소스를 적절하게 구성하는 방법을 설명합니다. 아래 아키텍처 다이어그램에 표시된 것처럼 NFS 서버, 애플리케이션 서버, DataSync 에이전트 및 File Gateway 어플라이언스가 AWS 영역에 구축되어 사내 환경을 시뮬레이션합니다. NFS 서버의 데이터가 마이그레이션될 AWS 클라우드 영역을 시뮬레이션하는 S3 버킷이 AWS 영역에 생성됩니다.
