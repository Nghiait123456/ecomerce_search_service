- [Index Products](#index_products)
- [Design analysis Index](#design_analysis_index)

- [Search](#search)
- [Baisc query](#basic_query)
  - [Query #1: Text search for a product](#query_1_text_search_product)
  - [Query #2: Text search for a product with a brand name or a certain phrase](#query_2_text_search_for_a_product_with_a_brand_name_or_a_certain_phrase)
  - [Query #3: Search for a particular category of products](#query_3_search_for_a_particular_category_of_products)
  - [Query #4: Text search for products within a certain category](#query_4_text_search_for_products_within_a_certain_category)
  - [Query #5: Search and filter for products by ratings](#query_5_search_and_filter_for_products_by_ratings)
  - [Query #6: Filter by pricing parameters](#query_6_filter_by_pricing_parameters)
  - [Query #7: Sorting search results](#query_7_sorting_search_results)

- [Advance](#advance)
- [Improving search relevance](#improving_search_relevance)
  - [Boost the relevance of individual fields](#boost_the_relevance_of_individual_fields)
  - [Decrease the relevance of documents for certain search terms](#decrease_the_relevance_of_documents_for_certain_search_terms)
  - [Auto suggest category](#auto_suggest_category)
  - [Useful for handling typos and to provide "Did you mean this instead" feature](#useful_for_handling_typos_and_to_provide_did_you_mean_this_instead)


## Index Products <a name="index_products"></a>
```
PUT /products
{
"mappings": {
      "properties": {
        "brand": {
          "type": "keyword",
          "fields": {
            "raw": {
              "type": "text"
            }
          }
        },
      
        "product_category_tree": {
          "type": "text"
        },
        
        "crawl_timestamp": {
          "type": "date"
        },
        
        "is_FK_Advantage_product": {
          "type": "boolean"
        },
        
         "product_specifications": {
          "type": "object"
        },
        
        "category": {
          "type": "keyword"
        },
        "description": {
          "type": "text"
        },
        "discounted_price": {
          "type": "float"
        },
        "has_next_day_delivery": {
          "type": "boolean"
        },
        "id": {
          "type": "long"
        },
        "image": {
          "type": "keyword"
        },
        "overall_rating": {
          "type": "half_float"
        },
        "pid": {
          "type": "keyword"
        },
        "product_name": {
          "type": "text"
        },
        "product_rating": {
          "type": "half_float"
        },
        "product_url": {
          "type": "keyword"
        },
        "retail_price": {
          "type": "float"
        },
        "uniq_id": {
          "type": "keyword"
        }
      }
    }
}
```


## Search <a name="Search"></a>
## Match <a name="match"></a>
Match query. </br>
Returns documents that match a provided text, number, date or boolean value. The provided text is analyzed before matching. </br>
The match query is the standard query for performing a full-text search, including options for fuzzy matching. </br>


## Query#1: Text search for a producte <a name="query_1_text_search_a_product"></a>
https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-match-query.html </br>
``` 
GET name-of-index/_search
{
  "query": {
    "match": {
      "Column/field within which to search": {
        "query": "Search keywords"
      }
    }
  }
}


GET products/_search
{
  "size": 100,
  "query": {
    "match": {
      "product_name": {
        "query": "shoes"
      }
    }
  }
}

```
## Match Many <a name="match_many"></a>
https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-multi-match-query.html </br>
```
  GET products/_search
{
  "query": {
    "multi_match": {
      "query": "Search keywords",
      "fields": ["field1", "field2"]
    }
  }
}


GET products/_search
{
  "size": 100,
  "query": {
    "multi_match": {
      "query": "shoes",
      "fields": ["product_name", "description"]
    }
  }
}
```
## Query #2: Text search for a product with a brand name or a certain phrase <a name="query_2_text_search_for_a_product_with_a_brand_name_or_a_certain_phrase"></a>
https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-match-query-phrase.html </br>
```
GET name-of-index/_search
{
  "query": {
    "match_phrase": {
      "Column/field within which to search": {
        "query": "Search phrase"
      }
    }
  }
}


GET products/_search
{
  "size": 100,
  "query": {
    "match_phrase": {
      "product_name": {
        "query": "Kindle Paperwhite"
      }
    }
  }
}

GET name-of-index/_search
{
  "query": {
    "multi_match": {
      "query": "Search phrase",
      "fields": [
        "field_1",
        "field_2",
        "field_3"
      ],
      "type": "phrase"
    }
  }
}


GET products/_search
{
  "query": {
    "multi_match": {
      "query": "Kindle Paperwhite",
      "fields": [
        "product_name",
        "description",
      ],
      "type": "phrase"
    }
  }
}
```

## Query #3: Search for a particular category of products <a name="query_3_search_for_a_particular_category_of_products"></a>
https://www-elastic-co.translate.goog/guide/en/elasticsearch/reference/6.8/query-dsl-term-query.html?_x_tr_sl=en&_x_tr_tl=vi&_x_tr_hl=vi </br>

```
GET name-of-index/_search
{
  "query": {
    "term": {
      "name-of-field": "keyword",
    }
  }
}


GET products/_search
{
  "query": {
    "term" : { "category" : "Grocery" } 
  }
}
```

## Query #4: Text search for products within a certain category <a name="query_4_text_search_for_products_within_a_certain_category"></a>
```
  GET name-of-index/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "match/match_phrase/term": {
            "name-of-field": "value" 
          }
        },
        {
          "match/match_phrase/term": {
            "name-of-field": "value" 
          }
        }
      ]
    }
  }
}



GET products/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "match_phrase": {
            "product_name": "Redmi Note 10" 
          }
        },
        {
          "term": {
            "category": "Smartphones" 
          }
        }
      ]
    }
  }
}
```

## Query #5: Search and filter for products by ratings <a name="query_5_search_and_filter_for_products_by_ratings"></a>
https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-range-query.html </br>
``` 
GET name-of-index/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "match/match_phrase/term": {
            "name": "Enter the value you are looking for" 
          }
        },
        {
          "range":{
            "field-name": {
              "gte": "Enter lowest value of the range here",
              "lte": "Enter highest value of the range here"
            }
          }
        }
      ]
    }
  }
}


GET products/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "match_phrase": {
            "product_name": "Redmi Note 10" 
          }
        },
        {
          "term": {
            "category": "Smartphones" 
          }
        },
        {
          "range":{
            "product_rating": {
              "gte": "4"
            }
          }
        }
      ]
    }
  }
}


```
## Query #6: Filter by pricing parameters <a name="query_6_filter_by_pricing_parameters"></a>
```
GET products/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "match_phrase": {
            "product_name": "Redmi Note 10" 
          }
        },
        {
          "term": {
            "category": "Smartphones" 
          }
        },
        {
          "range": {
            "product_rating": {
              "gte": "4"
            }
          }
        },
        {
          "range": {
            "retail_price": {
              "gte": 300,
              "lte": 1200
            }
          }
        }
      ]
    }
  }
}
```

## Query #7: Sorting search results <a name="query_7_sorting_search_results"></a>
``` 
GET name-of-index/_search
{
  "sort" : [
    {
      "field-name": {
        "order": "specify-a-sort-order"
      }
    }
  ],
  "query": {
    ...
  }
}

GET products/_search
{
  "sort" : [
    {
      "retail_price": {
        "order": "asc"
      }
    }
  ],
  "query": {
    "bool": {
      "must": [
        {
          "match_phrase": {
            "product_name": "Redmi Note 10" 
          }
        },
        {
          "term": {
            "category": "Smartphones" 
          }
        },
        {
          "range": {
            "product_rating": {
              "gte": "4"
            }
          }
        },
        {
          "range": {
            "retail_price": {
              "gte": 300,
              "lte": 1500
            }
          }
        }
      ]
    }
  }
}

```






## Advance <a name="advance"></a>
## Improving search relevance <a name="improving_search_relevance"></a>
## Boost the relevance of individual fields  <a name="boost_the_relevance_of_individual_fields"></a>
``` 
GET products/_search
{
  "query": {
    "multi_match": {
            "query": "Redmi Note 10",
            "fields": [
              "product_name^2",
              "description"
            ]
    }
  }
}

GET name-of-index/_search
{
  "query" : {
    "multi_match": {
      "query": "search term",
      "fields": [
        "field1^2",
        "field2",
        "field3"
      ]
    }
  }
}


```

## Decrease the relevance of documents for certain search terms  <a name="decrease_the_relevance_of_documents_for_certain_search_terms"></a>

/** 
todo dont understand 
*/
``` 
GET name-of-index/_search
{
  "query" : {
    "boosting": {
      "positive": {
        "term": {
          "text": "search term"
        }
      },
      "negative": {
        "term": {
          "text": "term to demote"
        }
      },
      "negative_boost": 0.5
    }
  }
}



GET products/_search
{
  "query": {
    "boosting": {
      "positive": {
        "term": {
           "category" : "Smartphones"
        }
      },
      "negative": {
        "match": {
          "category": "usb"
        }
      },
      "negative_boost": 0.1
    }
  }
}
```
## Auto suggest category <a name="autocomplete_suggest_category"></a>
/**
 todo
*/
## Useful for handling typos and to provide "Did you mean this instead" feature. <a name="useful_for_handling_typos_and_to_provide_did_you_mean_this_instead"></a>
/**
todo https://taranjeet.medium.com/elasticsearch-using-completion-suggester-to-build-autocomplete-e9c120cf6d87
*/


