# Module 1 : 온프레미스 및 클라우드 내 지역에 리소스 배포

Module 1에서는 CloudFormation 스크립트를 사용하여 두 개의 AWS 리전에 리소스를 배포합니다.<br>
하나는 온프레미스 환경을 나타내고 다른 하나는 클라우드 내 환경을 나타내는데, 우선 모든 리소스가 배포되면 애플리케이션 서버의 NFS 서버에서 내보내기를 마운트하고 기존 파일을 확인합니다.
이 모듈에서는 CloudFormation 스크립트를 사용하여 2개의 AWS 리전에서 리소스를 배포합니다: 온프레미스 환경을 나타내는 하나와 클라우드 내 환경을 나타내는 하나입니다. 
모든 리소스가 배포된 후, NFS 서버에서 Application 서버로 NFS 내보내기를 마운트하고 기존 파일을 확인합니다.
[1-1](./images/1-1.png)

### 2. Perform Data Transformations
![1-2](https://user-images.githubusercontent.com/105655711/191056398-6f3585e1-325b-4ed6-a0ca-627142083d29.png)

### 3. Explore the DataLake using SQL and Visualization tools
![1-3](https://user-images.githubusercontent.com/105655711/191056419-a07c703a-7206-4d4b-92cd-5da4a5a592a5.png)

<!--### 4. Finally you will apply Machine Learning
![1-4](https://user-images.githubusercontent.com/105655711/191056429-6afcd88d-5702-44b0-b067-88a44c6701e1.png)
-->
오늘 실습은 위와 같은 각 과정에 대해 순차적으로 진행될 예정 입니다.<br>
이제 [1.Lab: Setting Lab Account](../detail/1.Lab:SettingLabAccount.md)에서 본 실습을 위한 AWS Event Account를 생성해 보겠습니다.
