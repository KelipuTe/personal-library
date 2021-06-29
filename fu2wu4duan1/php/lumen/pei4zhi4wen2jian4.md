## lumen配置文件

- 文档更新时间：2021-07-07
- "laravel/lumen-framework":"v5.6.4"

### 使用laravel的配置方式

想在lumen中如果想使用laravel的配置方式，可以把lumen源代码中laravel的配置文件复制到config目录下。配置文件可以在`vendor\laravel\lumen-framework\config`目录下找到。

例如：如果在lumen中想同时连接两个数据库。这个时候需要把vendor的config目录下的database.php配置文件，复制到config目录下。使用方式和在laravel中的使用方式是一样的。

注意，在laravel中，我们可以直接使用config()读取配置在config目录下的自定义配置文件。但是在lumen中，在新建config目录并创建配置文件（如：config\message.php）后，需要在bootstrap/app.php中手动加载：`$app->configure('message')`。

### 在laravel中同时连接两个数据库

首先配置database.php，增加一个mysql链接。

```php
'mysql2' => [
  'driver'    => 'mysql',
  'host'      => 'host2',
  'database'  => 'database2',
  'username'  => 'user2',
  'password'  => 'password2',
  'charset'   => 'utf8mb4',
  'collation' => 'utf8mb4_unicode_ci',
  'prefix'    => '',
]
```

配置完成后，就可以在程序中使用mysql2链接另外一个数据库了。

```php
//在使用数据库迁移时（migration），指定数据表使用的链接
Schema::connection('mysql2')->create('some_table', function($table) {
  $table->increments('id');
});

//在使用查询构造器时，指定数据库连接
DB::connection('mysql2')->select(...);

//在模型中，指定模型数据库连接
class UserModel extends Eloquent
{
  protected $connection = 'mysql2';
}

//模型也可以实例化之后，再指定数据库连接
$userModel = new UserModel();
$userModel->setConnection('mysql2');
$user = $userModel->find(1);
//on()方法也是可以的
$userModel->on('mysql2')->find(1); 
```

