---
layout:     post
title:      "Example Post with Code Highlight"
subtitle:   "CommonJS，RequireJS，SeaJS 归纳笔记"
date:       2015-05-25
author:     "Hux"
header-img: "img/post-bg-js-module.jpg"
tags:
    - 前端开发
    - JavaScript
---


# hyper ledger / fabric

#### fabric repo - [github](https://github.com/hyperledger/)
 
	git clone https://github.com/hyperledger/fabric  
    
 
#### interface.go
	$GOPATH/src/github.com/hyperledger/fabric/core/chaincode/shim/interfaces.go  

Chaincode interface must be implemented by all chaincodes. The fabric runs the transactions by calling these functions as specified.    

>반드시 구현해야 하는 함수들을 정의  
>'chaincode.go'에서 함수의 세부적인 내용 작성
  
 
#### chaincode.go    

	$GOPATH/src/github.com/hyperledger/fabric/core/chaincode/shim/chaincode.go	
  
 


