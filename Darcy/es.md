# es

**查找某一笔订单：**

**京东**

{
  "query": {
    "bool": {
      "must": [
        {
          "term": {
            "userId": {
              "value": "1"
            }
          }
        },
        {
          "nested": {
            "path": "trades",
            "query": {
              "bool": {
                "must": [
                  {
                    "term": {
                      "trades.tid": {
                        "value": "212734916733"
                      }
                    }
                  }
                ]
              }
            },
            "inner_hits": {}
          }
        }
      ]
    }
  },
  "_source": "{field}"
}



**淘宝**

{
  "query": {
    "bool": {
      "must": [
        {
          "term": {
            "userId": {
              "value": "6000000009"
            }
          }
        },
        {
          "nested": {
            "path": "trades",
            "query": {
              "bool": {
                "must": [
                  {
                    "term": {
                      "trades.tid": {
                        "value": "2592162684987910325"
                      }
                    }
                  }
                ]
              }
            },
            "inner_hits": {}
          }
        }
      ]
    }
  }
}



**抖音**

{
  "query": {
    "bool": {
      "must": [
        {
          "term": {
            "inBlacklist": false
          }
        },
        {
          "exists": {
            "field": "mobileId"
          }
        },
        {
          "nested": {
            "path": "trades",
            "query": {
              "bool": {
                "must": [
                  {
                    "terms": {
                      "trades.tid": [
                        "4833028012736626410"
                      ]
                    }
                  }
                ]
              }
            }
          }
        }
      ]
    }
  }
}





{
  "query": {
    "bool": {
      "must": [
        {
          "term": {
             "userId": {
              "value": "7"
            }
          }
        },
        {
          "nested": {
            "path": "trades",
            "query": {
              "bool": {
                "must": [
                  {
                    "terms": {
                      "trades.tid": [
                        "4871545030329702122"
                      ]
                    }
                  }
                ]
              }
            }
          }
        }
      ]
    }
  }
}



**抖音： curl -XGET 'http://192.168.8.10:10000/dy_customer_1/_search'** （**查询指定返回加密字段**）

{
  "query": {
    "bool": {
      "must": [
        {
          "term": {
            "inBlacklist": false
          }
        },
        {
          "term": {
            "userId": {
              "value": "21"
            }
          }
        },
        {
          "exists": {
            "field": "mobileId"
          }
        },
        {
          "nested": {
            "path": "trades",
            "query": {
              "bool": {
                "must": [
                  {
                    "terms": {
                      "trades.tid": [
                        "4863439761331354785"
                      ]
                    }
                  }
                ]
              }
            }
          }
        }
      ]
    }
  }
}





**查看某笔订单的评价信息（抖音）：**



dy_comment_1/_search

{
  "query": {
    "bool": {
      "must": [
        {
          "term": {
            "userId": {
              "value": "7"
            }
          }
        },
        {
          "term": {
            "oid": {
              "value": "4801369856040013546"
            }
          }
        }
      ]
    }
  }
}




**查看某个人的信息：**

**customer_v4_10/CustomerEs/_search**

{
  "query": {
    "bool": {
      "must": [
        {
          "term": {
            "userId": {
              "value": "6000000009"
            }
          }
        },
        {
          "term": {
            "ouid": {
              "value": "AAEJARHcAAAr4sUAASkW6FrJ"
            }
          }
        }
      ]
    }
  }
}



**删除某个用户:**

**customer_v4_10/CustomerEs/_delete_by_query**

{
  "query": {
    "bool": {
      "must": [
        {
          "term": {
            "userId": {
              "value": "1728689939"
            }
          }
        }
      ]
    }
  }
}







**删除评价信息（淘宝）：**

POST traderate_v2_10/TradeRate/_delete_by_query

{
  "query": {
    "bool": {
      "must": [
        {
          "term": {
            "userId": {
              "value": "6000000009"
            }
          }
        },
        {
          "term": {
            "tradeId": "2230130451837561928"
          }
        }
      ]
    }
  }
}





**删除某个订单（抖音）：**

POST  dy_customer_1/_delete_by_query

