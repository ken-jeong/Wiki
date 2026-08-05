---
aliases: []
type: Lecture
tags:
  - MLOps
draft: false
date: 2026-02-25
---
## 1. ML Engineer와 MLOps의 부상
- Amazon AWS
- Microsoft Azure
- Google GCP

## 2. MLOps란?

## 3. DevOps & MLOps
### 3.1. DevOps
- **DevOps**
	- 개발(Dev) + 운영(Ops) 협업 방식
	- 지속적 통합(CI), 지속적 배포(CD)
	- 장점: 빠른 배포 주기, 운영 안정성 강화

### 3.2. CI/CD
- 지속적 통합(**CI**; Continuous Integration)
	- 지속적으로 퀄리티 컨트롤을 적용하는 프로세스
	- 기존 통합 방식의 문제점인 통합의 지옥 해결
	- 자주 통합하여 코드 충돌 방지
	
	- **SaaS(Software as a Service) Build Server**
		- [[A4_GitHub Actions|GitHub Actions]]
		- Jenkins
		- GitLab
		- CircleCI
		- *Travis-CI*
	- **Cloud-Native Build System**
		- AWS Code Build

### 3.3. CD
- 지속적 배포(**CD**; Continuous Delivery)
	- 빌드, 테스트뿐만 아니라 배포까지 자동화
	- ==DevOps의 기반 플랫폼==
	
	- 예:
		- *ArgoCD*

### 3.4. Microservices
- **마이크로서비스**(Microservices)
	- 의존성이 거의 없고, 독립적인 기능을 가진 소프트웨어 서비스

- 예:
	- 머신러닝 추론 Endpoint
	- Python Flask 프레임워크
	- **FaaS**(Function as a Service): AWS 람다
	- **Caas**(Container as a Service)
		- AWS Fargate, Google Cloud Run, Azure App Services

### 3.5. Monitoring
- **모니터링**(Monitoring)
	- New Relic
	- DataDog
	- StackDriver

### 3.6. 효과적인 기술 소통
- **효과적인 기술 소통**
	- AutoML

## 4. MLOps 욕구 단계 이론
- DevOps
- 데이터 자동화
- 플랫폼 자동화
- MLOps

### 4.1. DevOps 구현
- 테스트 자동화
	- Scaffold
		- GitHub Repo
			- Makefile
			- requirements.txt
			- .py

- **스캐폴드**(Scaffold)
	- 비계(발판 구조물)
	- 프로젝트를 빠르게 시작하고 일관되게 확장할 수 있도록 기본 구조와 표준 설정을 자동으로 생성해주는 템플릿 또는 초기 골격

```
ml-project/  
├── data/  
├── models/  
├── src/  
├── tests/  
├── Dockerfile  
├── requirements.txt  
├── .github/workflows/ci.yml  
└── train.py
```

### 4.2. CI with GitHub Actions
- Python 스캐폴딩 프로젝트에 CI를 빠르게 적용

```yml
# .github/workflows/azure.yml
name: azure-mlops-train-ci

on:
  push:
    branches: ["main"]
    paths:
      - "src/**"
      - "azureml/**"
      - ".github/workflows/azure.yml"
  workflow_dispatch: {}

env:
  AZUREML_RG: ${{ secrets.AZUREML_RG }}
  AZUREML_WS: ${{ secrets.AZUREML_WS }}
  JOB_SPEC: azureml/job.yml

jobs:
  train_and_register:
    runs-on: ubuntu-latest
    
    steps:
      - name: Checkout
        uses: actions/checkout@v4
        
      - name: Azure Login
        uses: azure/login@v2
        with:
          creds: ${{ secrets.AZURE_CREDENTIALS }}
          
      # Azure ML CLI(v2) 설치
      - name: Install Azure ML CLI
        run: |
          az extension add -n ml -y
          az version
          az ml -h
          
      # 워크스페이스 세팅 확인
      - name: Set defaults
        run: |
          az configure --defaults group=$AZUREML_RG workspace=$AZUREML_WS
          
      # 학습 Job 제출
      - name: Submit training job
        id: submit
        run: |
          JOB_NAME=$(az ml job create -f $JOB_SPEC --query name -o tsv)
          echo "JOB_NAME=$JOB_NAME" >> $GITHUB_OUTPUT
          echo "Submitted job: $JOB_NAME"
          
      # Job 완료 대기
      - name: Wait job completion
        run: |
          az ml job stream -n ${{ steps.submit.outputs.JOB_NAME }}
          
      # (선택) 모델 등록: job outputs 경로를 모델로 등록
      # azureml/job.yml에서 outputs에 모델 아티팩트가 저장되도록 구성되어 있어야 함.
      - name: Register model
        run: |
          MODEL_NAME="my-model"
          MODEL_PATH="azureml://jobs/${{ steps.submit.outputs.JOB_NAME }}/outputs/artifacts/paths/model/"
          az ml model create --name $MODEL_NAME --path $MODEL_PATH --type custom_model
```

### 4.3. DataOps & Data Engineering
**DataOps**
- Apache AirFlow
- AWS Glue
- AWS Athena
- AWS QuickSight

### 4.4. 플랫폼 자동화
- AWS S3 Data Lake -> AWS 세이지메이커
- GCP -> Google AI 플랫폼
- Azure -> Azure ML Studio
- K8s -> Kubeflow

### 4.5. MLOps
- 모델 학습과 재학습
- -> 배포 + 버저닝
- -> 추적용 로그와 아티팩트
- -> 모니터링
- -> 모델 학습과 재학습

## 5. 마치며
- MLOps의 재료
	- Software Engineering
	- Data Engineering
	- Modeling
	- Business Problems
