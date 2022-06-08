# Jira 설치

## Jira 설치

> 알림 테스트를 위한 Jira 설치 과정을 기록

[Download Jira Software Data Center | Atlassian](https://www.atlassian.com/software/jira/download/data-center)

## JIRA 설치 미디어 권한 설정

설치 받은 미디어의 권한을 설정합니다.

```jsx
chmod a+x atlassian-jira-software-X.X.X-x64.bin
```

## JIRA 설치 미디어 실행

설치를 위해 해당 미디어를 실행합니다.

```jsx
sudo ./atlassian-jira-software-X.X.X-x64.bin
```

## JIRA 설치

1. JIRA 설치는 다음과 같이 진행됩니다. OK를 선택합니다.

```jsx
`This will install JIRA Software 8.x.x on your computer`

`OK [o, Enter], Cancel [c]`
```

1. 설치 경로, Port 등을 바꾸기 위해서 2번 Custom을 선택합니다.

```jsx
`Please choose one of the following:`

`Express Install 1, Custom Install 2, Upgrade an existing JIRA installation 3`
```

### Default Install

1. default 정보가 출력됩니다. i를 입력하여 JIRA 설치를 진행합니다.

```jsx
'Details on where Jira Software will be installed and the settings that will be used.'
'Installation Directory: /opt/atlassian/jira'
'Home Directory: /var/atlassian/application-data/jira'
'HTTP Port: 8080'
'RMI Port: 8005'
'Install as service: Yes'
'Install [i, Enter], Exit [e]'
```

1. 설치가 완료되고 JIRA를 시작하기 위해 y를 선택합니다.

```jsx
`Start JIRA Software 8.x.x now?`

`Yes y, No n`
```

### Custom Install

1. 다음은 위의 2번 절차에서 Custom Install을 선택할 경우의 예시를 보여줍니다. JIRA 설치 경로를 선택합니다. 기본을 사용할 경우 Enter를 입력합니다.

```jsx
'Select the folder where you would like Jira Software to be installed.'
'Where should Jira Software be installed?'
'[/opt/atlassian/jira]'
```

1. JIRA 데이터를 설치할 경로를 선택합니다. 기본을 사용할 경우 Enter를 입력합니다. Port 설정입니다. 만약 Port를 변경하기를 희망하면 2번을 입력 후에 원하는 Port를 설정해줍니다.

```jsx
'Configure which ports Jira Software will use.'
'Jira requires two TCP ports that are not being used by any other'
'applications on this machine. The HTTP port is where you will access Jira'
'through your browser. The Control port is used to startup and shutdown Jira.'
'Use default ports (HTTP: 8080, Control: 8005) - Recommended [1, Enter], Set custom value for HTTP and Control ports [2]'
```

1. 희망하는 Port 입력

```jsx
'HTTP Port Number'
'[8080]'
18080
'Control Port Number'
'[8005]'
18005
```

1. JIRA를 서비스에 등록하기를 원하면 Yes를 선택해 줍니다.

```jsx
`Install JIRA as Service?`

`Yes, No`
```

1. i를 입력하여 JIRA 설치를 진행합니다.

```jsx
Install i, Exit e
```

1. 설치가 완료되고 JIRA를 시작하기 위해 y를 선택합니다.

```jsx
'Installation of Jira Software 8.22.3 is complete'
'Start Jira Software 8.22.3 now?'

`Yes y, No n`
```

## **Jira 초기 설정 및 라이선스 활성화**

1. 직접 설정

![](<../.gitbook/assets/image (6).png>)

2\. 내장 DB사용

![](<../.gitbook/assets/image (20).png>)

3\.&#x20;

![](<../.gitbook/assets/image (41).png>)

4\. 라이센스 키 입력

![](<../.gitbook/assets/image (12).png>)

5\. 이메일 알림 설정

![](<../.gitbook/assets/image (46).png>)
