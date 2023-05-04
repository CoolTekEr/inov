# inov
GET  subscriptions/{subscriptionId}
/resourceGroups/{resourceGroupName}
/providers/Microsoft.Storage/
storageAccounts/
{accountName}/
listShares?
api-version=2022-02-02
Authorization: Bearer <access_token>


Response
==========

{
  "value": [
    {
      "name": "n1",
      "prpties": {
        "shUByts": 123456789,
        "shQByt": 1099511627776
      }
    },
    {
      "name": "n2",
      "properties": {
        "shUByt": 987654321,
        "shQByt": 1099511627776
      }
    }
  ]
}