{
  "query": {
    "bool": {
      "must": [
        {
          "term": {
            "userId": {
              "value": "7"
            }
          }
        },
        {
          "nested": {
            "path": "trades",
            "query": {
              "bool": {
                "must": [
                  {
                    "term": {
                      "trades.tid": {
                        "value": "4801369856040013546"
                      }
                    }
                  }
                ]
              }
            },
            "inner_hits": {}
          }
        }
      ]
    }
  },
  "_source": "{field}"
}





**删除一个用户下所有订单：**



POST  dy_customer_1/_delete_by_query

{
  "query": {
    "bool": {
      "must": [
        {
          "term": {
            "userId": {
              "value": "21"
            }
          }
        }
      ]
    }
  }
}



**有效客户数（有手机号客户数）：**



{
  "query": {
    "bool": {
      "must": [
        {
          "term": {
            "userId": {
              "value": "6000000009"
            }
          }
        },
        {
          "exists": {
            "field": "mobile"
          }
        }
      ]
    }
  }
}





**查询某个人的指定信息（通过customer_id查询）：**



**淘宝**

{
  "_source": "mobile",
  "query": {
    "bool": {
      "must": [
        {
          "term": {
            "userId": {
              "value": "6000000009"
            }
          }
        },
        {
          "term": {
            "id": {
              "value": "451088300"
            }
          }
        }
      ]
    }
  }
}



**抖音：查看某个人的最后营销时间**

{
  "_source": "lastMarketingSmsTime",
  "query": {
    "bool": {
      "must": [
        {
          "term": {
            "userId": {
              "value": "7"
            }
          }
        },
        {
          "term": {
            "mobileId": {
              "value": "1"
            }
          }
        }
      ]
    }
  }
}



**查询系统柜中有手机号的客户**



{
  "query": {
    "bool": {
      "must": [
        {
          "term": {
            "userId": {
              "value": "6000000009"
            }
          }
        },
        {
          "nested": {
            "path": "trades",
            "query": {
              "bool": {
                "must_not": [
                  {
                    "exists": {
                      "field": "mobile"
                    }
                  }
                ]
              }
            }
          }
        }
      ]
    }
  }
}





**查看导入的订单（trades.imported为true)**

customer_v4_10/CustomerEs/_search

{
  "query": {
    "bool": {
      "must": [
        {
          "term": {
            "userId": {
              "value": "6000000009"
            }
          }
        },
        {
          "nested": {
            "path": "trades",
            "query": {
              "bool": {
                "must": [
                  {
                    "term": {
                      "trades.imported": {
                        "value": "true"
                      }
                    }
                  }，{
                    "exists":{
                    "field":"trades.orders"
                    }
                  } 

]
              }
            }
          }
        }
      ]
    }
  },
  "sort": [
    {
      "trades.createdTime": {
        "order": "desc"
      }
    }
  ],
  "_source": {
    "include": [
      "trades",
      "trades.endTime",
      "trades.orders.itemId"
    ]
  }
}





**修改es数据，字段赋值自己修改满足条件**

POST customer_10_v3/CustomerDetail/6000000009_~AQmXofSoCgLERGiTcntyWA==~1~/_/_update_by_query
{
 "doc": {
  "firstPayTime":1600415100000
 }
}





**抖音更新某个字段（修改mobile_id）**

POST  dy_customer_1/_update_by_query

{
  "script": {
    "source": "ctx._source.mobileId=292652",
    "lang": "painless"
  },
  "query": {
    "term": {
      "id": "21_TP9j/QqAJYFQyodavy4GdC3fNgsEYaNDNiP4jxq039Y="
    }
  }
}





**淘宝将某个字段：旗帜置为空**

POST /customer_v4_10/CustomerEs/_update_by_query
{
  "script": {
    "source":  "for(t in ctx._source.trades){if(t.id+\"\" == params.id){t.remove(\"sellerFlag\")}}",
    "params": {
      "id": "1237145592229878891"
    }
  },
  "query": {
    "bool": {
      "must": [
        {
          "term": {
            "encryptNick": {
              "value": "~X0etJMV16R+E15aQHi4I2/I6P5SDoQO92ddNRmF46Fk=~1~",
              "boost": 1
            }
          }
        }
      ]
    }
  }
}





**淘宝将某个字段更新：旗帜重置**

POST /customer_v4_10/CustomerEs/_update_by_query_

