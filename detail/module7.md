


### Workshop Cleanup

## S3 자원 정리하기: 객체(여러 버전 포함) 삭제
객체 및 S3 버킷은 API 및 콘솔을 통해 프로그래밍 방식으로 삭제할 수 있습니다. 본 실습에서 업로드한 객체와 생성한 버킷이 더 이상 필요하지 않은 경우, 비용이 발생하지 않도록 삭제해야 합니다.

S3버킷 페이지로 가서 모든 파일을 하나씩 삭제할 수 있지만 본 가이드에서는 한 번에 삭제할 수 있는 방법을 설명합니다.

## Empty bucket 기능을 활용하여 모든 객체 삭제하기
버킷에 있는 모든 객체를 삭제하고 싶을 경우, S3 콘솔에서 Empty 옵션을 사용할 수 있습니다.

1. S3 콘솔에서 본 실습에서 생성한 버킷의 **라디오 버튼** 을 선택한 후, **Empty** 버튼을 클릭합니다.

![7-1](/images/7-1.png)

2. "Empty bucket" 페이지로 이동한 후, permanently delete 를 입력합니다. 그 후, **Empty** 버튼을 클릭하여 모든 객체를 영구적으로 삭제합니다.

![7-2](/images/7-2.png)

3. "Empty bucket:status" 페이지로 이동된 후, 모든 버킷의 객체가 삭제되었다는 메세지를 확인할 수 있습니다. **Exit** 을 클릭한 후, S3 콘솔로 다시 돌아갑니다.

![7-4](/images/7-4.png)

여기까지 S3 버킷안에 있는 모든 객체를 삭제했습니다.

## Storage Gateway 삭제하기
   1. 다음 명령을 실행하여 Application 서버에서 Storage Gateway NFS share를 마운트 해제합니다.
   
    
    sudo umount /mnt/fgw
    

   2. CLI를 실행하는 브라우저 창을 닫습니다.
   3. **Storage Gateway** 페이지로 이동하여 Storage Gateway **NFS file share**를 삭제합니다.
   4. *DataMigrationGateway*라는 이름의 **Storage Gateway**를 삭제합니다. 게이트웨이 EC2 인스턴스는 삭제되지 않습니다. 마지막 단계에서 환경을 만들때 자동생성한 CloudFormation stack이 삭제되면 인스턴스가 자동으로 삭제됩니다.

## CloudFormation을 통해 전체 리소스 삭제
이제 S3버킷을 포함한 Module2~5에서 활용한 모든 리소스는 Module1에서 자동 생성한 방식처럼 다시 해당 CloudFormation stack을 삭제하는 과정을 통해 정리하게 됩니다.
(꼭 Cloud Formation이 resource 삭제를 완료할 때까지 기다린 후 다음 단계로 이동할 필요는 없습니다!)

   1. **CloudFormation** 페이지로 이동하여 *DataMigrationWorkshop-inCloudResources*이라는 stack을 삭제합니다.
   2. **CloudFormation** 페이지로 이동하여 *DataMigrationWorkshop-onPremResources*이라는 stack을 삭제합니다.
   3. 다음과 같이 **Events** 탭에서 실시간으로 자원이 삭제되고 있는 것을 확인할 수 있습니다.
   
![7-5](/images/7-5.png)
   
모든 CloudFormation 템플릿이 올바르게 삭제되었는지 확인하려면 이 워크샵에서 생성된 모든 EC2 인스턴스가 **terminated state**인지 확인하십시오.

## 수고하셨습니다. 모든 실습을 완료 하셨습니다!👏👏👏
