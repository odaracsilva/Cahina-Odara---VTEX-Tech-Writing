## How to create an order invoicing integration
#### Understand how to create an order invoicing integration

Invoicing an order means entering the invoice of the items into the Order Management System (OMS) module, making it available to the customer, and completing the order flow.

In this article, learn how to optimize creating an order invoicing integration.

#### Understanding the order flow

Before starting the integration and invoicing process, it is necessary to understand the relevant status for the invoice to be received successfully. 

Through the order flow, it is possible to monitor the lifecycle of an order until it reaches one of the final status  (`invoiced` or `canceled`). 

In this flow, the invoicing process is recognized once the order reaches the `ready-for-handling` status.

![orderFlow](https://iili.io/dmI3GUX.png )

>⚠️ Attempting to send the order invoice in status preceding `ready-for-handling` will result in the invoice being discarded.

> ℹ️ Some order status can be seen via API but are not visible in the order flow seen in Admin. For more information, see the guide  [Order flow](https://help.vtex.com/pt/tutorial/fluxo-e-status-de-pedidos--tutorials_196).


#### Integrating orders

To monitor the status change of orders, you can use either Feed or Hook. The Feed works as a queue of events that can be periodically queried, while the Hook acts reactively, notifying an endpoint about a change in the order JSON.

> ℹ️ Read [Create or update Feed configuration](https://developers.vtex.com/docs/api-reference/orders-api#post-/api/orders/feed/config) or [Create or update Hook configuration](https://developers.vtex.com/docs/api-reference/orders-api#post-/api/orders/hook/config) for instructions on setting up a Feed or Hook.

Both monitoring forms have two filters that can be applied: `fromWorkflow` and `fromOrders`.

`fromWorkflow`

This type of filter allows consumption of data from the order flow, meaning only the status filtered will be queued in the Feed’s event list or notified to the endpoint set up in the Hook.
In a scenario where you want to monitor completed orders and orders that are ready for handling in the Feed, you have the following configuration:

```json
"filter": {
        "type": "FromWorkflow",
        "status": [
            "order-completed",
            "ready-for-handling"
        ]
    },
    "queue": {
        "visibilityTimeoutInSeconds": 240,
        "messageRetentionPeriodInSeconds": 345600
    }
```
Considering the same scenario for the Hook, you have:

```json
{
    "filter": {
        "type": "FromWorkflow",
        "status": [
            "order-completed",
            "ready-for-handling"
        ]
    },
    "hook": {
        "url": "https://endpoint.example/path",
            "headers": {
                "key": "value"
            }
    }
}
```

`fromOrders`

This type of filter allows any property of the order JSON to be filtered when infomed in the `expression` string. This provides greater customization compared to the `fromWorkflow` filter, as various criteria can be combined to select orders.

Considering a scenario where you want to filter orders that are in a certain status and commercial policy:

```json
{
  "filter": {
    "type": "FromOrders",
     "expression": "(status = \"handling\" and salesChannel = \"2\")",
    "disableSingleFire": false
  },
  "queue": {
    "visibilityTimeoutInSeconds": 250,
    "MessageRetentionPeriodInSeconds": 345600
  }
}
```

For the same scenario with a Hook configuration:

```json
{
  "filter": {
    "type": "FromOrders",
    "expression": "(status = \"handling\" and salesChannel = \"2\")",
    "disableSingleFire": false
  },
  "hook": {
    "url": "https://endpoint.example/path",
    "headers": {
      "key": "value"
    }
  }
}
```

> ℹ️ To learn more, read [Feed v3](https://developers.vtex.com/docs/guides/orders-feed).

#### Instructions

##### Step 1 – Set up a Feed or Hook to integrate orders

To start the invoicing process, it iss necessary to ensure that the orders are properly integrated into the ERP, so you must set up a Feed or Hook that considers the `ready-for-handling` status. You can find examples above.

#### Step 2 – Invoicing an order

The invoice or partial invoice is sent through a POST call to the endpoint [Order invoice notification](https://developers.vtex.com/docs/api-reference/orders-api#post-/api/oms/pvt/orders/-orderId-/invoice), which must be requested for orders that are in one of the following statuses: `ready-for-handling`, `start-handling`, `handling`, and `invoice`.

| **API Status**      | **Meaning**                                                                                                                                                                                                                                             |
|---------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **invoice**         | The platform is verifying the invoice. If the order remains at this stage, there may have been a problem settling the payment or adding the invoice. Learn more in [​​My store's order still has the status "Verifying invoice"](https://help.vtex.com/en/tutorial/o-pedido-da-minha-loja-esta-parado-no-status-verificando-nota-fiscal--2YY7ILOOd0lEjpiT7SSgag?utm_term=&utm_campaign=BRA_pmax_2023&utm_source=adwords&utm_medium=ppc&hsa_acc=9663921675&hsa_cam=20809358286&hsa_grp=&hsa_ad=&hsa_src=x&hsa_tgt=&hsa_kw=&hsa_mt=&hsa_net=adwords&hsa_ver=3&gad_source=1&gclid=CjwKCAjwx4O4BhAnEiwA42SbVBaXWPIlN61faHuN6I_nuAyvWKJ0bgqbbQvqYDvrlSuDStlz5JuOWBoCcGIQAvD_BwE). |
| **start-handling**  | Authorization status to continue the order handling flow. It is used when authorized manually. If there is ERP integration, this status waits for the ERP confirmation for the order flow to continue.                                             |
| **ready-for-handling** | Status that indicates that you need to start order handling, prepare the invoice, and track the order. In this status, the platform reserves the item. This is usually done by an ERP integration, but you can do it manually through order management. The order will only go to the next stage once this one has been confirmed. |
| **handling**        | In this status, the order items are reserved. The order remains at this status until it receives an invoice notification, usually from the ERP.                                                                                                                              |

>⚠️ When invoicing an order, the value recorded on the invoice does not always match the total or initial order value. In these cases, a partial invoice must be inserted. For more details, read [Partial invoices](https://help.vtex.com/tracks/orders--2xkTisx4SXOWXQel8Jg8sa/q9GPspTb9cHlMeAZfdEUe).

For a successful invoice, the fields `type`, `issuanceDate`, `invoiceNumber`, `invoiceValue`, and `items` must be sent, 
as in the example below: 

###### Example

```json
{
    "type": "Output",
    "issuanceDate": "2024-01-31T18:25:43-05:00",
    "invoiceNumber": "DFG-v7731485plzv-01",
    "invoiceValue": "2499",
    "items": [
      {
        "id": "123",
        "price": 2499,
        "description": "335",
        "quantity": 2
      }
    ]
}
```
>⚠️  The "items" array is not mandatory. However, if you need to send a return invoice (`"type": "input"`) and the output invoice did not declare the array, it will not be possible to perform the return.

#### Step 3 – Updating an invoice

Some invoice properties can be changed or sent via the PATCH [Update order's partial invoice (send tracking number)](https://developers.vtex.com/docs/api-reference/orders-api#patch-/api/oms/pvt/orders/-orderId-/invoice/-invoiceNumber-). This route is recommended for sending or updating tracking information, and it requires the fields `trackingNumber`, `trackingUrl`, `dispatchedDate`, and `courier`. The `embeddedInvoice` string is mandatory for external marketplaces, such as Mercado Livre, and inform the XML text of the invoice.

```json
{
  "trackingNumber": "87658",
  "trackingUrl": "https://www.tracking.com/url",
  "courier": "carrierOne",
  "dispatchedDate": "2022-02-08T13:16:13.4617653+00:00"
}
```

>⚠️  The use of the POST call to the endpoint [Order invoice notification](https://developers.vtex.com/docs/api-reference/orders-api#post-/api/oms/pvt/orders/-orderId-/invoice) may also override invoice tracking information. However, for tracking updates on external marketplaces, the flow recognizes changes only with the use of the PATCH call to the endpoint [Update order's partial invoice (send tracking number)](https://developers.vtex.com/docs/api-reference/orders-api#patch-/api/oms/pvt/orders/-orderId-/invoice/-invoiceNumber-).


#### Common use cases for route modification
- Changing tracking code
- Changing the courier

#### Common errors when sending invoices

*Attempt to change the invoiceValue* 

```json
"NotifyInvoice: {\"title\":\"Validation errors occurred\",\"status\":400,\"detail\":\"Invoice value cannot be modified\",\"traceId\":\"00-d0448b70935f8b840098ac362277dde2-79bbe1aa6b41974a-01\"}",
```

The invoice value sent to the order cannot be overwritten, so it is important to send the correct value. If the invoice has a value lower than the total order value, a partial invoice can be sent with the remaining amount. For invoices added with a value higher than the order total, it is not possible to edit or delete the invoice.


*Incorrectly formatted or missing information* 

```json
400 Bad Request: "{"error":{"code":"1","message":"Invoice Request is invalid. The following fields are mandatory: invoiceNumber, invoiceValue and issuanceDate","exception":null}}"
```

This response occurs when some required information is not sent in the POST [Order invoice notification](https://developers.vtex.com/docs/api-reference/orders-api#post-/api/oms/pvt/orders/-orderId-/invoice) or when the format of a field does not match what is requested by the documentation.