_{
  "script": {
    "source":   "for(t in ctx._source.trades){t.sellerFlag=1}",
    "params": {
      "id": "1237145592229878891"
    }
  },
  "query": {
    "bool": {
      "must": [
        {
          "term": {
            "encryptNick": {
              "value": "~X0etJMV16R+E15aQHi4I2/I6P5SDoQO92ddNRmF46Fk=~1~",
              "boost": 1
            }
          }
        }
      ]
    }
  }
} 



**抖音售后时间applyTime/售后状态afterSaleStatus/售后类型afterSaleType/售后原因refundReason/退款金额refundFee**



{
  "query": {
    "bool": {
      "must": [
        {
          "term": {
            "userId": {
              "value": "7"
            }
          }
        },
        {
          "nested": {
            "path": "trades.aftersales",
            "query": {
              "bool": {
                "must": [
                  {
                    "term": {
                      "trades.aftersales.applyTime": {
                        "value": "1638340863000"
                      }
                    }
                  }
                ]
              }
            }
          }
        }
      ]
    }
  }
}



**抖音查询售后订单：**



{
  "query": {
    "bool": {
      "must": [
        {
          "term": {
            "userId": {
              "value": "21"
            }
          }
        },
        {
          "nested": {
            "path": "trades.aftersales",
            "query": {
              "bool": {
                "must": [
                  {
                    "term": {
                      "trades.aftersales.aftersaleId": {
                        "value": "6998320812528812329"
                      }
                    }
                  }
                ]
              }
            }
          }
        }
      ]
    }
  }
}





**抖音查询退货物流：**

**已发货：**

{
  "query": {
    "bool": {
      "must": [
        {
          "term": {
            "userId": {
              "value": "20"
            }
          }
        },{
          "nested": {
            "path": "trades.refunds",
            "query": {
              "bool": {
                "must": [
                  {
                  

                     "exists":{
                         "field":"trades.refunds.logisticsCode"
            
                     }
                  }
                ]
              }
            }
          }
        }
      ]
    }
  }
}





**具体物流单号：**

customer_dy_test_new_0/_search

{
  "query": {
    "bool": {
      "must": [
        {
          "term": {
            "userId": {
              "value": "20"
            }
          }
        },{
          "nested": {
            "path": "trades.refunds",
            "query": {
              "bool": {
                "must": [
                  {
                  

                     "term":{
                         "trades.refunds.logisticsCode": {
                         "value":"YT0183628283"
            }
                     }
                  }
                ]
              }
            }
          }
        }
      ]
    }
  }
}

**未发货：**

{
  "query": {
    "bool": {
      "must": [
        {
          "term": {
            "userId": {
              "value": "20"
            }
          }
        },{
          "nested": {
            "path": "trades.refunds",
            "query": {
              "bool": {
                "must_not": [
                  {
                  

                     "exists":{
                         "field":"trades.refunds.logisticsCode"
            
                     }
                  }
                ]
              }
            }
          }
        }
      ]
    }
  }
}







**查询预售订单状态的人：**

{
  "query": {
    "bool": {
      "must": [
        {
          "term": {
            "userId": {
              "value": "21"
            }
          }
        },
        {
          "nested": {
            "path": "trades",
            "query": {
              "bool": {
                "must": [
                  {
                    "term": {
                      "trades.stepTradeInfo.stepTradeStatus": {
                        "value": "FRONT_NOPAID_FINAL_NOPAID"
                      }
                    }
                  }
                ]
              }
            }
          }
        }
      ]
    }
  }
}





**修改抖音预售订单状态：**

dy_customer_1/_update_by_query

{
  "query": {
    "term": {
      "id": "21_cN3Ol3XHEah/usEPvG+N3VtLv1/LIk6RLZVGDDnKX1c="
    }
  },
  "script": {
    "source": "for (t in ctx._source.trades) {if (t.tid == '4876374824132756820'){t.stepTradeInfo.stepTradeStatus = 'FRONT_PAID_FINAL_NOPAID_CLOSED'}}",
    "lang": "painless"
  }
}



将最后营销时间置为空，模拟用户从来没有营销过

customer_v4_10/_update_by_query

{
  "query": {
    "term": {
      "id": "451088300"
    }
  },
  "script": {
    "source": "ctx._source.remove(\"lastMarketingSmsTime\")"
  }
}

