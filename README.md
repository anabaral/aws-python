# aws-python 

## 가정

- Windows 환경, Python 3.8 

## 셋업

여기를 참조해서 진행: https://boto3.amazonaws.com/v1/documentation/api/latest/guide/quickstart.html

### boto3 설치

```
C:\Users\rindon>python -m pip install --upgrade pip
...
C:\Users\rindon>pip install boto3
...
```
(사실 위 설치 순서는 반대로 했음. 뭐 괜찮겠지)

### 접근 키 획득

AWS CLI 혹은 boto3 에서 접근할 때 권한을 얻기 위해 사용할 키를 얻자.
웹 콘솔의 나의 메뉴에서 '나의 보안 자격 증명 (혹은 Security-Credentials)' 항목을 선택  
![나의 메뉴](https://github.com/anabaral/aws-python/blob/main/img/aws-01.png)

'CLI, SDK 및 API 액세스를 위한 액세스 키' 라는 내용이 보일 것인데 [액세스 키 만들기] 버튼으로 만들면 됨.
아이디와 비밀키를 볼 수 있는데, 비밀키는 한 번만 보여주니까 잘 적어두자. 다만
- 비밀키를 잊어버리면 새로 만들고 지금까지 이용했던 PC나 어플리케이션을 변경해 주는 불편함이 생김.
- 비밀키를 나도 모르게 오픈해 버리면... 차라리 잊는 게 나을 정도의 돈 문제가 생길 것임.

### AWS CLI 설치

여기를 참조해서 진행: https://aws.amazon.com/ko/cli/

64비트 실행 프로그램을 다운받아 설치.

설치 후 인증/권한 위한 명령 실행

```
C:\Users\rindon>aws configure
AWS Access Key ID [None]: AKIAYUPXW4SKDIE9D01X                        <-- 적당히 바꾼 것이니 염려 마세요
AWS Secret Access Key [None]: gA+xBBs1XxxxYyyyyzzzAaaaa2ooo0pppppEaan <-- 적당히 바꾼 것이니 염려 마세요
Default region name [None]: ap-northeast-2                            <-- 서울
Default output format [None]: 그냥엔터                                 <-- json/yaml/text/table 선택가능
```

이 입력 결과는 홈디렉터리 밑의 ```.aws``` 밑에 파일로 저장됩니다.

음.. 그런데 이런 식이면 '누군가 다른 사람의 권한을 위임받아서' 동작할 수가 없을 수 있는데..

아무튼 테스트 한다 치고 몇 번의 시행착오를 거쳐 다음을 실행해 보았음:
```
ec2 = boto3.client('ec2)
for m in ec2.describe_instances()['Reservations']:
  for n in m['Instances']:
    print(n['InstanceId'] + " " + n['InstanceType'])
```

어디서 찾아 보니 다음 코드로 임시 권한을 가지고 수행이 가능하다고 함:
참조 https://boto3.amazonaws.com/v1/documentation/api/latest/guide/credentials.html
```
ec2 = boto3.client('ec2, aws_access_key_id='AKIAYUPXW4SKDIE9D01X', aws_secret_access_key='gA+xBBs1XxxxYyyyyzzzAaaaa2ooo0pppppEaan' )
for m in ec2.describe_instances()['Reservations']:
  for n in m['Instances']:
    print(n['InstanceId'] + " " + n['InstanceType'])
```
검색해 얻는 예제는 ```aws_session_token=SESSION_TOKEN``` 도 같이 쓰도록 하고 있지만 
이것은 Role을 한정시킨 세션을 생성해 실행할 때 쓰는 용법으로 쓰는 게 좋지만 굳이 쓰지 않아도 실행됨.









