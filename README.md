# AWS datasync로 NFS 서버를 AWS S3로 마이그레이션
스토리지 시스템과 서비스 간의 데이터 이동을 단순화, 자동화 및 가속화하는 온라인 데이터 전송 서비스인 AWS datasync를 사용하여 온프레미스에 위치한 NFS 서버를 AWS S3로 마이그레이션 하고, 온프레미스에 있는 Application 서버가 사용하던 NFS를 AWS File Gateway를 통해 S3로 대체해 보는 실습입니다.
기존 AWS의 Workshop인 [**AWS DataSync: NFS Server Migration**]([https://catalog.us-east-1.prod.workshops.aws/workshops/976050cc-0606-4b23-b49f-ca7b8ac4b153/en-US/](https://catalog.workshops.aws/datasync-nfs-server-migration-with-storage-gateway/en-US))을 한글화 하고, 실습자에게 필요한 부분만 구성합니다.<br>
본 Workshop은 약 1~1.5시간 정도 소요되며, 활용되는 AWS Resource 목록은 다음과 같습니다.

* ![AWS Datasync](https://aws.amazon.com/ko/datasync/), [Amazon S3](https://aws.amazon.com/ko/s3/), [AWS Filegateway](https://aws.amazon.com/ko/storagegateway/file/) 그리고 코드기반으로 실습환경을 자동으로 프로비저닝해 줄 [AWS Formation](https://aws.amazon.com/ko/cloudformation/)이 활용됩니다.
