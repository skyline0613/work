



### Request
```json
{
   "transactionId": "065c3f40-afbb-4282-986f-2125ede19023",
   "sysId":"LINEBC",
   "id":"B123456789",
   "insuredList ": ["A254068417","G194016688"]
  }

```



### Response


#### success
```json

{
    "transactionId": "065c3f40-afbb-4282-986f-2125ede19023",
    "returnCode": "00",
    "returnMessage": "success",
    "returnData": {
        "insuredList": [
{
            "id":"A254068417",
            "amountList":[2000,1900,1800,1700,1600,1500,1400,1300,1200,1100,1000,
900,800,700,600,500,400,300,200,100]
},
            "id":"G194016688",
            "amountList":[55.2]
}
         ]
    }
}
```


#### No data
```json

{
    "transactionId": "065c3f40-afbb-4282-986f-2125ede19023",
    "returnCode": "06",
    "returnMessage": "data not found",
    "returnData": null
}
```


#### Error
```json


{
    "transactionId": "065c3f40-afbb-4282-986f-2125ede19023",
    "returnCode": "99",
    "returnMessage": "server error",
    "returnData": null,
    "error": {
        "type": "INTERNAL_SERVER_ERROR",
        "details": "Database connection timeout",
        "error_code": "DB_TIMEOUT_001"
    }
}
```