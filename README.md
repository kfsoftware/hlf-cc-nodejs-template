## HLF node chaincode template

### Start tunnel
```bash
ngrok tcp 9999
```

### Install, approve and commit chaincode
```bash
CHAINCODE_NAME=template_node
API_URL="http://localhost:8080/graphql"
SIGNATURE_POLICY="OR('Org1MSP.member', 'Org2MSP.member')"
CHAINCODE_ADDRESS_TUNNEL="4.tcp.eu.ngrok.io:13438" # the forwarding URL you get from opening an ngrok tunnel
# localChaincode port must be the same as the port you use to connect for ngrok
hlf-cc-dev start --localChaincode="localhost:9999" \
  --chaincodeAddress="${CHAINCODE_ADDRESS_TUNNEL}" \
  --chaincode="${CHAINCODE_NAME}" \
  --signaturePolicy="${SIGNATURE_POLICY}" \
  --env-file="${PWD}/.env" \
  --apiUrl="${API_URL}"
```

### Start chaincode

```bash
npm i # install dependencies
npm run build:w # in another terminal
npm run start:dev
```

You can also use any IDE like vscode or WebStorm to debug the chaincode.

Every time you start the tunnel, you must restart the chaincode as the certificates and package id will change.

### Test chaincode

You can go to the playground(instead of /graphql, use /playground) and use the following mutation and query to test the chaincode:

```graphql
mutation invoke {
  invokeChaincode(input:{
    chaincodeName:"template_node"
    function: "InitLedger"
    args: []
    transientMap: []
  }){
    response
    transactionID
    chaincodeStatus
  }
}

mutation query {
  queryChaincode(input:{
    chaincodeName:"template_node"
    function: "GetAllAssets"
    args: []
    transientMap: []
  }){
    response
    chaincodeStatus
  }
}

```
