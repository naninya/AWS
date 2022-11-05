# IAM & AWS CLI
## IAM (Identity and Access Management)
IAM에서는 사용자를 생성하고, 그룹에 배치하기 때문에 글로벌 서비스에 해당됩니다. 맨 처음 계정을 생성할 때 루트 계정이 기본으로 생성되는데, 그 후에는 루트 계정을 더 이상 사용해서도, 공유해서도 안 됩니다. 그 대신 **새로운 사용자를 생성하는 것**이 좋습니다. 

여기서 사용자와 그룹을 생성하는 이유는 뭘까요? 루트 사용자는 계정에 대한 모든 권한을 가지고 있습니다. 어떤 작업이라도 가능하고, 모든 설정에 접근할 수 있습니다. 그렇기 때문에 위험한 계정이 될 수 있고, 보안 상의 이유로도 사용하지 않는 것이 좋습니다. 따라서 별도의 관리자 계정을 만드는 것이 좋습니다.

사용자나 그룹을 생성하면 각각 **권한을 부여해서 AWS 계정 사용을 허용할 수 있습니다.** 이를 위해 **사용자 또는 그룹에게 정책, 또는 IAM 정책이라고 불리는 JSON 문서를 지정할 수 있습니다.**

<img width="400" alt="aws" src="https://user-images.githubusercontent.com/87610758/199052285-919462bf-3e97-4ebd-97f3-42c94bdb95f5.png">


문서는 이런 형태입니다. 프로그래밍이 아니기 때문에 프로그래머가 아니어도 이해가 가능합니다. 간단한 영어로 **특정 사용자, 혹은 특정 그룹에 속한 모든 사용자들이 어떤 작업에 권한을 가지고 있는지를 설명해 놓은 문서**입니다.

이와 같은 형태의 JSON 문서를 통해 사용자들이 AWS 서비스를 이용하도록 허용할 수 있습니다. **이 정책들을 사용해 사용자들의 권한을 정의할 수 있게 됩니다.** AWS에서는 모든 사용자에게 모든 권한을 허용하지 않습니다. 모든 권한이 허용된다면, 새로운 사용자가 실행하는 서비스마다 큰 비용이 발생하거나, 보안 문제를 야기할 수도 있습니다. 따라서 AWS에서는 **최소 권한의 원칙을 적용합니다.** 즉 사용자가 꼭 필요로 하는 것 이상의 권한을 주지 않는 것입니다. 사용자가 세 개의 서비스에 접근해야 한다면 그에 대한 권한만 제한적으로 설정해야 합니다.

### IAM 콘솔 체험하기
---
IAM에서는 사용자와 그룹이 글로벌 관점에서 생성됩니다.

<img width="800" alt="aws" src="https://user-images.githubusercontent.com/87610758/199054065-c3ec1ce7-bc11-4c1d-b9a2-429cd3ea853c.png">


우리는 현재 IAM 대시보드에 있습니다. 처음으로 할 일은 IAM 사용자를 생성하는 것입니다. 사용자 항목으로 이동해 사용자 추가를 선택하겠습니다.

