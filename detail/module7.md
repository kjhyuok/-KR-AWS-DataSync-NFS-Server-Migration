


### Workshop Cleanup

이 워크샵 시나리오 이후에 모든 리소스가 삭제되었는지 확인하려면 아래에 설명된 순서에 따라 단계를 실행하세요.<br>
(꼭 Cloud Formation이 resource 삭제를 완료할 때까지 기다린 후 다음 단계로 이동할 필요는 없습니다!)

   1. 다음 명령을 실행하여 Application 서버에서 Storage Gateway NFS share를 마운트 해제합니다.
   
    
    sudo umount /mnt/fgw
    

   2. CLI를 실행하는 브라우저 창을 닫습니다.
   3. IN-CLOUD 리전의 **Storage Gateway** 페이지로 이동하여 Storage Gateway **NFS file share**를 삭제합니다.
   4. IN-CLOUD 리전의 *DataMigrationGateway*라는 이름의 **Storage Gateway**를 삭제합니다. 게이트웨이 EC2 인스턴스는 삭제되지 않습니다. On-premises 리전의 CloudFormation 이 삭제되면 인스턴스가 삭제됩니다.
   5. IN-CLOUD 리전에서 **data-migration-workshop** S3 버킷의 모든 개체를 삭제합니다. S3 버킷은 다음 단계에서 CloudFormation에 의해 삭제되기 전에 *empty* 상태로 있어야 합니다.
   6. IN-CLOUD 리전의 **CloudFormation** 페이지로 이동하여 *DataMigrationWorkshop-inCloudResources*이라는 stack을 삭제합니다.
   7. On-premises 리전의 **CloudFormation** 페이지로 이동하여 *DataMigrationWorkshop-onPremResources*이라는 stack을 삭제합니다.
    
모든 CloudFormation 템플릿이 올바르게 삭제되었는지 확인하려면 이 워크샵에서 생성된 모든 EC2 인스턴스가 On-premises 영역에서 **terminated state**인지 확인하십시오.
        
수고하셨습니다. 모든 실습을 완료 하셨습니다!👏👏👏
