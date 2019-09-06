<h1 align="center"> eolinker-doc </h1>

<p align="center"> eolinker api doc generate tool.</p>


## Installing

```shell
$ composer require weiwei2012holy/eolinker-doc -vvv

# 发布配置
php artisan vendor:publish --provider="Weiwei2012holy\EolinkerDoc\ServiceProvider"

# 修改配置eolikner.php  配置默认账号和生成的文档项目id

# 运行接口生成工具
php artisan eolinker:create-doc     

```


## 使用说明

+ 使用[eolikner](https://eolinker.yidejia.com)作为api接口文档管理
+ 后台使用apidoc类型注释,通过命令可以一键生成文档


#### 一段简单的接口注释
```php

/**
 * @api            {get} api/home
 * @apiName        测试首页
 * @apiStatus      todo
 * @apiDescription 测试描述
 * @apiVersion     1.0.0
 * @apiPermission  token
 * @apiParam {integer} user_id 用户id
 * @apiSuccess {object} data={\App\WxaUser}
 * @apiSuccess {integer} data.new_name 名称
 */
public function home()
{
    $data = WxaUser::query()->first();
    $data->new_name = 'new name';
    return $data;
}

```

#### 参数说明


```bash
#创建api接口文档到eolikner
php artisan eolinker:create-doc               
```
注意事项:
> + 1.默认解析路由中,action位于`App\Http\Controllers`下的接口
> + 2.接口需要配置name,基本规则:`应用.模块.功能`,具体可以根据实际情况灵活调配,该类型的命名,对应了eolikner后台的分组关系
> + 3.参数目前只会修改和增加,不会输出原本的数据
> + 4.注释写法,参考[apidoc](http://apidocjs.com/#param-api-param)规范
> + 5.路由uri包含`api/admin`前缀,统一会再名称上面增加`后台`标记

#### 支持的apidoc标签

+ `@api {method} path [title]` ,默认会使用 `php artisan route:list`里面`name`,`path`,`method`
+ `@apiName name`,接口名称,有该值将会覆盖路由命名里面的值
+ `@apiDescription text` api接口详细描述或者写一些其他说明
+ `@apiParam [(group)] [{type}] [field=defaultValue] [description]` ,有效值:类型,字段名,描述,是否可选,默认值
+ `@apiSuccess [(group)] [{type}] [field=defaultValue] [description]` 返回值定义
+ `@apiParam`和`@apiSuccess`中的**`defaultValue`** ,如果格式为:`defaultValue={FuleNameOfModel@field1,field2}`,将会获取model对应mysql数据库字段描述信息最为返回值,`FuleNameOfModel`为model完整的名称,包含完整的命名空间,`@`后接字段名称,可以过来过滤字段返回(可选)
    1. 在模型中 `use EolinkerDoc\Traits\ModelInfo`,可以重写其中的`getTableFullColumnsCustom`方法实现覆盖源字段和增加数据库不存在字段
    2. todo 分表模型需要处理
    3. 注释示例:
        ```php
        * @apiSuccess {object[]} data={\App\Models\WxaMallLiving\WxaLivingComment} 评论数据
        * @apiParam {object[]} data={\App\Models\WxaMallLiving\WxBadWord} 
        ```
+ `@apiStatus` 标记接口状态: 接口正常提供服务:`[working,on]`,接口维护中:`[maintain,todo]`,接口已废弃:`[deprecated,down]`,默认接口状态为正常




TODO

## Contributing

You can contribute in one of three ways:

1. File bug reports using the [issue tracker](https://github.com/vendor/eolinker-doc/issues).
2. Answer questions or fix bugs on the [issue tracker](https://github.com/vendor/eolinker-doc/issues).
3. Contribute new features or update the wiki.

_The code contribution process is not very formal. You just need to make sure that you follow the PSR-0, PSR-1, and PSR-2 coding guidelines. Any new code contributions must be accompanied by unit tests where applicable._

## License

MIT