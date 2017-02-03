---
layout:     post
title:      "Chaincode - RestAPI"
subtitle:   "chaincode_example02"
date:       2017-02-03
author:     "Yns"
header-img: "img/post-bg-2015.jpg"
tags:
    - Hyherledger
    - Fabric
    - Blockchain
    - Chaincode
    - Postman
---


# Hyperledger

### 선행

>1. Go 실습 -> [click](http://golang.site/go/article/1-Go-프로그래밍-언어-소개)  
**'Gopath'가 무엇인지는 무조건 알고 넘어가기**
>  2. Docker 실습 -> [click](http://www.slideshare.net/pyrasis/docker-fordummies-44424016)  
**program : process** vs **image : container** 



## 1. 'Docker'에서 'Hyperledger' 실행 

>실습 원문 사이트 -> [click](http://hyperledger-fabric.readthedocs.io/en/v0.6/Setup/Chaincode-setup/#writing-building-and-running-chaincode-in-a-development-environment)


#### (1) 도커 이미지 'hyper ledger image'  를 설치

	docker pull hyperledger/fabric-peer:latest
	docker pull hyperledger/fabric-membersrvc:latest
		  
#### (2) 편한 위치에 'blockchain' 폴더를 생성

#### (3) 폴더 안에 'docker-compose.yml' 파일 생성 후 아래 내용을 저장  

	membersrvc:
	  image: hyperledger/fabric-membersrvc
	  ports:
		- "7054:7054"
	  command: membersrvc
	vp0:
	  image: hyperledger/fabric-peer
	  ports:
		- "7050:7050"
		- "7051:7051"
		- "7053:7053"
	  environment:
		- CORE_PEER_ADDRESSAUTODETECT=true
		- CORE_VM_ENDPOINT=unix:///var/run/docker.sock
		- CORE_LOGGING_LEVEL=INFO
		- CORE_PEER_ID=vp0
		- CORE_PEER_PKI_ECA_PADDR=membersrvc:7054
		- CORE_PEER_PKI_TCA_PADDR=membersrvc:7054
		- CORE_PEER_PKI_TLSCA_PADDR=membersrvc:7054
		- CORE_SECURITY_ENABLED=true
		- CORE_SECURITY_ENROLLID=test_vp0
		- CORE_SECURITY_ENROLLSECRET=MwYpmSRjupbT
	  links:
		- membersrvc
	  command: sh -c "sleep 5; peer node start --peer-chaincodedev"


#### (4) 터미널에서 'docker-compose up' 입력 후 아래처럼 실행되면 성공  

![](https://s6.postimg.org/d3m4mraep/compose_up.png)


## 2. 'ChainCode' 실행 (Go lang)
> - '$GOPATH' 관련 내용은 'Go 실습에 나옴.
> - Window에서는 'git'을 따로 설치해야할 수 있음.
> - Window에서는 명령어가 조금 다를 수 있음.
	 
#### (1) download the sample chaincode  
>'curl'에서 에러 생기면 [click](http://tyboss.tistory.com/entry/Windows-윈도우에서-curl-설치-및-사용법)  

The chaincode project must be placed somewhere under the **src** directory in your local **$GOPATH** as shown below.
	
	mkdir -p $GOPATH/src/github.com/chaincode_example02/
	cd $GOPATH/src/github.com/chaincode_example02
	curl --request GET https://raw.githubusercontent.com/hyperledger/fabric/master/examples/chaincode/go/chaincode_example02/chaincode_example02.go > chaincode_example02.go
  
#### (2) clone the Hyperledger fabric to your local $GOPATH
	mkdir -p $GOPATH/src/github.com/hyperledger
	cd $GOPATH/src/github.com/hyperledger
	git clone http://gerrit.hyperledger.org/r/fabric
	  

#### (3) build your chaincode.
	cd $GOPATH/src/github.com/chaincode_example02
	go build
	
#### (4) 터미널에서 아래 커맨드를 입력
	CORE_CHAINCODE_ID_NAME=mycc CORE_PEER_ADDRESS=0.0.0.0:7051 ./chaincode_example02
	
#### (5) 맨 아래 'Received REGISTERED'가 나오면 성공

![](https://s6.postimg.org/idr3e1unl/chain_code.png)


## 3. REST API를 이용해서 hyper ledger 시스템에 접근

#### (1) 'Chrome'에서 'Postman' 앱을 설치 -> [click](https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop?utm_source=chrome-ntp-icon)
![](https://s6.postimg.org/k4a4fjc6p/postman.png)
#### (2) REST API 요청하기
  - 'GET'을 'POST'로 변경
  - url 추가

		http://127.0.0.1:7050/registrar
  - Body 클릭
  - raw format 클릭 및 입력

		{
		  "enrollId": "jim",
		  "enrollSecret": "6avZQLwcUe9b"
		}
- Return message 확인


		Login successful for user 'jim'.


#### (3) [여기](http://hyperledger-fabric.readthedocs.io/en/latest/Setup/Chaincode-setup/) 하단에 더 많은 요청 예제가 있으니 다 해보자.