![image](https://user-images.githubusercontent.com/87610758/199054862-4829240e-094e-44d4-9b1d-ca8ab1cba201.png)

왜 사용자를 생성해야 할까요?
보시는 것처럼 상단의 계정 이름을 클릭해 보시면 현재 루트 계정을 사용 중입니다. 앞서 설명했던 것처럼 루트 사용자는 계정에 대한 모든 권한을 가지고 있기 때문에, 보안 상의 이유로 새로운 사용자에 사용하려고 하는 서비스에만 권한을 제한적으로 부여해서 서비스를 이용하는 것이 좋습니다.

![image](https://user-images.githubusercontent.com/87610758/199055158-32e6c18d-1821-48a1-9d52-d9453b45769d.png)

사용하고자 하는 사용자 이름과 암호를 설정합니다. 비밀번호 재설정 필요는 체크 해제해도 됩니다.

![image](https://user-images.githubusercontent.com/87610758/199055970-fc5a648a-4712-4915-aed7-3f6a25d80631.png)

이제 사용자를 그룹에 배치할 겁니다. 먼저 그룹을 만들건데, 그룹 이름은 admin으로 하겠습니다.

![image](https://user-images.githubusercontent.com/87610758/199056997-4d289158-0673-43da-b9da-a4cc6973b023.png)

admin 그룹에 배치된 사용자는 이 그룹에 부여된 권한을 내려받게 됩니다. 그리고 권한은 정책을 통해 정의됩니다. 여기서 admin 그룹에 연결할 정책은 AdministratorAccess입니다. 이 정책은 admin 그룹에 속한 모든 사용자가 계정의 관리자 역할을 하도록 허용합니다.

![image](https://user-images.githubusercontent.com/87610758/199057052-fca642d5-14ce-4524-bc41-71044d2af292.png)

다음: 태그 버튼을 누릅니다.

![image](https://user-images.githubusercontent.com/87610758/199057283-cb16a52b-da65-47aa-9f7e-b663dbb31e79.png)

태그는 사용자의 접근을 추적, 조직, 제어할 수 있도록 도와주는 정보입니다. 특정 사용자에 대해 단순히 정보를 추가하는 용도입니다. 
작성 후 다음: 검토 버튼을 누릅니다.

![image](https://user-images.githubusercontent.com/87610758/199057887-84b9b3af-4a4d-4128-b618-fb0c3da86bba.png)

우리가 생성했던 사용자 이름, 관리콘솔 접근 방법, 비밀번호 설정 등의 정보를 요약해서 볼 수 있습니다. 사용자 만들기 버튼을 눌러봅시다.

![image](https://user-images.githubusercontent.com/87610758/199058148-2e56092b-93cc-4f0c-a5e1-52888cccd624.png)

진행하기 전 먼저 .csv 파일을 다운로드해야 합니다. 이 Download.csv 파일 내부에는 사용자들의 자격 증명 정보가 있습니다. **내가 아닌 팀원이나 다른 사용자 계정을 생성한 경우**에는 해당 계정을 사용하려는 사람의 메일 주소로 로그인에 대한 설명을 전송할 수도 있습니다.

![image](https://user-images.githubusercontent.com/87610758/199059499-0e1dc6b2-f52e-462c-ae1c-d28753cad236.png)

이제 우리가 생성한 내용을 살펴봅시다.

![image](https://user-images.githubusercontent.com/87610758/199059860-2ed2e4a1-a0d0-4ff1-9f18-5d5a58760174.png)


Admin 그룹으로 들어가 보면 AdministratorAccess라는 정책이 연결된 것을 볼 수 있습니다. 이 정책은 그룹 내 사용자들에게 전체 관리자 권한을 제공합니다.

![image](https://user-images.githubusercontent.com/87610758/200108217-75e412fc-170b-4ebb-86f6-83e2459ee703.png)

화면 왼쪽의 사용자 메뉴에서도 사용자마다 정보 확인이 가능합니다.

![image](https://user-images.githubusercontent.com/87610758/200108330-cc3ef84b-3b73-46be-8def-96c604ffc37d.png)

지금까지 그룹과 사용자를 생성했는데, 이제 Jae라는 사용자로 로그인 해봅시다.

### 로그인하기 
---
먼저 대시보드로 돌아갑니다. 대시보드 오른쪽 패널을 보면 AWS 계정에 대한 요약 정보가 있습니다. 

![image](https://user-images.githubusercontent.com/87610758/200109542-8ed1f10b-4115-4455-a591-37916c6c6232.png)

사용자마다 계정 ID가 할당되어 있는데, 로그인 시 이 **계정 ID**로 로그인이 가능합니다. 하지만 계정 ID뿐만 아니라 **계정 별칭**을 설정해서 계정 별칭으로도 로그인이 가능합니다. 예를 들어, jae-aws-access 별칭을 입력하고 변경 사항 저장을 하면, 더 보기 쉽게 커스텀된 로그인 URL로 접근이 가능합니다.

![image](https://user-images.githubusercontent.com/87610758/200109564-5bcbfc9f-73f1-4b51-8960-d63dc968f206.png)

복사한 로그인 URL로 이동하면

![image](https://user-images.githubusercontent.com/87610758/200109646-bd714bbb-c9ef-402c-9334-31562d7c8c20.png)

다시 AWS 로그인 화면으로 이동이 됩니다.

![image](https://user-images.githubusercontent.com/87610758/200109666-b40157f4-1398-4097-94a5-948a6c18e5a9.png)

방금 생성한 IAM 사용자명과 비밀번호를 입력한 뒤 Sign In을 클릭합니다.

![image](https://user-images.githubusercontent.com/87610758/200109716-7140b5ba-e08c-4686-80bf-bb8ea0f49695.png)

그럼 IAM 사용자로서 콘솔에 로그인할 수 있습니다.

![image](https://user-images.githubusercontent.com/87610758/200109724-77fe4349-670c-4a9c-9616-534815481be3.png)

### IAM - Password Policy
---
사용자와 그룹을 생성했으니 해당 그룹과 사용자들의 정보를 보호할 수 있도록 보안 설정을 해야합니다. 이를 위한 두 가지 방어 메커니즘이 있습니다. 

#### 첫 번째, 비밀번호 정책 (Password Policy) 정의
비밀번호가 강력할수록 계정에 대한 보안성이 더 높아지겠죠? AWS에서는 다양한 옵션을 이용해 비밀번호 정책 생성이 가능합니다. 
- 비밀번호 최소 길이 설정
- 특정 유형 글자 사용 설정 (대-소문자, 숫자, 물음표 등 특수 문자)
- 비밀번호 만료 기간 설정 (e.g. 90일 뒤 비밀번호 재설정)
- 비밀번호 재사용 방지 (이전 비밀번호 사용 X)

위와 같은 비밀번호 정책을 이용하면 계정에 대한 치명적인 공격을 막아내는데 도움이 될 수 있습니다. 이제 비밀번호 정책을 어떻게 설정할 수 있는지 알아봅시다. 

대시보드의 왼쪽 계정 설정 메뉴에 들어가서
![image](https://user-images.githubusercontent.com/87610758/200116230-0a7ddbea-e76b-45e3-a5ea-79db4d77b0b1.png)

비밀번호 정책의 Change 버튼을 누르면
![image](https://user-images.githubusercontent.com/87610758/200116237-b31b8975-3aff-439a-a0f1-8fe94a791236.png)

해당 암호 정책 수정 페이지에서 비밀번호 정책을 설정할 수 있습니다. 비밀번호 정책을 더 많이 설정한다면, 계정의 보안을 더욱 강화시킬 수 있겠죠?
![image](https://user-images.githubusercontent.com/87610758/200116243-b90af5e7-dfb4-43c7-aeb8-cd4ac7752ce6.png)

#### 두 번째, 다요소 인증 (Multi Factor Authentication - MFA)
- MFA devices options in AWS
  - Virtual MFA device<br>
  ![image](https://user-images.githubusercontent.com/87610758/200116304-48595e58-8d67-411e-9569-3d6150ba95bd.png)
    - Google Authenticator (phone only)
    - Authy (multi-device)<br>
      컴퓨터와 휴대전화에서 같이 사용할 수 있고, 하나의 장치에서도 토큰을 여러개 지원합니다.
  - Universal 2nd Factor (U2F) Security Key<br>
  ![image](https://user-images.githubusercontent.com/87610758/200116346-679688db-1b42-4afa-bb7f-0b4221903be7.png)<br>
    범용 두 번째 인자, 혹은 U2F 보안 키라고 불리는 것도 있습니다. 이는 물리적 장치로 예를 들어 Yubico 사의 YubiKey가 있습니다. Yubico는 AWS의 제3자 회사로 AWS 제공 장치가 아니라 제3자 회사의 장치입니다 이렇게 물리적 장치를 사용하면 전자 열쇠에 달고 다닐 수 있으니 사용이 상당히 편리할 수 있겠죠 YubiKey는 하나의 보안 키에서 여러 루트 계정과 IAM 사용자를 지원하기 때문에 하나의 키로도 충분합니다.
  - Hardware Key Fob MFA Device<br>
    ![image](https://user-images.githubusercontent.com/87610758/200116385-2777a89f-da0d-459f-ac00-c65e80c7c17b.png)<br>
    다른 옵션으로 하드웨어 키 팝 MFA 장치도 있습니다. 이 역시 AWS의 제3자 회사인 Gemalto의 제품입니다. 
  
  - Hardware Key Fob MFA Device for AWS GovCloud (US)<br>
    ![image](https://user-images.githubusercontent.com/87610758/200116408-37baa561-9e76-4da3-b562-29a562310a52.png)<br>
    만약 미국 정부의 클라우드인 AWS GovCloud를 사용하는 경우라면, 사진에 보이는 특수한 키 팝이 필요한데요. 역시 SurePassID라는 제3자 회사가 제공하고 있습니다.
