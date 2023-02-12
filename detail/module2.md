# Module 2 : DataSync를 사용하여 S3에 초기 파일 복사

이 모듈에서는 온프레미스 지역에 배포된 DataSync Agent를 활성화하고 DataSync location를 생성한 다음 Source location에서 Destination location로 데이터를 복사하는 DataSync 작업을 생성합니다.<br><br>
DataSync 작업은 데이터 복사 작업을 수행하며 **Source & Destination** 두 location이 필요합니다. DataSync에서 **location**는 파일이 상주하거나 복사될 Endpoint입니다. 위치는 NFS 내보내기, SMB 공유, Amazon S3 버킷, HDFS, Windows용 FSx, Lustre용 FSx 또는 Amazon EFS 파일 시스템일 수 있습니다. location object는 task와 독립적이며 단일 location을 여러 task에 사용할 수 있습니다.

![2-1](../images/2-1.png)

### Module Steps (👉🏻*Storage 모든 실습을 us-east-1: US East(N. Virginia)에서 진행합니다.*)
***
1. 클라우드 배포영역에서 **Activate the DataSync agent**<br>
Agent Instance는 Module1에서 생성되었지만 사용하려면 클라우드 배포영역에서 활성화해야 합니다. Agent를 활성화하려면 아래 단계를 따르세요.<br>

    1. IN-CLOUD 리전의 AWS Management 콘솔 페이지로 이동하고 서비스를 클릭한 다음 **DataSync**를 선택합니다.
    2. DataSync 에이전트가 없으면 **Get started** 버튼을 클릭하고 그렇지 않으면 **Create agent** 버튼을 클릭합니다.
    3. DataSync 에이전트를 실행할 EC2 인스턴스는 이미 온프레미스 지역에 배포되었습니다.
    4. 서비스 엔드포인트를 "**Public service endpoints**"로 둡니다.
    5. 활성화 키 섹션 아래에 온프레미스 지역에서 실행 중인 DataSync agent 인스턴스의 Public IP 주소를 입력합니다. 온프레미스 지역의 **CloudFormation Outputs**에서 이 IP 주소를 가져올 수 있습니다. 활성화를 위해 웹 브라우저에서 agent에 액세스할 수 있어야 하므로 여기에서 **Public IP** 주소를 사용합니다. 아래와 같이 agent의 IP 주소를 입력하고 키 가져오기를 클릭합니다.
    ![2-2](../images/2-2.png)
    6. 활성화에 성공하면 활성화 키가 표시되고 추가 정보를 입력하라는 메시지가 표시됩니다.
    ![2-3](../images/2-3.png)
    7. 원하는 경우 agent 이름을 입력한 다음 **Create agent**를 클릭합니다.

2. Create NFS location<br>

    1. DataSync 서비스 페이지의 왼쪽에서 위치를 클릭한 다음 위치 생성을 클릭합니다.
    2. 온프레미스 NFS 서버의 위치를 생성합니다. 위치 유형 드롭다운에서 네트워크 파일 시스템(NFS)을 선택합니다.
    3. 에이전트 드롭다운에서 이전 단계에서 생성한 DataSync 에이전트를 선택합니다.
    4. 온프레미스 지역의 CloudFormation 출력에 따라 NFS 서버의 사설 IP 주소를 입력합니다. 이것은 이전 모듈에서 애플리케이션 서버에 NFS 내보내기를 마운트하는 데 사용된 것과 동일한 IP 주소였습니다. DataSync 에이전트가 NFS 내보내기를 마운트하는 데 사용할 IP 주소입니다.
    ![2-4](../images/2-4.png)
    6. Click Create location.

