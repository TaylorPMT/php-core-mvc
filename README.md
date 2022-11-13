# Welcome to the PHP MVC framework

Đây là một khung MVC đơn giản để xây dựng các ứng dụng web bằng PHP. Nó miễn phí và [mã nguồn mở](GIẤY PHÉP).


## Starting an application using this framework

1.
1. Run **composer update**đ ể cài đặt các phụ thuộc của dự án.

1.Định cấu hình máy chủ web của bạn để có thư mục ** public ** làm thư mục gốc của web.
1.Mở [App / Config.php] (App / Config.php) và nhập dữ liệu cấu hình cơ sở dữ liệu của bạn.

1. Tạo routes, add controllers, views and models.

Xem dưới đây để biết thêm chi tiết.

## Configuration

Cài đặt cấu hình được lưu trữ trong lớp [App / Config.php] (App / Config.php). Cài đặt mặc định bao gồm dữ liệu kết nối cơ sở dữ liệu và cài đặt để hiển thị hoặc ẩn chi tiết lỗi. Bạn có thể truy cập các cài đặt trong mã của mình như sau: `Config :: DB_HOST`. Bạn có thể thêm cài đặt cấu hình của riêng mình tại đây.

## Routing

[Router](Core/Router.php) dịch URL thành bộ điều khiển và hành động. Các tuyến được thêm vào [Frontend Controller](public/index.php). Ví dụ router home được includes routers đến `index` trong [Home Controller](App/Controllers/Home.php).

Routes bằng phương thức `add`. Bạn có thể thêm các tuyến URL cố định và chỉ định bộ điều khiển và hành động, như sau:

```php
$router->add('', ['controller' => 'Home', 'action' => 'index']);
$router->add('posts/index', ['controller' => 'Posts', 'action' => 'index']);
```

Hoặc bạn có thể routes **variables**, như thế này:

```php
$router->add('{controller}/{action}');
```

Ngoài ra trong **controller** and **action**,bạn có thể chỉ định bất kỳ tham số nào bạn thích trong dấu ngoặc nhọn và cũng có thể chỉ định biểu thức chính quy tùy chỉnh cho tham số đó:

```php
$router->add('{controller}/{id:\d+}/{action}');
```

Có thể định nghĩa namespace cho controller:

```php
$router->add('admin/{controller}/{action}', ['namespace' => 'Admin']);
```

## Controllers

Controllers respond hành động của users (clicking on a link, submitting a form etc.). Controllers là class được kế thừa (extend) từ [Core\Controller](Core/Controller.php) class.

Controllers được lưu trữ ở `App/Controllers` folder. Một ví dụ [Home controller](App/Controllers/Home.php) bao gồm . Controller classes cần phải ở trong thư mục `App/Controllers` namespace.Có thể add thêm subdirectories cho thư mục controllers, vì vậy khi thêm một route hoặc controller cần phải chỉ định tên namespace (Xem phần routing ở bên trên).

Controller classes chứa các phương thức là các hành động .Để tạo một hành động , Thêm một **`Action`** suffix cho method name. Ví dụ controller trong [App/Controllers/Home.php](App/Controllers/Home.php) có một ví dụ `index` hành động.

Có thể truy cập route parameters (Ví dụ **id** parameter được hiển thị trong các ví dụ routes ở trên ) trong các parameters thông qua thuộc tính `$this->route_params` .

### Action filters

Controllers có thể **before** và **after** filter methods. Đây là các phương thức được gọi trước qua sau **every** action method call trong một controller. Ví dụ, hữu ích cho việc xác thực, đảm bảo rằng người dùng đã đăng nhập trước khi cho phép họ thực hiện một hành động. Tùy chọn thêm một
**before filter** đến một controller như thế này:

```php
/**
 * Before filter. Return false to stop the action from executing.
 *
 * @return void
 */
protected function before()
{
}
```

Để ngăn hành động được gọi ban đầu thực thi, hãy trả về return `false` từ the before filter method. Một **after filter** được thêm vào như thế này:

```php
/**
 * After filter.
 *
 * @return void
 */
protected function after()
{
}
```

## Views

Views are used to display information (normally HTML). View files go in the `App/Views` folder. Views can be in one of two formats: standard PHP, but with just enough PHP to show the data. No database access or anything like that should occur in a view file. You can render a standard PHP view in a controller, optionally passing in variables, like this:

```php
View::render('Home/index.php', [
  'name' => 'Dave',
  'colours' => ['red', 'green', 'blue'],
]);
```

The second format uses the [Twig](http://twig.sensiolabs.org/) templating engine. Using Twig allows you to have simpler, safer templates that can take advantage of things like [template inheritance](http://twig.sensiolabs.org/doc/templates.html#template-inheritance). You can render a Twig template like this:

```php
View::renderTemplate('Home/index.html', [
  'name' => 'Dave',
  'colours' => ['red', 'green', 'blue'],
]);
```

A sample Twig template is included in [App/Views/Home/index.html](App/Views/Home/index.html) that inherits from the base template in [App/Views/base.html](App/Views/base.html).

## Models

Models are used to get and store data in your application. They know nothing about how this data is to be presented in the views. Models extend the `Core\Model` class and use [PDO](http://php.net/manual/en/book.pdo.php) to access the database. They're stored in the `App/Models` folder. A sample user model class is included in [App/Models/User.php](App/Models/User.php). You can get the PDO database connection instance like this:

```php
$db = static::getDB();
```

## Errors

If the `SHOW_ERRORS` configuration setting is set to `true`, full error detail will be shown in the browser if an error or exception occurs. If it's set to `false`, a generic message will be shown using the [App/Views/404.html](App/Views/404.html) or [App/Views/500.html](App/Views/500.html) views, depending on the error.

## Web server configuration

Pretty URLs are enabled using web server rewrite rules. An [.htaccess](public/.htaccess) file is included in the `public` folder. Equivalent nginx configuration is in the [nginx-configuration.txt](nginx-configuration.txt) file.

---

Signup for the course [here](https://davehollingworth.net/phpmvcg) and understand how this framework is built from scratch, putting it all together step by step.
