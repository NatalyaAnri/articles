Использование JOLT трансформаций в NIFI
JOLT используется для изменения структуры json документов: разворачивание массивов, изменение вложенности, переименование полей и т.д. 

Пример трансформации для разворачивания массива:

//input json
`{
  "_id": "jdhdcjesdjdx6rm4l478jnc",
  "CreatedDate": "2019-08-14T18:17:02.726Z",
  "UpdatedDate": "2019-11-14T09:39:29.936Z",
  "UserId": "6jkhtthb8cca4201376c6",
  "TransactionIds": [
    1295132607,
    1298378873,
    1305346157,
    1312322697
  ],
  "ExternalAccumulationId": "gjjdp189djuhj8933",
  "Status": 0
}`
//JOLT spec
`[
  {
    "operation": "shift",
    "spec": {
      "*": "&",
      "TransactionIds": {
        "*": "TransactionIds.[].TransactionIds"
      }
    }
    }
  ,
  {
    "operation": "shift",
    "spec": {
      "TransactionIds": {
        "*": {
          "@": "[&1]",
          "@(2,_id)": "[&1]._id",
          "@(2,CreatedDate)": "[&1].CreatedDate",
          "@(2,UpdatedDate)": "[&1].UpdatedDate",
          "@(2,ReceiverUserId)": "[&1].ReceiverUserId",
          "@(2,PayoutTransactionIds)": "[&1].PayoutTransactionIds",
          "@(2,ExternalAccumulationId)": "[&1].ExternalAccumulationId",
          "@(2,Status)": "[&1].Status",
          "@(2,UserId)": "[&1].UserId"
        }
      }
    }
    }
  ]
`
//output json
`[ {
  "TransactionIds" : 1295132607,
  "_id" : "jdhdcjesdjdx6rm4l478jnc",
  "CreatedDate" : "2019-08-14T18:17:02.726Z",
  "UpdatedDate" : "2019-11-14T09:39:29.936Z",
  "ExternalAccumulationId" : "gjjdp189djuhj8933",
  "Status" : 0,
  "UserId" : "6jkhtthb8-33f0-4453-cca4201376c6"
}, {
  "TransactionIds" : 1298378873,
  "_id" : "jdhdcjesdjdx6rm4l478jnc",
  "CreatedDate" : "2019-08-14T18:17:02.726Z",
  "UpdatedDate" : "2019-11-14T09:39:29.936Z",
  "ExternalAccumulationId" : "gjjdp189djuhj8933",
  "Status" : 0,
  "UserId" : "6jkhtthb8-33f0-4453-cca4201376c6"
}, {
  "TransactionIds" : 1305346157,
  "_id" : "jdhdcjesdjdx6rm4l478jnc",
  "CreatedDate" : "2019-08-14T18:17:02.726Z",
  "UpdatedDate" : "2019-11-14T09:39:29.936Z",
  "ExternalAccumulationId" : "gjjdp189djuhj8933",
  "Status" : 0,
  "UserId" : "6jkhtthb8-33f0-4453-cca4201376c6"
}, {
  "TransactionIds" : 1312322697,
  "_id" : "jdhdcjesdjdx6rm4l478jnc",
  "CreatedDate" : "2019-08-14T18:17:02.726Z",
  "UpdatedDate" : "2019-11-14T09:39:29.936Z",
  "ExternalAccumulationId" : "gjjdp189djuhj8933",
  "Status" : 0,
  "UserId" : "6jkhtthb8-33f0-4453-cca4201376c6"
} ]
`
