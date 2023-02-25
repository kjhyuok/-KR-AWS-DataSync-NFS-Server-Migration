# Module 3 : On-premises의 Application은 Storage Gateway를 사용해서 AWS S3로 액세스 해볼까요?

이제 NFS 서버에서의 파일이 S3 버킷으로 복사되었습니다. 이 모듈에서는 CloudFormation에 의해 배포된 File Gateway를 구성하여 온프레미스 지역에서 S3 버킷과 연결하고 [**Storage Gateway**](https://aws.amazon.com/ko/storagegateway/) NFS share를 통해 S3 버킷의 파일에 접근할 수 있도록 액세스를 제공할 예정 입니다.\
Application 서버에서 **Storage Gateway** share를 마운트하여 파일에 대한 액세스를 확인해 봅시다.

![3-1](<../images/4-1 (4).png>)

### Module Steps

👉🏻_Storage 모든 실습을 us-east-1: US East(N. Virginia)에서 진행합니다.

1.  **Activate the Storage Gateway**\
    바로 전 Module2에서 DataSync agent를 활성화한 것처럼 Storage Gateway에 대해 비슷한 단계를 수행하여 활성화해야 합니다. 아래 단계를 따라서 진행합니다.

    1. AWS Management 콘솔 페이지로 이동하고 **Services**를 클릭한 다음 **Storage Gateway**를 선택합니다.
    2. Gateway 생성을 위해 **Create gateway** 버튼을 클릭합니다.(기존 생성된 Gateway가 있다면, **Get started**)
    3. Gateway 이름을 "*DataMigrationGateway*"로 지정하고 **Amazon S3 File Gateway** 유형을 선택합니다.
    4. 호스트 플랫폼으로 **Amazon EC2**를 선택하고 게이트웨이 설정 확인을 선택한 후 **Next**를 클릭합니다.
    5. **Launch EC2 instance** 항목을 Customize your setting으로 지정 후 **Confirm set up gateway**에 체크하고 Next 클릭 합니다.

    ![3-1-1](../images/3-1-1.png)

    ![3-1-2](../images/3-1-2.png)

    6. 앞서 Module1과정에서 CloudFormation을 사용해서 생성된 File Gateway instance(EC2명: fileGateway)의 **fileGatewayPublicIP**를 입력하고,
    7. **Publicly accessible** endpoint 유형을 선택한 뒤 **Next**를 클릭합니다.

    ![3-1-3](../images/3-1-3.png)

    9. Gateway setting을 검토하고 **Next**를 클릭합니다.
    10. Gateway가 활성화된 다음 로컬 디스크 장치를 준비하는 데 1분 정도 소요됩니다. **300GiB /dev/sdc** 장치를 **Cache**에 할당합니다. 자주 액세스하는 파일을 캐시하는 데 사용할 Gateway의 로컬 디스크입니다.
    11. **CloudWatch log group** 패널에서 *Deactivate logging*를 선택하고 **Configure**을 클릭합니다.
    12. 기본 Storage Gateway 페이지에 *DataMigrationGateway*라는 이름의 Gateway가 표시됩니다.

    ![3-2](../images/3-2.png)
2.  **Create a Storage Gateway NFS share**

    1. Storage Gateway의 좌측 메뉴에서 **File shares**를 선택하고, **Create File share** 버튼을 클릭합니다.
    2. 방금 생성된 *DataMigrationGateway* 를 선택합니다.
    3. *Access objects using* 설정에서 **NFS** 선택합니다.
    4. **Amazon S3 bucket name**에 DataSync가 파일을 복사한 S3 버킷의 이름(*data-migration-workshop-xxxx*)을 입력합니다. 버킷 이름은 CloudFormation 스택 *Outputs*에서 확인할 수 있습니다.
    5. **Customize configuration** 을 클릭합니다.

    ![3-2-1](../images/3-2-1.png)

    6. **Automated cache refresh from S3**을 None으로 설정하고 **Next** 클릭 합니다.
    7. **S3 prefix name**은 비워 둡니다.
    8. Click **Next**.

    ![3-2-2](../images/3-2-3.png)

    9. default S3 스토리지 설정을 유지한 후 **Next**를 클릭합니다.
    10. **Access object** 섹션에서 **Add client**를 클릭하고, Application 서버의 **Private IP Address**를 추가한 후 "/32"를 추가합니다. 이렇게 하면 Application 서버만 게이트웨이의 NFS 파일 공유에 액세스할 수 있습니다.

    ![3-3](../images/3-3.png)

    11. **Mount options** 섹션에서 **Squash level**을 "_No root squash_"으로 변경합니다.
    12. Click **Next**.
    13. file share 설정을 검토하고 **Create**를 클릭합니다.
    14. share-xx를 선택하고 mount 지침을 확인합니다.

    ![3-4-1](../images/3-4-1.png)
3.  **Application 서버에 NFS share Mount**

    1. Application 서버에 대한 CLI로 돌아가서 아래 명령을 실행하여 Storage Gateway 공유에 대한 새 Mount 지점을 생성합니다.

    ```
    sudo mkdir /mnt/fgw
    ```

    2. 전 단계에서 생성한 Storage Gateway > File Shares에서 하단의 _Linux Example Commands_를 Copy하여 "\[MountPath]"를 "/mnt/fgw"로 바꿉니다. **sudo로 명령을 실행해야 합니다.**
    3. 이제 애플리케이션 서버에 두 개의 NFS 마운트 지점이 있어야 합니다. 하나는 온프레미스 NFS 서버용(/mnt/data에 mounted)과 Storage Gateway용(/mnt/fgw에 mounted)입니다.

    ```
    mount | grep nfs4
    ```

    ![3-5-1](../images/3-5-1.png)

### Validation Step

다음 명령을 실행하여 2 NFS shares에 동일한 파일 집합이 있는지 확인합니다.

```
diff -qr /mnt/data /mnt/fgw
```

/mnt/fgw: .aws-datasync-metadata에 하나의 추가 파일만 표시되어야 합니다.\
이 파일은 작업이 실행될 때 S3 버킷의 DataSync에 의해 생성되었는데 다른 모든 파일은 동일하며 데이터가 오류 없이 DataSync에 의해 완전히 전송되었음을 나타냅니다.\
다음 명령을 실행하여 Application 서버가 Storage gateway와 잘 마운트 되고 S3에 저장된 파일들이 보이는지 확인해 보시기 바랍니다.

```
ls /mnt/fgw/images
```

### Module3 Summary

이 모듈에서는 성공적으로 Storage Gateway를 활성화하고 NFS서버의 File share를 생성했습니다.\
그런 다음 Application 서버에 File share를 마운트하고 On-premises NFS 서버의 파일이 S3 버킷에 올바르게 복사되었는지 확인 했습니다.\
\
이 실습의 궁극적인 목표는 On-premises NFS 서버를 종료하고나서 AWS내 스토리지 리소스로 저장소를 변경하는 것인데, 일반적으로 마이그레이션이 발생하는 동안 또는 그 직 후에 생성되는 새로운 파일이 있으므로 최종 컷오버 전에 또 다른 증분 파일에 대한 S3로의 복사가 필요합니다.\
\
따라서 다음 모듈에서는 Storage Gateway share로의 최종 컷오버 전에 증분 복사를 한 번 더 수행합니다.

[Module4](module4.md)로 GoGo!👏