3. Create S3 location<br>
    1. DataSync 서비스 페이지의 왼쪽에서 위치를 클릭한 다음 위치 생성을 클릭합니다.
    2. S3 버킷의 위치를 생성합니다. 위치 유형 드롭다운에서 Amazon S3 버킷을 선택합니다.
    3. S3 버킷 드롭다운에서 data-migration-workshop으로 시작하고 뒤에 긴 GUID가 오는 S3 버킷을 선택합니다.
    4. S3 스토리지 클래스를 표준으로 유지하십시오.
    5. 폴더 아래에 "/"를 입력합니다. 이렇게 하면 모든 파일이 버킷의 최상위 수준으로 복사됩니다.
    6. IAM 역할에서 DataMigrationWorkshop-inCloud로 시작하는 S3 버킷 IAM 역할을 선택합니다. 역할의 전체 이름은 클라우드 내 CloudFormation 스택의 출력에서 찾을 수 있습니다.
    ![2-5](../images/2-5.png)
    7. 페이지 왼쪽에서 위치를 다시 클릭합니다. 이제 두 개의 위치가 나열되어야 합니다. 하나는 NFS 서버용이고 다른 하나는 S3 버킷용입니다.
    ![2-6](../images/2-6.png)

4. Create a task<br>
    1. DataSync 서비스 페이지의 왼쪽에서 작업을 클릭한 다음 작업 생성을 클릭합니다.
    2. 원본 위치 옵션에서 기존 위치 선택을 선택합니다.
    3. 기존 위치 드롭다운에서 이전에 생성한 NFS 서버 위치를 선택합니다.
    4. Click Next.
    ![2-7](../images/2-7.png)
    5. 대상 위치 옵션에서 기존 위치 선택을 선택합니다.
    6. 기존 위치 드롭다운에서 이전에 생성한 S3 버킷 위치를 선택합니다.
    7. Click Next.
    8. 데이터 확인 드롭다운에서 전송된 데이터만 확인을 선택합니다. 다른 모든 옵션은 그대로 유지하십시오.
    9. 작업 로깅에서 자동 생성을 선택하여 CloudWatch 로그 그룹 및 리소스 정책을 생성합니다.
    ![2-8](../images/2-8.png)
    10. 해당 작업 설정을 검토하고 작업 생성을 클릭합니다.

5. Run the task<br>
    1. **Task status**가 "*Available*"으로 보고될 때까지 기다립니다. (페이지를 refresh해야 할 수도 있습니다.)
    ![2-9](../images/2-9.png)
    2. 작업을 실행하려면 **Start**버튼을 클릭하고 **Start with defaults** 클릭합니다.
    3. 작업은 즉시 "*Running*" 상태로 전환됩니다.
    4. **History** 탭 아래 목록에서 task execution 개체를 클릭합니다.
    ![2-10](../images/2-10.png)
    5. 작업이 실행되면 실행 상태가 "*Launching*"에서 "*Preparing*", "*Transferring*", "*Verifying*", 마지막으로 "*Success*"로 진행됩니다. task execution은 아래와 같이 작업에 대한 통계를 보고합니다. 작업을 완료하는 데 몇 분 정도 걸립니다. 작업이 완료되면 202개의 파일이 전송되었음을 알 수 있습니다. 이것들은 우리가 지정한 path에 있는 2개의 폴더와 함께 데이터 세트에 있는 200개의 jpg 파일들 입니다.
    ![2-11](../images/2-11.png)
    ![2-12](../images/2-12.png)

### Validation Step
***
IN-CLOUD 지역 관리 콘솔에서 서비스를 선택한 다음 **S3**를 선택합니다. 버킷 목록에서 **data-migration-workshop** 버킷을 클릭합니다. 그 안에는 "*images*"라는 최상위 폴더가 표시되는데 이 폴더 안에는 NFS 서버의 .jpg 파일 200개가 있어야 합니다.
![2-13](../images/2-13.png)

### Module1 Summary
***
이 모듈에서는 DataSync agent를 성공적으로 활성화하고 온프레미스 NFS 서버에서 AWS의 S3 버킷으로 파일을 복사하는 작업을 생성하고 그 파일이 성공적으로 복사되었는지 확인했습니다.<br>
다음 [Module3](../detail/module3.md)에서는 온프레미스 파일 게이트웨이를 생성해 보고 S3 버킷에 연결하여 NFS를 통해 AWS 클라우드 내의 파일에 액세스할 수 있도록 구성할 것입니다.<br>

[Module3](../detail/module3.md)로 GoGo!👏

