json11
------

json11是一个轻量级的C++11库, 提供JSON的序列化和反序列化功能.

核心的对象是 `json11::Json`. 可以用来表示任意类型的JSON数据：`null`, `bool`, `number (int or double)`, `string (std::string)`, `array (std::vector)`, 或者`object (std::map)`.

`json11::Json`类型的对象和其他值类型一样，支持赋值、拷贝、传递、比较等操作. 我们还提供了辅助方法
- `Json::dump`用来将`json11::Json`类型的对象序列化为string
- `Json::parse (static)`用来将std::string反序列化为`json11::Json`类型的对象

使用C++11提供的初始化器可以很容易创建一个`json11::Json`对象:

```c++
Json my_json = Json::object {
    { "key1", "value1" },
    { "key2", false },
    { "key3", Json::array { 1, 2, 3 } },
};
std::string json_str = my_json.dump();
```

这里还提供了一些内置的构造函数，可以将标准库类型和用户自定义类型自动的转化为`json11::Json`对象。例如:

```c++
class Point {
public:
    int x;
    int y;
    Point (int x, int y) : x(x), y(y) {}
    Json to_json() const { return Json::array { x, y }; }
};

std::vector<Point> points = { { 1, 2 }, { 10, 20 }, { 100, 200 } };
std::string points_json = Json(points).dump();
```

`json11::Json`同时支持下标和关键字索引：

```c++
Json json = Json::array { Json::object { { "k", "v" } } };
std::string str = json[0]["k"].string_value();
```

未完待续。

更多内容请查看`json11.hpp`。
