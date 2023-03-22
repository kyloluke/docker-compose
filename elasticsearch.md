
# 此文档基于版本 es-7.17.0

# 概念
MySQL	Elasticsearch
表（Table）	        索引（Index）
记录（Row）	        文档（Document）
字段（Column）	    字段（Fields）

# 操作

- 删除所有
    `curl -XDELETE 172.20.0.4:9200/_all`
## 索引
- 1.创建<br>
    `curl -XPUT http://172.20.0.4:9200/test_index` <br>
    在 Elasticsearch 的返回中如果包含了 "acknowledged" : true, 则代表请求成功 


- 2.查询<br>
    `curl http://172.20.0.4:9200/test_index?pretty` <br>
    查询索引 json格式显示test_index索引
    

## 类型  定义一张表的字段类型
```
    curl -H'Content-Type: application/json' -XPUT http://172.20.0.4:9200/test_index/_mapping?pretty -d'{
    "properties": {
        "title": { "type": "text", "analyzer": "ik_smart" }, 
        "description": { "type": "text", "analyzer": "ik_smart" },
        "price": { "type": "scaled_float", "scaling_factor": 100 }
    }
    }'
```
- 提交数据中的 properties 代表这个索引中各个字段的定义，其中 key 为字段名称，value 是字段的类型定义；
- type 定义了字段的数据类型，常用的有 text / integer / date / boolean，当然还有许多类型，不一一列出。<br>
类型参考: https://www.elastic.co/guide/en/elasticsearch/reference/current/mapping-types.html
- analyzer 是一个新的概念，这是告诉 Elasticsearch 应该用什么方式去给这个字段做分词，这里我们用了 ik_smart，是一个中文分词器，后面会有介绍。


## 文档  插入数据
```
curl -H'Content-Type: application/json' -XPUT http://172.20.0.4:9200/test_index/_doc/1?pretty -d'{
    "title": "iPhone X",
    "description": "新品到货",
    "price": 8848
}'
curl -H'Content-Type: application/json' -XPUT http://172.20.0.4:9200/test_index/_doc/2?pretty -d'{
    "title": "OPPO R15",
    "description": "新品到货",
    "price": 2000
}'
```
- URL 中的 1 和 2 是文档的 ID，这点和 Mysql 不太一样，Elasticsearch 的文档 ID 不是自增的，需要我们手动指定


## laravel 中如何使用 
#### 参考：*https://learnku.com/courses/ecommerce-advance/8.x/create-commodity-index/10782*
### 初始化

- 引入composer包 <br> `composer require elasticsearch/elasticsearch '~7.0'`
- 配置es主机地址<br>
    ~~~php
        // .env
        ES_HOSTS=localhost // 我的本地docker配置则是es7-17

        // config/database.php
        'elasticsearch' => [
                // Elasticsearch 支持多台服务器负载均衡，因此这里是一个数组
                'hosts' => explode(',', env('ES_HOSTS')),
            ]
    ~~~
- 初始化es单例模式<br>
**app/Providers/AppServiceProvider.php**
    ~~~php
        use Elasticsearch\ClientBuilder as ESClientBuilder;
        .
        .
        public function register()
        {
            // 注册一个名为 es 的单例
            $this->app->singleton('es', function () {
                // 从配置文件读取 Elasticsearch 服务器列表
                $builder = ESClientBuilder::create()->setHosts(config('database.elasticsearch.hosts'));
                // 如果是开发环境
                if (app()->environment() === 'local') {
                    // 配置日志，Elasticsearch 的请求和返回数据将打印到日志文件中，方便我们调试
                    $builder->setLogger(app('log')->driver());
                }

                return $builder->build();
            });
        }
    ~~~

### 开始使用
- 创建索引 <br>
`curl -XPUT http://localhost:9200/products?pretty`
- 创建类型
curl -H'Content-Type: application/json' -XPUT http://172.20.0.4:9200/products/_mapping/?pretty -d'{
  "properties": {
    "type": { "type": "keyword" } ,
    "title": { "type": "text", "analyzer": "ik_smart" }, 
    "long_title": { "type": "text", "analyzer": "ik_smart" }, 
    "category_id": { "type": "integer" },
    "category": { "type": "keyword" },
    "category_path": { "type": "keyword" },
    "description": { "type": "text", "analyzer": "ik_smart" },
    "price": { "type": "scaled_float", "scaling_factor": 100 },
    "on_sale": { "type": "boolean" },
    "rating": { "type": "float" },
    "sold_count": { "type": "integer" },
    "review_count": { "type": "integer" },
    "skus": {
      "type": "nested",
      "properties": {
        "title": { "type": "text", "analyzer": "ik_smart" }, 
        "description": { "type": "text", "analyzer": "ik_smart" },
        "price": { "type": "scaled_float", "scaling_factor": 100 }
      }
    },
    "properties": {
      "type": "nested",
      "properties": {
        "name": { "type": "keyword" }, 
        "value": { "type": "keyword" }
      }
    }
  }
}'

- 创建文档
    ~~~php
        // $arr 是一条数据的数组
        // 给 products 表添加一条记录
        app('es')->index(['id' => $arr['id'], 'index' => 'products', 'body' => $arr]);
    ~~
- 查询文档 <br>
查询 test_index表中的id=1的记录
    ~~~php
        // 生成此url：http://172.20.0.4/test_index/_doc/1
        app('es')->get(['index' => 'test_index', 'id' => 1])
    ~~~

- 查询所以的数据条数 <br>
`curl http://localhost:9200/products/_doc/_count?pretty`







curl -H'Content-Type: application/json' -XPUT http://172.20.0.7:9200/products/_mapping?pretty -d'{
  "properties": {
    "type": { "type": "keyword" } ,
    "title": { "type": "text", "analyzer": "ik_smart" }, 
    "long_title": { "type": "text", "analyzer": "ik_smart" }, 
    "category_id": { "type": "integer" },
    "category": { "type": "keyword" },
    "category_path": { "type": "keyword" },
    "description": { "type": "text", "analyzer": "ik_smart" },
    "price": { "type": "scaled_float", "scaling_factor": 100 },
    "on_sale": { "type": "boolean" },
    "rating": { "type": "float" },
    "sold_count": { "type": "integer" },
    "review_count": { "type": "integer" },
    "skus": {
      "type": "nested",
      "properties": {
        "title": { "type": "text", "analyzer": "ik_smart", "copy_to": "skus_title" }, 
        "description": { "type": "text", "analyzer": "ik_smart", "copy_to": "skus_description" },
        "price": { "type": "scaled_float", "scaling_factor": 100 }
      }
    },
    "properties": {
      "type": "nested",
      "properties": {
        "name": { "type": "keyword" }, 
        "value": { "type": "keyword", "copy_to": "properties_value" }
      }
    }
  }
}'



$params = [
    'index' => 'products',
    'body'  => [
        'query' => [
            'bool' => [
                'filter' => [
                    ['term' => ['on_sale' => true]],
                ],
                'must' => [
                    [
                        'multi_match' => [
                            'query'  => 'XPG单条',
                            'fields' => [
                                'skus_title',
                                'skus_description',
                                'properties_value',
                            ],
                        ],
                    ],
                ],
            ],
        ],
    ],
];
app('es')->search($params);