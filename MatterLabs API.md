<h1 align = center> Explorer API v2 </h1>

# Table of Contents

- [Table of Contents](#table-of-contents)
- [Get transactions for an address](#get-transactions-for-an-address)
  - [Query Parameters](#query-parameters)
  - [Sample Request](#sample-request)
  - [Sample Response](#sample-response)
  - [Response Parameters](#response-parameters)
  - [Error List](#error-list)

# Get transactions for an address

Call this method to fetch transactions with associated data under a specific address.

|                 |                        |
| :-------------: | ---------------------- |
| **Method Type** | GET                    |
|    **Path**     | /v2/users/transactions |

## Query Parameters

| Parameter  |  Type  | Description                                                              | Required |
| :--------: | :----: | ------------------------------------------------------------------------ | :------: |
|   module   | string | Module type; set to 'account' in this case                               |    Y     |
|  address   | string | Specify account address                                                  |    Y     |
| startblock |  int   | Block number to start the search from; `0` by default                    |    N     |
|  endblock  |  int   | Block number to end search at                                            |    N     |
|    page    |  int   | Set no. of pages to enable pagination; Leave empty to disable by default |    N     |
|   offset   |  int   | Specfiy no. of transactions per page for pagination                      |    N     |
|    sort    | string | Set sorting preference; `asc` for ascending, `desc` for descending       |    N     |

> **Note:** This endpoint returns a maximum of **_X_** records.

## Sample Request

Consider the following cURL request.

```shell
curl -X GET https://server:port/v2/users/transactions?module=account&address=0xc679HDJH9HHJI098a3CC98sjskjjks&startblock=0&endblock=100000&page=1&offset=5&sort=desc

```

You may also obtain the same transaction data by by invoking a `fetch` method. Here's an example.

```js
var url = new URL('https://server:port/v2/users/transactions?')
fetch(url + new URLSearchParams({ // Concatenating parameters to form string using URLSearchParams
    module : 'account',
    address : '0xc679HDJH9HHJI098a3CC98sjskjjks',
    startblock : 0,
    endblock : 100000,
    page : 1, // null to disable pagination
    offset : 5,
    sort : 'desc',
})) // Sending GET request, headers optional

.then(res => {
    return res.json()
})

.then(data => console.log(data)) // Printing response data

.catch(error => {
    switch (error) { // Catching errors by error codes
        case 200:
            console.log(error)
            console.log('Success')
            break
        case 400:
            console.log(error)
            console.log('Bad Request. Double check the path URL and the query parameter data')
            break
        case 401:
            console.log(error)
            console.log('Unauthorized API request. Must authenticate request with an API key first')
            break
        case 403:
            console.log(error)
            console.log('Forbidden. Request limit reached, or blocked by whitelist settings. Get in touch with us.')
            break
        case 429:
            console.log(error)
            console.log('Too many concurrent requests. Slow down.')
            break
        case 500:
            console.log(error)
            console.log('Internal Server Error. Get in touch with us.')；
            break
    }
});
```

## Sample Response

A typical success response would look like the one illustrated below.

```json
{
  "status": "1",
  "message": "OK",
  "result": [
    {
      "blockNumber": "14923678",
      "timeStamp": "1654646411",
      "hash": "0xc52783ad354aecc04c670047754f062e3d6d04e8f5b24774472651f9c3882c60",
      "nonce": "1",
      "blockHash": "0x7e1638fd2c6bdd05ffd83c1cf06c63e2f67d0f802084bef076d06bdcf86d1bb0",
      "transactionIndex": "61",
      "from": "0x9aa99c23f67c81701c772b106b4f83f6e858dd2e",
      "to": "",
      "value": "0",
      "gas": "6000000",
      "gasPrice": "83924748773",
      "isError": "0",
      "txreceipt_status": "1",
      "input": "0xa9059cbb000000000000000000000000313143c4088a47c469d06fe3fa5fd4196be6a4d600000000000000000000000000000000000000000003b8e97d229a2d54800000",
      "contractAddress": "0xc5102fe9359fd9a28f877a67e36b0f050d81a3cc",
      "cumulativeGasUsed": "10450178",
      "gasUsed": "4457269",
      "confirmations": "122485",
      "methodId": "0x61016060",
      "functionName": ""
    },
    {
      "blockNumber": "14923692",
      "timeStamp": "1654646570",
      "hash": "0xaa45b4858ba44230a5fce5a29570a5dec2bf1f0ba95bacdec4fe8f2c4fa99338",
      "nonce": "7",
      "blockHash": "0x8df71a12a8c06b36c06c26bf6248857dd2a2b75b6edbb4e33e9477078897b282",
      "transactionIndex": "27",
      "from": "0x9aa99c23f67c81701c772b106b4f83f6e858dd2e",
      "to": "0xc5102fe9359fd9a28f877a67e36b0f050d81a3cc",
      "value": "0",
      "gas": "6000000",
      "gasPrice": "125521409858",
      "isError": "0",
      "txreceipt_status": "1",
      "input": "0xa9059cbb000000000000000000000000313143c4088a47c469d06fe3fa5fd4196be6a4d600000000000000000000000000000000000000000003b8e97d229a2d54800000",
      "contractAddress": "",
      "cumulativeGasUsed": "1977481",
      "gasUsed": "57168",
      "confirmations": "122471",
      "methodId": "0xa9059cbb",
      "functionName": "transfer(address _to, uint256 _value)"
    }
  ]
}
```

## Response Parameters

Here's a brief description of the major response parameters.

|    Parameter    |  Type  | Description                                                          |
| :-------------: | :----: | -------------------------------------------------------------------- |
|   blockNumber   |  int   | Block number the transaction is in; `null` if pending                |
|    timeStamp    |  int   | UNIX timestamp                                                       |
|      hash       | String | Transaction hash;                                                    |
|    blockHash    | String | Block hash; `null` if pending                                        |
|      from       | String | Transaction From address                                             |
|       to        | String | Transaction To address; `null` if transaction creates a new contract |
|      value      | String | Value transferred using transaction in `Wei`                         |
|       gas       |  int   | Gas provided by transaction sender                                   |
|      input      | String | Message sent with the transaction                                    |
| contractAddress | String | Address of the newly deployed contract; else `null`                  |
|  functionName   | String | Name of the contract function invoked；subject to wrapper library    |

## Error List

| Code | Description              | Recommended Fix                                                                                                                |
| :--: | ------------------------ | ------------------------------------------------------------------------------------------------------------------------------ |
| 200  | Success response         | -                                                                                                                              |
| 400  | Bad request              | Invalid request parameters. Double check the path URL and the query parameter data types                                       |
| 401  | Unauthorized API request | You need to authorize the request with an API key before sending a request. Follow [this link](x) to find out how              |
| 403  | Forbidden                | You've hit the request limit! Or maybe, your request was blocked by whitelist settings. [Get in touch with us](x) to find out. |
| 429  | Too many requests        | Slow down. You've exceeded the max. concurrent request capacity.                                                               |
| 500  | Internal Server Error    | Oops! Your request couldn't be processed for some reason. [Contact us](x) if you see this.                                     |
