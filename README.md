# wibmopay-server

## UPI Related calls

- [UPI Virtual Address validation](https://github.com/mypoolin/mypoolin-server/blob/master/README.md#upi-virtual-address-validation)

- [UPI Send (Single virtual address)](https://github.com/mypoolin/mypoolin-server/blob/master/README.md#upi-send-single-virtual-address)

- [UPI Send Async (Single virtual address)](https://github.com/mypoolin/mypoolin-server/blob/master/README.md#upi-send-async-single-virtual-address)

- [UPI Transaction Status](https://github.com/mypoolin/mypoolin-server/blob/master/README.md#upi-transaction-status)

- [Check credit balance](https://github.com/mypoolin/mypoolin-server/blob/master/README.md#check-credit-balance)

## IMPS Related Calls

- [IMPS Send (Single Bank Account)](https://github.com/mypoolin/mypoolin-server/blob/master/README.md#imps-send-single-bank-account)

- [IMPS Send Async (Single Bank Account)](https://github.com/mypoolin/mypoolin-server/blob/master/README.md#imps-send-async-single-bank-account)

- [IMPS Transaction Status](https://github.com/mypoolin/mypoolin-server/blob/master/README.md#imps-transaction-status)

- [Check credit balance](https://github.com/mypoolin/mypoolin-server/blob/master/README.md#check-credit-balance-1)


## General Information

### Base URL for UPI calls:

https://sdk.mypoolin.com/merchants_upi

### Base URL for IMPS calls:

https://sdk.mypoolin.com/merchants_imps

**API Key will be shared by Mypoolin Admin**



## UPI Related calls

### UPI Virtual Address validation

- Endpoint		- /validate_vpa
- Request Type	- POST
- Parameters - 
  1. beneficiary_virtual_address (UPI Virtual Address of beneficiary to be validated)

**_Sample Request_**

`curl -X POST https://sdk.mypoolin.com/merchants_upi/validate_vpa -d "beneficiary_virtual_address=8800149537@upi" -H "apikey:API_KEY"`



### UPI Send (Single virtual address)

- Endpoint		- /request_upi_single
- Request Type	- POST
- Parameters - 
  1. beneficiary_virtual_address (UPI Virtual Address of beneficiary to be validated)
  2. beneficiary_amount (Amount to be credited to the beneficiary)


**_Sample Request_**

`curl -X POST https://sdk.mypoolin.com/merchants_upi/request_upi_single -d "beneficiary_virtual_address=8800149537@upi" -d "beneficiary_amount=1" -H "apikey:API_KEY"
`

**_Sample Response_**

Success (status code - 200)
```Json
{
    "message": "Transaction Successful",
    "mypoolin_commission": 3,
    "order_id": "UPII447I1541153265190IU",
    "other_commission": 0,
    "sent_amount": 1,
    "status": "success"
}

```  
Failed (status code - 200)
```
{
    "message": "Virtual Address Not Valid",
    "mypoolin_commission": 0,
    "order_id": "UPII447I1541154557275IU",
    "other_commission": 0,
    "sent_amount": 1,
    "status": "failed"
}
```

Pending (status code - 200)
```
{
    "message": "Transaction Pending",
    "mypoolin_commission": 0,
    "order_id": "UPII1157I1542261163412IU",
    "other_commission": 0,
    "sent_amount": 1,
    "status": "pending"
}
```

### UPI Send Async (Single virtual address)

- Endpoint		- /request_upi_single_async
- Request Type	- POST
- Parameters - 
  1. beneficiary_virtual_address (UPI Virtual Address of beneficiary to be validated)
  2. beneficiary_amount (Amount to be credited to the beneficiary)


**_Sample Request_**

`curl -X POST https://sdk.mypoolin.com/merchants_upi/request_upi_single_async -d "beneficiary_virtual_address=8800149537@upi" -d "beneficiary_amount=1" -H "apikey:API_KEY"`


### UPI Transaction Status

- Endpoint		- /check_transaction_status
- Request Type	- POST
- Parameters - 
  1. order_id (Order id obtained from /request_upi_single request)


**_Sample Request_**

`curl -X POST https://sdk.mypoolin.com/merchants_upi/check_transaction_status -H 'apikey: API_KEY' -d order_id=None1510559700IU`

**_Sample Response_**  
Success (status code - 200)
```Json
{
    "response": {
        "mypoolin_commission": 3,
        "order_id": "UPII447I1541153265190IU",
        "transfer_amount": 1,
        "transfer_bank_ref_no": "830615315861",
        "transfer_txn_status": "COMPLETED",
        "transfer_type": "UPI"
    },
    "status": "success"
}
```
Failed (status code - 200)
```
{
    "response": {
        "mypoolin_commission": 0,
        "order_id": "UPII447I1541153752903IU",
        "transfer_amount": 1,
        "transfer_bank_ref_no": "",
        "transfer_txn_status": "Virtual Address Not Valid",
        "transfer_type": "UPI"
    },
    "status": "failed"
}
```

Failed (status code - 400)
```
{
    "response": {
        "mypoolin_commission": 0,
        "order_id": "UPII447I1541153265190I",
        "transfer_amount": "",
        "transfer_bank_ref_no": "",
        "transfer_txn_status": "Order Id Not Found",
        "transfer_type": "UPI"
    },
    "status": "failed"
}
```


### Check credit balance

- Endpoint		- /get_balance
- Request Type	- GET


**_Sample Request_**

`curl -X GET https://sdk.mypoolin.com/merchants_upi/get_balance -H 'apikey: API_KEY'`


## IMPS Related calls


### IMPS Send (Single Bank Account)

- Endpoint		- /request_imps_single
- Request Type	- POST
- Parameters - 
  1. beneficiary_account_number (Account number of the beneficiary)
  2. beneficiary_ifsc_code (IFSC Code of the bank branch associated with the account)
  3. beneficiary_amount (Amount to be credited to the beneficiary)


**_Sample Request_**

`curl -X POST https://sdk.mypoolin.com/merchants_imps/request_imps_single -H 'apikey: API_KEY' -F 'beneficiary_name=Shobhit Verma' -F 'beneficiary_account_number=8411566856' -F 'beneficiary_ifsc_code=KKBK0005029' -F 'beneficiary_amount=1'`

**_Sample Response_**

Success (status code - 200)
```Json
{
    "message": "Transaction Successful",
    "mypoolin_commission": 5,
    "other_commission": 0,
    "requestNo": "IMPSI447I1540982809312IU",
    "sent_amount": 1,
    "status": "success"
}
```  
Failed (status code - 200)
```
{
    "message": "Account Details Not Matching",
    "mypoolin_commission": 0,
    "other_commission": 0,
    "requestNo": "IMPSI447I1540982781373IU",
    "sent_amount": 1,
    "status": "failed"
}
```

### IMPS Send Async (Single Bank Account)


- Endpoint		- /request_imps_single_async
- Request Type	- POST
- **Note			- This call makes a non blocking call to the IMPS switch. The requester only gets the order ID back and is expected to hit transaction status for the status**

- Parameters - 
  1. beneficiary_account_number (Account number of the beneficiary)
  2. beneficiary_ifsc_code (IFSC Code of the bank branch associated with the account)
  3. beneficiary_amount (Amount to be credited to the beneficiary)


**_Sample Request_**

`curl -X POST https://sdk.mypoolin.com/merchants_imps/request_imps_single -H 'apikey: API_KEY' -F 'beneficiary_name=Shobhit Verma' -F 'beneficiary_account_number=8411566856' -F 'beneficiary_ifsc_code=KKBK0005029' -F 'beneficiary_amount=1'`



### IMPS Transaction Status


- Endpoint		-  /check_transaction_status
- Request Type	- POST
- Parameters - 
  1. order_id (Order id obtained from /request_imps_single request)

**_Sample Request_**

`curl -X POST  https://sdk.mypoolin.com/merchants_imps/check_transaction_status  -H 'apikey: API_KEY' -F order_id=IMPSIMERI28I1510723520IU`

**_Sample Response_**  
Success (status code - 200)
```Json
{
    "response": {
        "mypoolin_commission": 5,
        "order_id": "IMPSI447I1540974899279IU",
        "transfer_amount": 1,
        "transfer_bank_ref_no": "830414452718",
        "transfer_txn_status": "COMPLETED",
        "transfer_type": "IMPS"
    },
    "status": "success"
}
```
Failed (status code - 200)
```
{
    "response": {
        "mypoolin_commission": 0,
        "order_id": "IMPSI447I1540979902502IU",
        "transfer_amount": 1,
        "transfer_bank_ref_no": "",
        "transfer_txn_status": "Account Details Not Matching",
        "transfer_type": "IMPS"
    },
    "status": "failed"
}
```

Pending (status code - 200)
```
{
   "response": {
       "mypoolin_commission": 0,
       "order_id": "IMPSI447I1548920283487IU",
       "transfer_amount": 1,
       "transfer_bank_ref_no": "",
       "transfer_txn_status": "Transaction in pending state",
       "transfer_type": "IMPS"
   },
   "status": "pending"
}
```

Failed (status code - 400)
```
{
    "response": {
        "mypoolin_commission": 0,
        "order_id": "IMPSI447I1541152208766I",
        "transfer_amount": "",
        "transfer_bank_ref_no": "",
        "transfer_txn_status": "Order Id Not Found",
        "transfer_type": "IMPS"
    },
    "status": "failed"
}
```


### Check credit balance


- Endpoint		-  /get_balance
- Request Type	- POST
- Parameters - 
  1. order_id (Order id obtained from /request_imps_single request)

**_Sample Request_**

`curl -X GET  https://sdk.mypoolin.com/merchants_imps/get_balance -H 'apikey: API_KEY'`


For any queries, feel free to reach out to the developers on backend@wibmopay.com


**Documentation Last Updated - 30th October 2018**

