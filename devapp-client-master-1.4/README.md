# Fabric Client Sample Application

## Requirement

### Hyperledger Fabric version
v1.0.6

### Browser / 브라우저
we tested this webapp with Chrome v65.0.3325.181 and Firefox v52.7.2.

이 응용 프로그램은 Chrome v65.0.3325.181와 Firefox v52.7.2에서 동작 확인하고 있습니다.

### Application modules and development /의존 모듈
see /package.json

/package.json 에 따라 모듈을 확인하시기 바랍니다.

## Install / 설치
`npm install` on application root
응용 프로그램의 루트 디렉토리에서`npm install`하십시오.

## Fabric setup
you need to set up your fabric Environment first.
and install and instantiate CarTransfer chaincode.
then setup application config for Peers, Orderers, CAs, MSP users and cert pathes info on /config/default.json .

앱 이용에 연결된 Fabric 환경을 설정해야합니다. 게다가 체인 코드 설치 및 인스턴스화를 수행하십시오. 연결된 응용 프로그램은 책에서 제공하는 cartransfer 체인 코드입니다. 또한 설정 파일 (/config/default.json)에 Peers, Orderers, CAs, MSP 사용자와 인증서의 경로를 설정합니다.

```
"asset": {
  //admin user for channel setup
  "adminUser":"admin",
  "users":{
    //all enrollments on MSP
    "admin":{
       "secret":"adminpw",
       "org":"org1"
    }
  },
  //your key-val store path for ECerts
  "keyValueStore":"./fabric-client-kvs",
  //your TLS certs Root directory
  "cert_dir": "/home/vagrant/git/fabric-samples/first-network/",
  //channel setting
  "channel":{
    "name": "mychannel"
  },
  //chaincode setting
  "chaincode":{
    "id" : "mycc",
    "path" : "github.com/hyperledger/fabric/examples/chaincode/go/chaincode_example02",
    "endorsement" : {
      "identities": [
        { "role": { "name": "member", "mspId": "org1" }},
        { "role": { "name": "member", "mspId": "org2" }}
      ],
      "policy": {
        "2-of": [{ "signed-by": 0 }, { "signed-by": 1 }]
      }
    }
  },
  //orderer setting
  "orderer": {
    "url": "grpcs://localhost:7050",
    "server-hostname": "orderer.example.com",
    "tls_cacerts": "crypto-config/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem"
  },
  //organization and peer/CA setting
  "org1": {
    "name": "Org1",
    "mspid": "Org1MSP",
    "ca": {
      "url": "https://localhost:7054",
      "name": "ca-org1"
    },
    "peer1": {
      "events": "grpcs://localhost:7053",
      "requests": "grpcs://localhost:7051",
      "server-hostname": "peer0.org1.example.com",
      "tls_cacerts": "crypto-config/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/msp/tlscacerts/tlsca.org1.example.com-cert.pem"
    }
  },
  //then your other orgs..
```

## Register MSP Users / MSP 에 등록
`node scripts/registerUser.js` on application root.

응용 프로그램의 루트 디렉토리에서`node scripts / registerUser.js`하십시오.

It'll be error if user is already registered.

그러나 사용자가 등록 된 경우 오류가 발생하지만 문제 없습니다.

## Deploy (Install/Instantiate/Update) Chaincode 
`node scripts/deploy.js` on application root.
응용 프로그램의 루트 디렉토리에서`node scripts / deploy.js`하십시오.

you need to set up /config/default.json asset.chaincode property and consts on deploy.js for your installation target org and chaincode.

배치에 있어서는 /config/default.json의 asset.chaincode 속성의 설정과 scripts / deploy.js의 consts 값 설정에서 배포 대상 체인 코드와 Organization 등을 지정합니다.

## Add Owners for CarTransfer chaincode 
`node scripts/addOwners.js` on application root.
응용 프로그램의 루트 디렉토리에서`node scripts / addOwners.js`하십시오.

you need to add owners after deployment of CarTransfer chaincode.and this script add all MSP users as Owner.
It'll be error if owner is already registered.

CarTransfer 체인 코드의 이용은 이쪽을 사용하고 소유자 등록 해 둘 필요가 있습니다. 이 스크립트는 register에 등록 된 MSP 사용자를 소유자로 등록합니다. 이미 소유자가 등록 된 경우 오류가 발생하지만 문제 없습니다.
또한 배포 직후의 요청 오류를 일으킬 수 있습니다. 다시 실행하면 정상적으로 등록 할 수 있습니다.

## Build Application / 응용 프로그램 빌드
`npm run build` on application root

응용 프로그램의 루트 디렉토리에서`npm run build`하십시오.
사전에 node.js와 npm 설치가 필요합니다.

## Run Application
`npm start`

you can access webapp with userid and password.
default setting is on /config/default.json users and password property.
(you should change the setting.)

응용 프로그램에 대한 액세스는 기본 인증이 걸려 있습니다.
/config/default.json의 users 속성 및 password 속성을 사용할 수 있습니다.
(기본 설정은 필요에 따라 변경합니다.)

## Test fabric client code
`npm run test:node`

you need to setup config/default.json and use chaincode_example02 for test.

테스트 실행에 chaincode_example02 설치 및 config / default.json의 환경 설정이 필요합니다.
